# TqSdk 策略示例 - 趋势策略 (Part 4)

**包含策略:** Qstick Trend Strategy, Aroon Indicator Strategy

---

### 7. Qstick Trend Strategy (Qstick趋势策略)

Analyzes price differences between closing and opening prices to identify market trends and momentum by calculating a moving average of these differences over a specific period. Uses zero-line crossovers for entry/exit signals.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
import pandas as pd
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from tqsdk.ta import ATR

# ===== 全局参数设置 =====
SYMBOL = "CFFEX.IC2303"  # 中证500指数期货
POSITION_SIZE = 30  # 持仓手数
START_DATE = date(2022, 8, 1)  # 回测开始日期
END_DATE = date(2023, 1, 30)  # 回测结束日期

# Qstick指标参数
QSTICK_PERIOD = 10  # Qstick周期
SMA_PERIOD = 8  # 价格SMA周期，用于确认趋势

# 风控参数
ATR_PERIOD = 14  # ATR计算周期
STOP_LOSS_MULTIPLIER = 2.0  # 止损ATR倍数
MAX_HOLDING_DAYS = 5  # 最大持仓天数

# ===== 全局变量 =====
current_direction = 0  # 当前持仓方向：1=多头，-1=空头，0=空仓
entry_price = 0  # 开仓价格
stop_loss_price = 0  # 止损价格
entry_date = None  # 开仓日期


# ===== Qstick指标计算函数 =====
def calculate_qstick(open_prices, close_prices, period):
    """计算Qstick指标"""
    # 计算收盘价与开盘价的差值
    diff = close_prices - open_prices
    # 计算移动平均
    qstick = diff.rolling(window=period).mean()
    return qstick


# ===== 策略开始 =====
print("开始运行Qstick趋势指标期货策略...")

# 创建API实例
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 订阅合约的K线数据
klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)  # 日线数据

# 创建目标持仓任务
target_pos = TargetPosTask(api, SYMBOL)

try:
    while True:
        # 等待更新
        api.wait_update()

        # 如果K线有更新
        if api.is_changing(klines.iloc[-1], "datetime"):
            # 确保有足够的数据计算指标
            if len(klines) < max(QSTICK_PERIOD, SMA_PERIOD, ATR_PERIOD) + 5:
                continue

            # 计算Qstick指标
            klines['qstick'] = calculate_qstick(klines.open, klines.close, QSTICK_PERIOD)

            # 计算价格SMA，用于确认趋势
            klines['price_sma'] = klines.close.rolling(window=SMA_PERIOD).mean()

            # 计算ATR用于设置止损
            atr_data = ATR(klines, ATR_PERIOD)

            # 获取最新数据
            current_price = float(klines.close.iloc[-1])
            current_datetime = pd.to_datetime(klines.datetime.iloc[-1], unit='ns')
            current_qstick = float(klines.qstick.iloc[-1])
            previous_qstick = float(klines.qstick.iloc[-2])
            current_price_sma = float(klines.price_sma.iloc[-1])
            current_atr = float(atr_data.atr.iloc[-1])

            # 输出调试信息
            print(f"日期: {current_datetime.strftime('%Y-%m-%d')}, 价格: {current_price:.2f}")
            print(f"Qstick: {current_qstick:.4f}")

            # ===== 交易逻辑 =====

            # 空仓状态 - 寻找开仓机会
            if current_direction == 0:
                # 多头开仓条件：Qstick从负值向上穿越零轴，价格在SMA上方
                if previous_qstick < 0 and current_qstick > 0 and current_price > current_price_sma:
                    current_direction = 1
                    target_pos.set_target_volume(POSITION_SIZE)
                    entry_price = current_price
                    entry_date = current_datetime

                    # 设置止损价格
                    stop_loss_price = entry_price - STOP_LOSS_MULTIPLIER * current_atr

                    print(f"多头开仓: 价格={entry_price}, 止损={stop_loss_price:.2f}")

                # 空头开仓条件：Qstick从正值向下穿越零轴，价格在SMA下方
                elif previous_qstick > 0 and current_qstick < 0 and current_price < current_price_sma:
                    current_direction = -1
                    target_pos.set_target_volume(-POSITION_SIZE)
                    entry_price = current_price
                    entry_date = current_datetime

                    # 设置止损价格
                    stop_loss_price = entry_price + STOP_LOSS_MULTIPLIER * current_atr

                    print(f"空头开仓: 价格={entry_price}, 止损={stop_loss_price:.2f}")

            # 多头持仓 - 检查平仓条件
            elif current_direction == 1:
                # 计算持仓天数
                holding_days = (current_datetime - entry_date).days

                # 1. 止损条件：价格低于止损价格
                if current_price <= stop_loss_price:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止损平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%, 持仓天数={holding_days}")

                # 2. 反向信号平仓：Qstick从正值向下穿越零轴
                elif previous_qstick > 0 and current_qstick < 0:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头信号平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%, 持仓天数={holding_days}")

                # 3. 时间止损：持仓时间过长
                elif holding_days >= MAX_HOLDING_DAYS:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头时间平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%, 持仓天数={holding_days}")

            # 空头持仓 - 检查平仓条件
            elif current_direction == -1:
                # 计算持仓天数
                holding_days = (current_datetime - entry_date).days

                # 1. 止损条件：价格高于止损价格
                if current_price >= stop_loss_price:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止损平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%, 持仓天数={holding_days}")

                # 2. 反向信号平仓：Qstick从负值向上穿越零轴
                elif previous_qstick < 0 and current_qstick > 0:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头信号平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%, 持仓天数={holding_days}")

                # 3. 时间止损：持仓时间过长
                elif holding_days >= MAX_HOLDING_DAYS:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头时间平仓: 价格={current_price}, 盈亏={profit_pct:.2f}%, 持仓天数={holding_days}")

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 8. Aroon Indicator Strategy (Aroon指标趋势交易策略)

