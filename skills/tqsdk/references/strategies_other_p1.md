# TqSdk 策略示例 - 均值回归与其他策略 (Part 1)

**包含策略:** Kalman Filter Pairs Trading Strategy, Distance-Based Pairs Trading Strategy, Pivot Point Reversal Strategy

---

### 1. Kalman Filter Pairs Trading Strategy (卡尔曼滤波配对交易策略)

Implements pairs trading using dynamic hedge ratios estimated via Kalman filtering. Tracks the relationship between two correlated futures contracts (stainless steel and nickel) and generates signals when their price spread deviates significantly from equilibrium, expecting mean reversion.

```python
import numpy as np
import pandas as pd
from tqsdk import TqApi, TqAuth, TargetPosTask, TqBacktest, BacktestFinished
from datetime import date

# === 全局参数 ===
SYMBOL_Y = "SHFE.ss2209"
SYMBOL_X = "SHFE.ni2209"
OBS_VAR = 0.01
STATE_VAR = 0.0001
INIT_MEAN = 1.0
INIT_VAR = 1.0
WIN = 60
OPEN_H = 2.0
OPEN_L = -2.0
CLOSE_H = 0.5
CLOSE_L = -0.5
STOP_SPREAD = 3.0
MAX_HOLD = 10
POS_PCT = 0.05
INIT_CAP = 10000000

# === 全局变量 ===
state_mean = INIT_MEAN
state_var = INIT_VAR
prices_y, prices_x, hedge_ratios, spreads, zscores = [], [], [], [], []
position = 0
entry_z = 0
entry_time = None
trade_count = 0
win_count = 0
total_profit = 0
total_loss = 0
hold_days = 0
last_day = None

# === API初始化 ===
api = TqApi(backtest=TqBacktest(start_dt=date(2022, 7, 4), end_dt=date(2022, 8, 31)),
                auth=TqAuth("快期账号", "快期密码"))
quote_y = api.get_quote(SYMBOL_Y)
quote_x = api.get_quote(SYMBOL_X)
klines_y = api.get_kline_serial(SYMBOL_Y, 60*60)
klines_x = api.get_kline_serial(SYMBOL_X, 60*60)
target_y = TargetPosTask(api, SYMBOL_Y)
target_x = TargetPosTask(api, SYMBOL_X)

try:
    while True:
        api.wait_update()
        if api.is_changing(klines_y.iloc[-1], "datetime") or api.is_changing(klines_x.iloc[-1], "datetime"):
            price_y = klines_y.iloc[-1]["close"]
            price_x = klines_x.iloc[-1]["close"]
            now = pd.to_datetime(klines_y.iloc[-1]["datetime"], unit="ns")
            today = now.date()
            if last_day and today != last_day and position != 0:
                hold_days += 1
            last_day = today
            prices_y.append(price_y)
            prices_x.append(price_x)
            if len(prices_y) > 10:
                # 卡尔曼滤波
                pred_mean = state_mean
                pred_var = state_var + STATE_VAR
                k_gain = pred_var / (pred_var * price_x**2 + OBS_VAR)
                state_mean = pred_mean + k_gain * (price_y - pred_mean * price_x)
                state_var = (1 - k_gain * price_x) * pred_var
                hedge_ratios.append(state_mean)
                spread = price_y - state_mean * price_x
                spreads.append(spread)
                if len(spreads) >= WIN:
                    recent = spreads[-WIN:]
                    mean = np.mean(recent)
                    std = np.std(recent)
                    z = (spread - mean) / std if std > 0 else 0
                    zscores.append(z)
                    print(f"时间:{now}, Y:{price_y}, X:{price_x}, 对冲比:{state_mean:.4f}, Z:{z:.4f}")
                    # 开仓
                    if position == 0:
                        if z < OPEN_L:
                            lots = int(INIT_CAP * POS_PCT / quote_y.margin)
                            lots_x = int(lots * state_mean * price_y * quote_y.volume_multiple / (price_x * quote_x.volume_multiple))
                            if lots > 0 and lots_x > 0:
                                target_y.set_target_volume(lots)
                                target_x.set_target_volume(-lots_x)
                                position = 1
                                entry_z = z
                                entry_time = now
                                print(f"开多Y空X, Y:{lots}, X:{lots_x}, 入场Z:{z:.4f}")
                        elif z > OPEN_H:
                            lots = int(INIT_CAP * POS_PCT / quote_y.margin)
                            lots_x = int(lots * state_mean * price_y * quote_y.volume_multiple / (price_x * quote_x.volume_multiple))
                            if lots > 0 and lots_x > 0:
                                target_y.set_target_volume(-lots)
                                target_x.set_target_volume(lots_x)
                                position = -1
                                entry_z = z
                                entry_time = now
                                print(f"开空Y多X, Y:{lots}, X:{lots_x}, 入场Z:{z:.4f}")
                    # 平仓
                    else:
                        profit_cond = CLOSE_L < z < CLOSE_H
                        stop_cond = (position == 1 and z < entry_z - STOP_SPREAD) or (position == -1 and z > entry_z + STOP_SPREAD)
                        max_hold = hold_days >= MAX_HOLD
                        if profit_cond or stop_cond or max_hold:
                            target_y.set_target_volume(0)
                            target_x.set_target_volume(0)
                            trade_count += 1
                            pnl = (z - entry_z) * position
                            if pnl > 0:
                                win_count += 1
                                total_profit += pnl
                            else:
                                total_loss -= pnl
                            reason = "回归" if profit_cond else "止损" if stop_cond else "超期"
                            print(f"平仓:{reason}, 出场Z:{z:.4f}, 收益:{pnl:.4f}, 持有天:{hold_days}")
                            position = 0
                            entry_z = 0
                            entry_time = None
                            hold_days = 0

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 2. Distance-Based Pairs Trading Strategy (基于距离的配对交易策略)

Identifies price-similar asset pairs and exploits short-term deviations in their price spreads. Assumes that highly similar assets' price differences exhibit mean-reversion tendencies.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = 'Chaos'

from tqsdk import TqApi, TqAuth, TargetPosTask, TqBacktest, BacktestFinished
from datetime import date, datetime
import numpy as np

# === 全局参数 ===
SYMBOL1 = "SHFE.rb2305"
SYMBOL2 = "SHFE.hc2305"
WINDOW = 30
K_THRESHOLD = 2.0
CLOSE_THRESHOLD = 0.5
MAX_HOLD_DAYS = 10
STOP_LOSS_PCT = 0.05
POSITION_LOTS1 = 200      # 合约1固定手数
POSITION_RATIO = 1.0     # 合约2与合约1的数量比例

# === 全局变量 ===
price_data1, price_data2 = [], []
position_long = False
position_short = False
entry_price1 = 0
entry_price2 = 0
position_time = None
trade_ratio = 1
entry_spread = 0
trade_count = 0
win_count = 0
total_profit = 0

# === API初始化 ===
api = TqApi(backtest=TqBacktest(start_dt=date(2023, 2, 1),end_dt=date(2023, 4, 27)),
            auth=TqAuth("快期账号", "快期密码")
)
quote1 = api.get_quote(SYMBOL1)
quote2 = api.get_quote(SYMBOL2)
klines1 = api.get_kline_serial(SYMBOL1, 24*60*60)
klines2 = api.get_kline_serial(SYMBOL2, 24*60*60)
target_pos1 = TargetPosTask(api, SYMBOL1)
target_pos2 = TargetPosTask(api, SYMBOL2)

print(f"策略开始运行，交易品种: {SYMBOL1} 和 {SYMBOL2}")

try:
    while True:
        api.wait_update()
        if api.is_changing(klines1.iloc[-1], "datetime") or api.is_changing(klines2.iloc[-1], "datetime"):
            price_data1.append(klines1.iloc[-1]["close"])
            price_data2.append(klines2.iloc[-1]["close"])
            if len(price_data1) <= WINDOW:
                continue
            if len(price_data1) > WINDOW:
                price_data1 = price_data1[-WINDOW:]
                price_data2 = price_data2[-WINDOW:]
            data1 = np.array(price_data1)
            data2 = np.array(price_data2)
            norm1 = (data1 - np.mean(data1)) / np.std(data1)
            norm2 = (data2 - np.mean(data2)) / np.std(data2)
            spread = norm1 - norm2
            mean_spread = np.mean(spread)
            std_spread = np.std(spread)
            current_spread = spread[-1]
            price_ratio = quote2.last_price / quote1.last_price
            trade_ratio = round(price_ratio * POSITION_RATIO, 2)
            position_lots2 = int(POSITION_LOTS1 * trade_ratio)
            current_time = datetime.fromtimestamp(klines1.iloc[-1]["datetime"] / 1e9)

            # === 平仓逻辑 ===
            if position_long or position_short:
                days_held = (current_time - position_time).days
                if position_long:
                    current_profit = (quote1.last_price - entry_price1) * POSITION_LOTS1 - (quote2.last_price - entry_price2) * position_lots2
                else:
                    current_profit = (entry_price1 - quote1.last_price) * POSITION_LOTS1 - (entry_price2 - quote2.last_price) * position_lots2
                profit_pct = current_profit / (entry_price1 * POSITION_LOTS1)
                close_by_mean = abs(current_spread - mean_spread) < CLOSE_THRESHOLD * std_spread
                close_by_time = days_held >= MAX_HOLD_DAYS
                close_by_stop = profit_pct <= -STOP_LOSS_PCT
                if close_by_mean or close_by_time or close_by_stop:
                    target_pos1.set_target_volume(0)
                    target_pos2.set_target_volume(0)
                    trade_count += 1
                    if profit_pct > 0:
                        win_count += 1
                    total_profit += current_profit
                    reason = "均值回归" if close_by_mean else "时间限制" if close_by_time else "止损"
                    print(f"平仓 - {reason}, 盈亏: {profit_pct:.2%}, 持仓天数: {days_held}")
                    position_long = False
                    position_short = False

            # === 开仓逻辑 ===
            else:
                if current_spread < mean_spread - K_THRESHOLD * std_spread:
                    target_pos1.set_target_volume(POSITION_LOTS1)
                    target_pos2.set_target_volume(-position_lots2)
                    position_long = True
                    position_time = current_time
                    entry_price1 = quote1.last_price
                    entry_price2 = quote2.last_price
                    entry_spread = current_spread
                    print(f"开仓 - 多价差, 合约1: {POSITION_LOTS1}手, 合约2: {-position_lots2}手, 比例: {trade_ratio}")
                elif current_spread > mean_spread + K_THRESHOLD * std_spread:
                    target_pos1.set_target_volume(-POSITION_LOTS1)
                    target_pos2.set_target_volume(position_lots2)
                    position_short = True
                    position_time = current_time
                    entry_price1 = quote1.last_price
                    entry_price2 = quote2.last_price
                    entry_spread = current_spread
                    print(f"开仓 - 空价差, 合约1: {-POSITION_LOTS1}手, 合约2: {position_lots2}手, 比例: {trade_ratio}")

        # 每日统计
        if api.is_changing(klines1.iloc[-1], "datetime"):
            account = api.get_account()
            print(f"日期: {current_time.date()}, 账户权益: {account.balance:.2f}, 可用资金: {account.available:.2f}")
            if trade_count > 0:
                print(f"交易统计 - 总交易: {trade_count}, 胜率: {win_count/trade_count:.2%}, 总盈亏: {total_profit:.2f}")

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 3. Pivot Point Reversal Strategy (枢轴点反转策略)

Computes support and resistance levels from the previous trading day's high, low, and close prices. When price drops to support levels and bounces, go long; when price rises to resistance and falls back, go short.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from tqsdk.tafunc import time_to_str

# ===== 全局参数设置 =====
SYMBOL = "SHFE.cu2309"
POSITION_SIZE = 100  
START_DATE = date(2023, 2, 10)
END_DATE = date(2023, 3, 15)

# 策略参数
REVERSAL_CONFIRM = 50
STOP_LOSS_POINTS = 100

# ===== 全局变量 =====
current_direction = 0
entry_price = 0
stop_loss_price = 0
prev_high = 0
prev_low = 0
prev_close = 0

# ===== 策略开始 =====
print("开始运行枢轴点反转策略...")

api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)

target_pos = TargetPosTask(api, SYMBOL)

def calculate_pivot_points(high, low, close):
    """计算枢轴点及支撑阻力位"""
    pivot = (high + low + close) / 3
    r1 = (2 * pivot) - low
    s1 = (2 * pivot) - high
    r2 = pivot + (high - low)
    s2 = pivot - (high - low)
    r3 = r1 + (high - low)
    s3 = s1 - (high - low)
    return pivot, r1, r2, r3, s1, s2, s3

try:
    while True:
        api.wait_update()

        if api.is_changing(klines.iloc[-1], "datetime"):
            if len(klines) < 2:
                continue

            current_price = klines.close.iloc[-1].item()
            current_high = klines.high.iloc[-1].item()
            current_low = klines.low.iloc[-1].item()
            prev_close = klines.close.iloc[-2].item()

            if klines.datetime.iloc[-1] != klines.datetime.iloc[-2]:
                prev_high = klines.high.iloc[-2].item()
                prev_low = klines.low.iloc[-2].item()
                prev_close = klines.close.iloc[-2].item()
                print(f"\n新的一天开始:")
                print(f"前一日数据 - 最高价: {prev_high:.2f}, 最低价: {prev_low:.2f}, 收盘价: {prev_close:.2f}")

            pivot, r1, r2, r3, s1, s2, s3 = calculate_pivot_points(prev_high, prev_low, prev_close)

            current_timestamp = klines.datetime.iloc[-1]
            current_datetime = time_to_str(current_timestamp)

            print(f"\n日期: {current_datetime}")
            print(f"当前价格: {current_price:.2f}")
            print(f"枢轴点: {pivot:.2f}")
            print(f"支撑位: S1={s1:.2f}, S2={s2:.2f}, S3={s3:.2f}")
            print(f"阻力位: R1={r1:.2f}, R2={r2:.2f}, R3={r3:.2f}")

            print("\n多头信号条件:")
            print(f"1. 价格在S1附近: {current_price <= s1 + REVERSAL_CONFIRM and current_price > s1 - REVERSAL_CONFIRM}")
            print(f"2. 价格高于当日最低价: {current_price > klines.low.iloc[-1].item()}")
            print(f"3. 价格高于前一日收盘价: {current_price > prev_close}")

            print("\n空头信号条件:")
            print(f"1. 价格在R1附近: {current_price >= r1 - REVERSAL_CONFIRM and current_price < r1 + REVERSAL_CONFIRM}")
            print(f"2. 价格低于当日最高价: {current_price < klines.high.iloc[-1].item()}")
            print(f"3. 价格低于前一日收盘价: {current_price < prev_close}")

            # ===== 交易逻辑 =====

            if current_direction == 0:
                if current_price < s1:
                    current_direction = 1
                    target_pos.set_target_volume(POSITION_SIZE)
                    entry_price = current_price
                    stop_loss_price = entry_price - STOP_LOSS_POINTS
                    print(f"\n多头开仓信号! 开仓价: {entry_price:.2f}, 止损价: {stop_loss_price:.2f}")

                elif current_price > r1:
                    current_direction = -1
                    target_pos.set_target_volume(-POSITION_SIZE)
                    entry_price = current_price
                    stop_loss_price = entry_price + STOP_LOSS_POINTS
                    print(f"\n空头开仓信号! 开仓价: {entry_price:.2f}, 止损价: {stop_loss_price:.2f}")

            elif current_direction == 1:
                if current_price >= pivot:
                    profit = (current_price - entry_price) * POSITION_SIZE
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止盈平仓: 价格={current_price:.2f}, 盈利={profit:.2f}")
                elif current_price <= stop_loss_price:
                    loss = (entry_price - current_price) * POSITION_SIZE
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止损平仓: 价格={current_price:.2f}, 亏损={loss:.2f}")

            elif current_direction == -1:
                if current_price <= pivot:
                    profit = (entry_price - current_price) * POSITION_SIZE
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止盈平仓: 价格={current_price:.2f}, 盈利={profit:.2f}")
                elif current_price >= stop_loss_price:
                    loss = (current_price - entry_price) * POSITION_SIZE
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止损平仓: 价格={current_price:.2f}, 亏损={loss:.2f}")

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---