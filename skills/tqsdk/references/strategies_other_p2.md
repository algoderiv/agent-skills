# TqSdk 策略示例 - 均值回归与其他策略 (Part 2)

**包含策略:** CCI Mean Reversion Strategy, RSI Overbought/Oversold Trading Strategy, Z-Score Mean Reversion Strategy

---

### 4. CCI Mean Reversion Strategy (CCI均值回归策略)

Leverages price in the short term's extreme deviation often reverting to mean. When CCI crosses +/-100 thresholds, it signals potential reversions from overbought/oversold states.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from tqsdk.tafunc import time_to_str
from tqsdk.ta import CCI

# ===== 全局参数设置 =====
SYMBOL = "SHFE.au2406"  # 黄金期货主力合约
POSITION_SIZE = 50  # 每次交易手数
START_DATE = date(2023, 9, 20)  # 回测开始日期
END_DATE = date(2024, 2, 28)  # 回测结束日期

# CCI参数
CCI_PERIOD = 10  # CCI计算周期
CCI_UPPER = 100  # CCI上轨
CCI_LOWER = -100  # CCI下轨
STOP_LOSS_PERCENT = 0.01  # 止损比例

# ===== 全局变量 =====
current_direction = 0  # 当前持仓方向：1=多头，-1=空头，0=空仓
entry_price = 0  # 开仓价格
stop_loss_price = 0  # 止损价格
last_cci_state = 0  # 上一次CCI状态：1=高于上轨，-1=低于下轨，0=中间区域

# ===== 策略开始 =====
print("开始运行CCI均值回归策略...")

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
            if len(klines) < CCI_PERIOD + 10:
                continue
            
            # 计算CCI
            cci_series = CCI(klines, CCI_PERIOD)
            cci = cci_series.iloc[-1].item()  # 使用item()方法获取标量值
            current_price = klines.close.iloc[-1].item()  # 同样使用item()方法
            
            # 确定当前CCI状态
            current_cci_state = 0
            if cci > CCI_UPPER:
                current_cci_state = 1
            elif cci < CCI_LOWER:
                current_cci_state = -1
            
            # 获取最新数据
            current_timestamp = klines.datetime.iloc[-1]
            current_datetime = time_to_str(current_timestamp)
            
            # 打印当前状态
            print(f"日期: {current_datetime}, 价格: {current_price:.2f}, CCI: {cci:.2f}")
            
            # ===== 交易逻辑 =====
            
            # 空仓状态 - 寻找开仓机会
            if current_direction == 0:
                # 多头开仓条件：CCI从下轨上穿
                if last_cci_state == -1 and current_cci_state == 0:
                    current_direction = 1
                    target_pos.set_target_volume(POSITION_SIZE)
                    entry_price = current_price
                    stop_loss_price = entry_price * (1 - STOP_LOSS_PERCENT)
                    print(f"多头开仓: 价格={entry_price:.2f}, 止损价={stop_loss_price:.2f}")
                
                # 空头开仓条件：CCI从上轨下穿
                elif last_cci_state == 1 and current_cci_state == 0:
                    current_direction = -1
                    target_pos.set_target_volume(-POSITION_SIZE)
                    entry_price = current_price
                    stop_loss_price = entry_price * (1 + STOP_LOSS_PERCENT)
                    print(f"空头开仓: 价格={entry_price:.2f}, 止损价={stop_loss_price:.2f}")
            
            # 多头持仓 - 检查平仓条件
            elif current_direction == 1:
                # 止盈条件：CCI从上轨下穿
                if last_cci_state == 1 and current_cci_state == 0:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止盈平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")
                
                # 止损条件：价格跌破止损价
                elif current_price < stop_loss_price:
                    loss_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止损平仓: 价格={current_price:.2f}, 亏损={loss_pct:.2f}%")
            
            # 空头持仓 - 检查平仓条件
            elif current_direction == -1:
                # 止盈条件：CCI从下轨上穿
                if last_cci_state == -1 and current_cci_state == 0:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止盈平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")
                
                # 止损条件：价格突破止损价
                elif current_price > stop_loss_price:
                    loss_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止损平仓: 价格={current_price:.2f}, 亏损={loss_pct:.2f}%")
            
            # 更新上一次CCI状态
            last_cci_state = current_cci_state

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 5. RSI Overbought/Oversold Trading Strategy (RSI超买/超卖交易策略)

