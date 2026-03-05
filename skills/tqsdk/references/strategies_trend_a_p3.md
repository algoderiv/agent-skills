# TqSdk 策略示例 - 趋势策略 (Part 3)

**包含策略:** Hull Moving Average Strategy, Keltner Channel Strategy

---

### 5. Hull Moving Average Strategy (Hull移动平均线策略)

A specialized moving average designed to reduce lag while maintaining smoothness, enabling faster and more accurate identification of market trends and turning points through weighted calculations and square root functions applied to price data.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date

import numpy as np
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from tqsdk.ta import ATR

# ===== 全局参数设置 =====
SYMBOL = "SHFE.au2306"  # 黄金期货合约
POSITION_SIZE = 30  # 单次交易手数
START_DATE = date(2022, 11, 1)  # 回测开始日期
END_DATE = date(2023, 4, 1)  # 回测结束日期

# Hull Moving Average 参数
LONG_HMA_PERIOD = 30  # 长周期HMA，用于确定大趋势
SHORT_HMA_PERIOD = 5  # 短周期HMA，用于入场信号

# 止损止盈参数
ATR_PERIOD = 14  # ATR计算周期
STOP_LOSS_ATR = 2.0  # 止损为入场点的2倍ATR
TAKE_PROFIT_ATR = 4.0  # 获利为入场点的4倍ATR
TRAILING_STOP = True  # 使用移动止损

# 新增止盈参数
FIXED_TAKE_PROFIT_PCT = 0.03  # 固定止盈比例（3%）
USE_TRAILING_PROFIT = True  # 是否使用追踪止盈
TRAILING_PROFIT_THRESHOLD = 0.02  # 触发追踪止盈的收益率阈值（2%）
TRAILING_PROFIT_STEP = 0.005  # 追踪止盈回撤幅度（0.5%）

# ===== 全局变量 =====
current_direction = 0   # 当前持仓方向：1=多头，-1=空头，0=空仓
entry_price = 0         # 开仓价格
stop_loss_price = 0     # 止损价格
take_profit_price = 0   # 止盈价格
highest_since_entry = 0 # 入场后的最高价（用于多头追踪止盈）
lowest_since_entry = 0  # 入场后的最低价（用于空头追踪止盈）

# ===== Hull移动平均线相关函数 =====
def wma(series, period):
    """计算加权移动平均线"""
    weights = np.arange(1, period + 1)
    return series.rolling(period).apply(lambda x: np.sum(weights * x) / weights.sum(), raw=True)

def hma(series, period):
    """计算Hull移动平均线"""
    period = int(period)
    if period < 3:
        return series
    
    half_period = period // 2
    sqrt_period = int(np.sqrt(period))
    
    wma1 = wma(series, half_period)
    wma2 = wma(series, period)
    
    raw_hma = 2 * wma1 - wma2
    
    return wma(raw_hma, sqrt_period)

# ===== 策略开始 =====
print("开始运行Hull移动平均线期货策略...")

# 创建API实例
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 订阅合约的K线数据
klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)

# 创建目标持仓任务
target_pos = TargetPosTask(api, SYMBOL)