Measures time elapsed since highest/lowest prices formed within a period. Generates trading signals through Aroon Up/Down line crossovers and extreme readings (above 75 or below 25) to identify trend formation, strength, and potential reversals.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
import pandas as pd
from datetime import date

# ===== 全局参数设置 =====
SYMBOL = "SHFE.au2306"  # 黄金期货合约
POSITION_SIZE = 30  # 持仓手数
START_DATE = date(2023, 2, 20)  # 回测开始日期
END_DATE = date(2023, 5, 5)  # 回测结束日期

# Aroon指标参数
AROON_PERIOD = 10  # Aroon计算周期
AROON_UPPER_THRESHOLD = 75  # Aroon上线阈值
AROON_LOWER_THRESHOLD = 25  # Aroon下线阈值

# 风控参数
STOP_LOSS_PCT = 1.2  # 止损百分比

# ===== 全局变量 =====
position = 0  # 当前持仓
entry_price = 0  # 入场价格
trades = []  # 交易记录

# ===== 主程序 =====
print(f"开始回测 {SYMBOL} 的Aroon指标策略...")
print(f"参数: Aroon周期={AROON_PERIOD}, 上阈值={AROON_UPPER_THRESHOLD}, 下阈值={AROON_LOWER_THRESHOLD}")
print(f"回测期间: {START_DATE} 至 {END_DATE}")