Based on the Relative Strength Index (RSI) indicator with mean reversion principles. Generates long signals when RSI rises above 35 after being in oversold territory, and short signals when RSI falls below 65 after being in overbought territory.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from tqsdk.ta import RSI
from tqsdk.tafunc import time_to_str

# ===== 全局参数设置 =====
SYMBOL = "DCE.a2101"
POSITION_SIZE = 500  # 每次交易手数
START_DATE = date(2020, 4, 20)  # 回测开始日期
END_DATE = date(2020, 11, 20)  # 回测结束日期

# RSI参数
RSI_PERIOD = 6  # RSI计算周期，
OVERBOUGHT_THRESHOLD = 65  # 超买阈值
OVERSOLD_THRESHOLD = 35  # 超卖阈值

# 风控参数
STOP_LOSS_PERCENT = 0.01  # 止损百分比
TIME_STOP_DAYS = 10  # 缩短时间止损天数

# ===== 全局变量 =====
current_direction = 0  # 当前持仓方向：1=多头，-1=空头，0=空仓
entry_price = 0  # 开仓价格
stop_loss_price = 0  # 止损价格
entry_date = None  # 开仓日期
was_overbought = False  # 是否曾进入超买区域
was_oversold = False  # 是否曾进入超卖区域

# ===== 策略开始 =====
print("开始运行RSI超买/超卖反转策略...")

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
            if len(klines) < RSI_PERIOD + 10:
                continue

            # 使用天勤的RSI函数计算
            rsi = RSI(klines, RSI_PERIOD)
            current_rsi = float(rsi.iloc[-1].iloc[0])
            previous_rsi = float(rsi.iloc[-2].iloc[0])

            # 更新超买/超卖状态
            if previous_rsi > OVERBOUGHT_THRESHOLD:
                was_overbought = True
            if previous_rsi < OVERSOLD_THRESHOLD:
                was_oversold = True

            # 获取最新数据
            current_price = float(klines.close.iloc[-1])
            current_timestamp = klines.datetime.iloc[-1]
            current_datetime = time_to_str(current_timestamp)  # 使用time_to_str转换时间

            # 打印当前状态
            print(f"日期: {current_datetime}, 价格: {current_price:.2f}, RSI: {current_rsi:.2f}")

            # ===== 交易逻辑 =====

            # 空仓状态 - 寻找开仓机会
            if current_direction == 0:
                # 多头开仓条件：RSI从超卖区域回升
                if was_oversold and previous_rsi < OVERSOLD_THRESHOLD and current_rsi > OVERSOLD_THRESHOLD:
                    current_direction = 1
                    target_pos.set_target_volume(POSITION_SIZE)
                    entry_price = current_price
                    stop_loss_price = entry_price * (1 - STOP_LOSS_PERCENT)
                    entry_date = current_timestamp  # 存储时间戳
                    print(f"多头开仓: 价格={entry_price:.2f}, 止损={stop_loss_price:.2f}")
                    was_oversold = False  # 重置超卖状态

                # 空头开仓条件：RSI从超买区域回落
                elif was_overbought and previous_rsi > OVERBOUGHT_THRESHOLD and current_rsi < OVERBOUGHT_THRESHOLD:
                    current_direction = -1
                    target_pos.set_target_volume(-POSITION_SIZE)
                    entry_price = current_price
                    stop_loss_price = entry_price * (1 + STOP_LOSS_PERCENT)
                    entry_date = current_timestamp  # 存储时间戳
                    print(f"空头开仓: 价格={entry_price:.2f}, 止损={stop_loss_price:.2f}")
                    was_overbought = False  # 重置超买状态

            # 多头持仓 - 检查平仓条件
            elif current_direction == 1:
                # 止损条件
                if current_price <= stop_loss_price:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止损平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 止盈条件：RSI进入超买区域
                elif current_rsi > OVERBOUGHT_THRESHOLD:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止盈平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 时间止损
                elif (current_timestamp - entry_date) / (60 * 60 * 24) >= TIME_STOP_DAYS:  # 计算天数差
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头时间止损: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

            # 空头持仓 - 检查平仓条件
            elif current_direction == -1:
                # 止损条件
                if current_price >= stop_loss_price:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止损平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 止盈条件：RSI进入超卖区域
                elif current_rsi < OVERSOLD_THRESHOLD:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止盈平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 时间止损
                elif (current_timestamp - entry_date) / (60 * 60 * 24) >= TIME_STOP_DAYS:  # 计算天数差
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头时间止损: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 6. Z-Score Mean Reversion Strategy (Z-Score均值回归策略)