try:
    while True:
        # 等待更新
        api.wait_update()
        
        # 如果K线有更新
        if api.is_changing(klines.iloc[-1], "datetime"):
            # 确保有足够的数据计算指标
            if len(klines) < max(SHORT_HMA_PERIOD, LONG_HMA_PERIOD, ATR_PERIOD) + 10:
                continue
                
            # 计算短期和长期HMA
            klines['short_hma'] = hma(klines.close, SHORT_HMA_PERIOD)
            klines['long_hma'] = hma(klines.close, LONG_HMA_PERIOD)
            
            # 计算ATR用于止损设置
            atr_data = ATR(klines, ATR_PERIOD)
            
            # 获取最新数据
            current_price = float(klines.close.iloc[-1])
            current_short_hma = float(klines.short_hma.iloc[-1])
            current_long_hma = float(klines.long_hma.iloc[-1])
            current_atr = float(atr_data.atr.iloc[-1])
            
            # 获取前一个周期的数据
            prev_short_hma = float(klines.short_hma.iloc[-2])
            prev_long_hma = float(klines.long_hma.iloc[-2])
            
            # 输出调试信息
            print(f"价格: {current_price}, 短期HMA: {current_short_hma:.2f}, 长期HMA: {current_long_hma:.2f}")
            
            # ===== 交易逻辑 =====
            
            # 空仓状态 - 寻找开仓机会
            if current_direction == 0:
                # 多头开仓条件
                if (prev_short_hma <= prev_long_hma and current_short_hma > current_long_hma
                        and current_price > current_long_hma):
                    
                    print(f"多头开仓信号: 短期HMA上穿长期HMA")
                    
                    # 设置持仓和记录开仓价格
                    current_direction = 1
                    target_pos.set_target_volume(POSITION_SIZE)
                    entry_price = current_price
                    
                    # 设置止损价格
                    stop_loss_price = current_long_hma - STOP_LOSS_ATR * current_atr
                    
                    # 设置止盈价格
                    take_profit_price = entry_price * (1 + FIXED_TAKE_PROFIT_PCT)
                    
                    # 重置追踪止盈变量
                    highest_since_entry = entry_price
                    
                    print(f"多头开仓价格: {entry_price}, 止损: {stop_loss_price:.2f}, 止盈: {take_profit_price:.2f}")
                
                # 空头开仓条件
                elif (prev_short_hma >= prev_long_hma and current_short_hma < current_long_hma
                      and current_price < current_long_hma):
                    
                    print(f"空头开仓信号: 短期HMA下穿长期HMA")
                    
                    # 设置持仓和记录开仓价格
                    current_direction = -1
                    target_pos.set_target_volume(-POSITION_SIZE)
                    entry_price = current_price
                    
                    # 设置止损价格
                    stop_loss_price = current_long_hma + STOP_LOSS_ATR * current_atr
                    
                    # 设置止盈价格
                    take_profit_price = entry_price * (1 - FIXED_TAKE_PROFIT_PCT)
                    
                    # 重置追踪止盈变量
                    lowest_since_entry = entry_price
                    
                    print(f"空头开仓价格: {entry_price}, 止损: {stop_loss_price:.2f}, 止盈: {take_profit_price:.2f}")
            
            # 多头持仓 - 检查平仓条件
            elif current_direction == 1:
                # 更新入场后的最高价
                if current_price > highest_since_entry:
                    highest_since_entry = current_price
                
                # 计算固定止损
                fixed_stop_loss = entry_price * (1 - STOP_LOSS_ATR)
                
                # 更新追踪止损（只上移不下移）
                new_stop = current_short_hma - STOP_LOSS_ATR * current_atr
                if new_stop > stop_loss_price:
                    stop_loss_price = new_stop
                    print(f"更新多头止损: {stop_loss_price:.2f}")
                
                # 平仓条件1: 短期HMA下穿长期HMA
                if prev_short_hma >= prev_long_hma and current_short_hma < current_long_hma:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%")
                
                # 平仓条件2: 价格跌破止损
                elif current_price < stop_loss_price or current_price < fixed_stop_loss:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止损: 价格={current_price}, 盈亏={profit_pct:.2f}%")
                
                # 平仓条件3: 价格达到止盈价格
                elif current_price >= take_profit_price:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止盈: 价格={current_price}, 盈亏={profit_pct:.2f}%")
            
            # 空头持仓 - 检查平仓条件
            elif current_direction == -1:
                # 更新入场后的最低价
                if current_price < lowest_since_entry or lowest_since_entry == 0:
                    lowest_since_entry = current_price
                
                # 计算固定止损
                fixed_stop_loss = entry_price * (1 + STOP_LOSS_ATR)
                
                # 更新追踪止损（只下移不上移）
                new_stop = current_short_hma + STOP_LOSS_ATR * current_atr
                if new_stop < stop_loss_price or stop_loss_price == 0:
                    stop_loss_price = new_stop
                    print(f"更新空头止损: {stop_loss_price:.2f}")
                
                # 平仓条件1: 短期HMA上穿长期HMA
                if prev_short_hma <= prev_long_hma and current_short_hma > current_long_hma:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%")
                
                # 平仓条件2: 价格突破止损
                elif current_price > stop_loss_price or current_price > fixed_stop_loss:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止损: 价格={current_price}, 盈亏={profit_pct:.2f}%")
                
                # 平仓条件3: 价格达到止盈价格
                elif current_price <= take_profit_price:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止盈: 价格={current_price}, 盈亏={profit_pct:.2f}%")

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 6. Keltner Channel Strategy (Keltner Channel策略)