try:
    # 创建API实例
    api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
                auth=TqAuth("快期账号", "快期密码"))

    # 订阅K线数据
    klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)  # 日K线
    target_pos = TargetPosTask(api, SYMBOL)

    # 主循环
    while True:
        api.wait_update()

        if api.is_changing(klines.iloc[-1], "datetime"):
            # 确保有足够的数据
            if len(klines) < AROON_PERIOD + 10:
                continue
            
            # ===== 计算Aroon指标 =====
            # 找出最近N周期内最高价和最低价的位置
            klines['rolling_high'] = klines['high'].rolling(window=AROON_PERIOD).max()
            klines['rolling_low'] = klines['low'].rolling(window=AROON_PERIOD).min()

            # 初始化Aroon Up和Aroon Down数组
            aroon_up = []
            aroon_down = []

            # 遍历计算每个时间点的Aroon值
            for i in range(len(klines)):
                if i < AROON_PERIOD - 1:
                    aroon_up.append(0)
                    aroon_down.append(0)
                    continue
                
                period_data = klines.iloc[i-AROON_PERIOD+1:i+1]
                # 明确指定skipna=True来处理NaN值
                high_idx = period_data['high'].fillna(float('-inf')).argmax()
                low_idx = period_data['low'].fillna(float('inf')).argmin()
                
                days_since_high = i - (i-AROON_PERIOD+1+high_idx)
                days_since_low = i - (i-AROON_PERIOD+1+low_idx)
                
                aroon_up.append(((AROON_PERIOD - days_since_high) / AROON_PERIOD) * 100)
                aroon_down.append(((AROON_PERIOD - days_since_low) / AROON_PERIOD) * 100)

            # 将Aroon值添加到klines
            klines['aroon_up'] = aroon_up
            klines['aroon_down'] = aroon_down

            # 计算Aroon Oscillator (可选)
            klines['aroon_osc'] = klines['aroon_up'] - klines['aroon_down']

            # 获取当前和前一个周期的数据
            current_price = float(klines.close.iloc[-1])
            current_time = pd.to_datetime(klines.datetime.iloc[-1], unit='ns')
            current_aroon_up = float(klines.aroon_up.iloc[-1])
            current_aroon_down = float(klines.aroon_down.iloc[-1])
            
            prev_aroon_up = float(klines.aroon_up.iloc[-2])
            prev_aroon_down = float(klines.aroon_down.iloc[-2])

            # 输出当前指标值，帮助调试
            print(f"当前K线: {current_time.strftime('%Y-%m-%d')}, 价格: {current_price:.2f}")
            print(f"Aroon Up: {current_aroon_up:.2f}, Aroon Down: {current_aroon_down:.2f}")

            # ===== 止损检查 =====
            if position != 0 and entry_price != 0:
                if position > 0:  # 多头止损
                    profit_pct = (current_price / entry_price - 1) * 100
                    if profit_pct < -STOP_LOSS_PCT:
                        print(f"触发止损: 当前价格={current_price}, 入场价={entry_price}, 亏损={profit_pct:.2f}%")
                        target_pos.set_target_volume(0)
                        trades.append({
                            'type': '止损平多',
                            'time': current_time,
                            'price': current_price,
                            'profit_pct': profit_pct
                        })
                        print(f"止损平多: {current_time}, 价格: {current_price:.2f}, 亏损: {profit_pct:.2f}%")
                        position = 0
                        entry_price = 0
                        continue

                elif position < 0:  # 空头止损
                    profit_pct = (entry_price / current_price - 1) * 100
                    if profit_pct < -STOP_LOSS_PCT:
                        print(f"触发止损: 当前价格={current_price}, 入场价={entry_price}, 亏损={profit_pct:.2f}%")
                        target_pos.set_target_volume(0)
                        trades.append({
                            'type': '止损平空',
                            'time': current_time,
                            'price': current_price,
                            'profit_pct': profit_pct
                        })
                        print(f"止损平空: {current_time}, 价格: {current_price:.2f}, 亏损: {profit_pct:.2f}%")
                        position = 0
                        entry_price = 0
                        continue

            # ===== 交易信号判断 =====
            # 1. Aroon交叉信号
            aroon_cross_up = prev_aroon_up < prev_aroon_down and current_aroon_up > current_aroon_down
            aroon_cross_down = prev_aroon_up > prev_aroon_down and current_aroon_up < current_aroon_down

            # 2. 强势信号
            strong_up = current_aroon_up > AROON_UPPER_THRESHOLD and current_aroon_down < AROON_LOWER_THRESHOLD
            strong_down = current_aroon_down > AROON_UPPER_THRESHOLD and current_aroon_up < AROON_LOWER_THRESHOLD

            # ===== 交易决策 =====
            if position == 0:  # 空仓状态
                # 多头信号
                if aroon_cross_up or strong_up:
                    position = POSITION_SIZE
                    entry_price = current_price
                    target_pos.set_target_volume(position)
                    signal_type = "交叉" if aroon_cross_up else "强势"
                    trades.append({
                        'type': '开多',
                        'time': current_time,
                        'price': current_price,
                        'signal': signal_type
                    })
                    print(f"开多仓: {current_time}, 价格: {current_price:.2f}, 信号: Aroon {signal_type}")

                # 空头信号
                elif aroon_cross_down or strong_down:
                    position = -POSITION_SIZE
                    entry_price = current_price
                    target_pos.set_target_volume(position)
                    signal_type = "交叉" if aroon_cross_down else "强势"
                    trades.append({
                        'type': '开空',
                        'time': current_time,
                        'price': current_price,
                        'signal': signal_type
                    })
                    print(f"开空仓: {current_time}, 价格: {current_price:.2f}, 信号: Aroon {signal_type}")

            elif position > 0:  # 持有多头
                # 平多信号
                if aroon_cross_down:
                    profit_pct = (current_price / entry_price - 1) * 100
                    target_pos.set_target_volume(0)
                    trades.append({
                        'type': '平多',
                        'time': current_time,
                        'price': current_price,
                        'profit_pct': profit_pct
                    })
                    print(f"平多仓: {current_time}, 价格: {current_price:.2f}, 盈亏: {profit_pct:.2f}%")
                    position = 0
                    entry_price = 0

            elif position < 0:  # 持有空头
                # 平空信号
                if aroon_cross_up:
                    profit_pct = (entry_price / current_price - 1) * 100
                    target_pos.set_target_volume(0)
                    trades.append({
                        'type': '平空',
                        'time': current_time,
                        'price': current_price,
                        'profit_pct': profit_pct
                    })
                    print(f"平空仓: {current_time}, 价格: {current_price:.2f}, 盈亏: {profit_pct:.2f}%")
                    position = 0
                    entry_price = 0

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

## Summary

All 8 strategies were successfully scraped from shinnytech.com. They are all built on the **TianQin SDK (tqsdk)** platform for futures backtesting and share a common architectural pattern:

| # | Strategy | Chinese Name | Contract | Timeframe |
|---|----------|-------------|----------|-----------|
| 1 | TRIX | 三重指数平滑平均线趋势策略 | CFFEX.IC2306 | Daily |
| 2 | Fractal Breakout | 基于分形的趋势突破策略 | SHFE.au2306 | 1-Hour |
| 3 | CMO | 钱德动量震荡指标策略 | DCE.c2309 | Daily |
| 4 | Vortex Indicator | 涡旋指标期货量化策略 | CFFEX.IC2306 | Daily |
| 5 | Hull MA | Hull移动平均线策略 | SHFE.au2306 | Daily |
| 6 | Keltner Channel | Keltner Channel策略 | CFFEX.IC2306 | Daily |
| 7 | Qstick | Qstick趋势策略 | CFFEX.IC2303 | Daily |
| 8 | Aroon | Aroon指标趋势交易策略 | SHFE.au2306 | Daily |