Identifies significantly overvalued or undervalued price levels by calculating how many standard deviations current prices deviate from their historical moving average. Executes counter-trend trades when prices reach extreme statistical levels.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
import numpy as np
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished
from tqsdk.tafunc import time_to_str

# ===== 全局参数设置 =====
SYMBOL = "SHFE.au2106" 
POSITION_SIZE = 50  # 每次交易手数
START_DATE = date(2020, 11, 1)  # 回测开始日期
END_DATE = date(2020, 12, 15)  # 回测结束日期

# Z-Score参数
WINDOW_SIZE = 14  # Z-Score计算窗口期
ENTRY_THRESHOLD = 1.8  # 开仓阈值
EXIT_THRESHOLD = 0.4  # 平仓阈值
STOP_LOSS_THRESHOLD = 2.5  # 止损阈值

# 风控参数
TIME_STOP_DAYS = 8  # 时间止损天数

# ===== 全局变量 =====
current_direction = 0  # 当前持仓方向：1=多头，-1=空头，0=空仓
entry_price = 0  # 开仓价格
entry_date = None  # 开仓日期

# ===== 策略开始 =====
print("开始运行Z-Score均值回归策略...")

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
            if len(klines) < WINDOW_SIZE + 10:
                continue

            # 计算Z-Score
            prices = klines.close.iloc[-WINDOW_SIZE:]  # 获取最近20天的收盘价
            mean = np.mean(prices)  # 计算均值
            std = np.std(prices)  # 计算标准差
            current_price = float(klines.close.iloc[-1])  # 当前价格

            # 处理标准差为0的情况
            if std == 0:
                z_score = 0  # 如果标准差为0，说明所有价格都相同，Z-Score设为0
            else:
                z_score = (current_price - mean) / std  # 计算Z-Score

            # 获取最新数据
            current_timestamp = klines.datetime.iloc[-1]
            current_datetime = time_to_str(current_timestamp)

            # 打印当前状态
            print(f"日期: {current_datetime}, 价格: {current_price:.2f}, Z-Score: {z_score:.2f}")

            # ===== 交易逻辑 =====

            # 空仓状态 - 寻找开仓机会
            if current_direction == 0:
                # 多头开仓条件：Z-Score显著低于均值
                if z_score < -ENTRY_THRESHOLD:
                    current_direction = 1
                    target_pos.set_target_volume(POSITION_SIZE)
                    entry_price = current_price
                    entry_date = current_timestamp
                    print(f"多头开仓: 价格={entry_price:.2f}, Z-Score={z_score:.2f}")

                # 空头开仓条件：Z-Score显著高于均值
                elif z_score > ENTRY_THRESHOLD:
                    current_direction = -1
                    target_pos.set_target_volume(-POSITION_SIZE)
                    entry_price = current_price
                    entry_date = current_timestamp
                    print(f"空头开仓: 价格={entry_price:.2f}, Z-Score={z_score:.2f}")

            # 多头持仓 - 检查平仓条件
            elif current_direction == 1:
                # 止损条件：Z-Score继续大幅下跌
                if z_score < -STOP_LOSS_THRESHOLD:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止损平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 止盈条件：Z-Score回归到均值附近
                elif -EXIT_THRESHOLD <= z_score <= EXIT_THRESHOLD:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头止盈平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 时间止损
                elif (current_timestamp - entry_date) / (60 * 60 * 24) >= TIME_STOP_DAYS:
                    profit_pct = (current_price - entry_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"多头时间止损: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

            # 空头持仓 - 检查平仓条件
            elif current_direction == -1:
                # 止损条件：Z-Score继续大幅上涨
                if z_score > STOP_LOSS_THRESHOLD:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止损平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 止盈条件：Z-Score回归到均值附近
                elif -EXIT_THRESHOLD <= z_score <= EXIT_THRESHOLD:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头止盈平仓: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

                # 时间止损
                elif (current_timestamp - entry_date) / (60 * 60 * 24) >= TIME_STOP_DAYS:
                    profit_pct = (entry_price - current_price) / entry_price * 100
                    target_pos.set_target_volume(0)
                    current_direction = 0
                    print(f"空头时间止损: 价格={current_price:.2f}, 盈亏={profit_pct:.2f}%")

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---