A trend-tracking system based on channel breakouts that trades when price breaks through channels composed of EMA and ATR-based volatility bands. Includes dynamic ATR multiplier adjustment based on trend strength and trailing stop-loss.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from datetime import date
import numpy as np
import pandas as pd

# ===== Global Parameter Settings =====
SYMBOL = "CFFEX.IC2306"  # CSI 500 Index Futures Contract
POSITION_SIZE = 30  # Position size in lots
START_DATE = date(2022, 11, 1)  # Backtest start date
END_DATE = date(2023, 4, 30)  # Backtest end date

# Keltner Channel Parameters
EMA_PERIOD = 8  # EMA period
ATR_PERIOD = 7  # ATR period
ATR_MULTIPLIER = 1.5  # ATR multiplier

# Additional Parameters - Trend Confirmation & Stop Loss
SHORT_EMA_PERIOD = 5  # Short-term EMA for trend confirmation
STOP_LOSS_PCT = 0.8  # Stop loss percentage (relative to ATR)
TRAILING_STOP = True  # Use trailing stop

print(f"Starting backtest of {SYMBOL} with Keltner Channel strategy...")
print(f"Parameters: EMA period={EMA_PERIOD}, ATR period={ATR_PERIOD}, ATR multiplier={ATR_MULTIPLIER}")
print(f"Additional parameters: Short EMA={SHORT_EMA_PERIOD}, Stop loss={STOP_LOSS_PCT}ATR, Trailing stop={TRAILING_STOP}")

try:
    api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
                auth=TqAuth("fast_account", "fast_password"))

    # Subscribe to K-line data
    klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)  # Daily K-line
    # Subscribe to quotes to get trading time
    quote = api.get_quote(SYMBOL)
    target_pos = TargetPosTask(api, SYMBOL)

    # Initialize trading state
    position = 0  # Current position
    entry_price = 0  # Entry price
    stop_loss = 0  # Stop loss price
    high_since_entry = 0  # Highest price since entry (for trailing stop)
    low_since_entry = 0  # Lowest price since entry (for trailing stop)
    trend_strength = 0  # Trend strength

    # Record trade information
    trades = []

    while True:
        api.wait_update()

        if api.is_changing(klines):
            # Ensure sufficient data
            if len(klines) < max(EMA_PERIOD, ATR_PERIOD, SHORT_EMA_PERIOD) + 1:
                continue

            # Calculate indicators
            close = klines.close.values
            high = klines.high.values
            low = klines.low.values

            # Calculate midline (EMA) and short-term EMA (for trend confirmation)
            ema = pd.Series(close).ewm(span=EMA_PERIOD, adjust=False).mean().values
            ema_short = pd.Series(close).ewm(span=SHORT_EMA_PERIOD, adjust=False).mean().values

            # Calculate trend direction and strength
            trend_direction = 1 if ema_short[-1] > ema[-1] else -1 if ema_short[-1] < ema[-1] else 0
            trend_strength = abs(ema_short[-1] - ema[-1]) / close[-1] * 100  # Trend strength percentage

            # Calculate ATR
            tr = np.maximum(high - low,
                            np.maximum(
                                np.abs(high - np.roll(close, 1)),
                                np.abs(low - np.roll(close, 1))
                            ))
            atr = pd.Series(tr).rolling(ATR_PERIOD).mean().values
            current_atr = float(atr[-1])

            # Dynamically adjust ATR multiplier based on trend strength
            dynamic_multiplier = ATR_MULTIPLIER
            if trend_strength > 0.5:  # Use tighter channel in strong trends
                dynamic_multiplier = ATR_MULTIPLIER * 0.8

            # Calculate upper and lower bands
            upper_band = ema + dynamic_multiplier * atr
            lower_band = ema - dynamic_multiplier * atr

            # Get current price and indicator values
            current_price = float(close[-1])
            current_upper = float(upper_band[-1])
            current_lower = float(lower_band[-1])
            current_ema = float(ema[-1])
            current_time = quote.datetime  # Use quote's datetime for current time

            # Update highest/lowest price since entry
            if position > 0:
                high_since_entry = max(high_since_entry, current_price)
                # Update trailing stop
                if TRAILING_STOP and high_since_entry > entry_price:
                    trailing_stop = high_since_entry * (1 - STOP_LOSS_PCT * current_atr / current_price)
                    stop_loss = max(stop_loss, trailing_stop)
            elif position < 0:
                low_since_entry = min(low_since_entry, current_price)
                # Update trailing stop
                if TRAILING_STOP and low_since_entry < entry_price:
                    trailing_stop = low_since_entry * (1 + STOP_LOSS_PCT * current_atr / current_price)
                    stop_loss = min(stop_loss if stop_loss > 0 else float('inf'), trailing_stop)

            # Trading logic
            if position == 0:  # Flat
                # Confirm trend direction and break through channel
                if current_price > current_upper and trend_direction > 0:
                    # Add volume filter
                    position = POSITION_SIZE
                    entry_price = current_price
                    high_since_entry = current_price
                    low_since_entry = current_price
                    # Set initial stop loss
                    stop_loss = current_price * (1 - STOP_LOSS_PCT * current_atr / current_price)
                    target_pos.set_target_volume(position)
                    print(f"Open long: price={current_price:.2f}, upper_band={current_upper:.2f}, stop_loss={stop_loss:.2f}")
                    trades.append(("Open long", current_time, current_price))

                elif current_price < current_lower and trend_direction < 0:
                    position = -POSITION_SIZE
                    entry_price = current_price
                    high_since_entry = current_price
                    low_since_entry = current_price
                    # Set initial stop loss
                    stop_loss = current_price * (1 + STOP_LOSS_PCT * current_atr / current_price)
                    target_pos.set_target_volume(position)
                    print(f"Open short: price={current_price:.2f}, lower_band={current_lower:.2f}, stop_loss={stop_loss:.2f}")
                    trades.append(("Open short", current_time, current_price))

            elif position > 0:  # Long position
                # Close on stop loss, retreat to midline, or trend reversal
                if (current_price <= stop_loss or
                        current_price <= current_ema or
                        (current_price < current_lower and trend_direction < 0)):
                    profit_pct = (current_price / entry_price - 1) * 100
                    profit_points = current_price - entry_price
                    target_pos.set_target_volume(0)
                    print(f"Close long: price={current_price:.2f}, profit={profit_pct:.2f}%, {profit_points:.2f} points")
                    position = 0
                    entry_price = 0
                    stop_loss = 0
                    trades.append(("Close long", current_time, current_price))

            elif position < 0:  # Short position
                # Close on stop loss, rise to midline, or trend reversal
                if (current_price >= stop_loss or
                        current_price >= current_ema or
                        (current_price > current_upper and trend_direction > 0)):
                    profit_pct = (entry_price / current_price - 1) * 100
                    profit_points = entry_price - current_price
                    target_pos.set_target_volume(0)
                    print(f"Close short: price={current_price:.2f}, profit={profit_pct:.2f}%, {profit_points:.2f} points")
                    position = 0
                    entry_price = 0
                    stop_loss = 0
                    trades.append(("Close short", current_time, current_price))

except BacktestFinished as e:
    print(f"Backtest completed: {e}")
    api.close()
```

---