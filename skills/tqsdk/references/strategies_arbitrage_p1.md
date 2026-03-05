# TqSdk 策略示例 - 套利策略 (Part 1)

**包含策略:** Hot Rolled Coil - Stainless Steel Arbitrage, Glass-Soda Ash Arbitrage, White Sugar - Corn Starch Arbitrage

---

### 1. Hot Rolled Coil - Stainless Steel Arbitrage (热卷--不锈钢套利/成材对冲策略)

This strategy exploits the "partial substitution relationship" and "relative value relationship" between hot rolled coil (HC) and stainless steel (SS) futures. It monitors the price ratio through Z-score standardization, identifying significant deviations from historical averages and betting on mean reversion.

```python
from tqsdk import TqApi, TqBacktest, TargetPosTask, TqAuth, BacktestFinished
import numpy as np
from datetime import date

# === 用户参数 ===
HC = "SHFE.hc2507"   # 热轧卷板主力合约
SS = "SHFE.ss2507"   # 不锈钢主力合约
START_DATE = date(2024, 10, 1)
END_DATE = date(2025, 1, 1)

LOOKBACK_DAYS = 30        # 历史窗口
STD_THRESHOLD = 2.0       # 开仓Z-score阈值
CLOSE_THRESHOLD = 0.5     # 平仓Z-score阈值
ORDER_VOLUME = 200         # 热卷下单手数
HC_RATIO = 1              # 热卷权重
SS_RATIO = 1              # 不锈钢权重

api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),auth=TqAuth("快期账号","快期密码"))
hc_quote = api.get_quote(HC)
ss_quote = api.get_quote(SS)
hc_klines = api.get_kline_serial(HC, 60*60*24, LOOKBACK_DAYS)
ss_klines = api.get_kline_serial(SS, 60*60*24, LOOKBACK_DAYS)

hc_pos = TargetPosTask(api, HC)
ss_pos = TargetPosTask(api, SS)

in_position = False

print(f"热卷-不锈钢套利策略启动，监控合约: {HC}, {SS}")

try:
    while True:
        api.wait_update()
        if api.is_changing(hc_klines) or api.is_changing(ss_klines):
            # 计算历史相对价值（比价）
            ratios = []
            for i in range(len(hc_klines.close) - 1):
                hc_value = hc_klines.close.iloc[i] * hc_quote.volume_multiple * HC_RATIO
                ss_value = ss_klines.close.iloc[i] * ss_quote.volume_multiple * SS_RATIO
                # 这里用比价，也可以用价差
                ratio = hc_value / ss_value if ss_value != 0 else 0
                ratios.append(ratio)
            mean_ratio = np.mean(ratios)
            std_ratio = np.std(ratios)

            # 当前比价
            hc_value = hc_klines.close.iloc[-1] * hc_quote.volume_multiple * HC_RATIO
            ss_value = ss_klines.close.iloc[-1] * ss_quote.volume_multiple * SS_RATIO
            current_ratio = hc_value / ss_value if ss_value != 0 else 0
            z_score = (current_ratio - mean_ratio) / std_ratio

            print(f"当前热卷/不锈钢比价: {current_ratio:.4f}, Z-score: {z_score:.2f}")

            # 计算不锈钢下单手数（按比例）
            ss_volume = int(ORDER_VOLUME * SS_RATIO / HC_RATIO)
            if ss_volume < 1:
                ss_volume = 1

            # === 交易信号判断 ===
            if not in_position:
                if z_score > STD_THRESHOLD:
                    # 比价偏高，预期回落：卖出热卷，买入不锈钢
                    print(f"比价偏高: 卖出热卷{ORDER_VOLUME}手，买入不锈钢{ss_volume}手")
                    hc_pos.set_target_volume(-ORDER_VOLUME)
                    ss_pos.set_target_volume(ss_volume)
                    in_position = True
                elif z_score < -STD_THRESHOLD:
                    # 比价偏低，预期回升：买入热卷，卖出不锈钢
                    print(f"比价偏低: 买入热卷{ORDER_VOLUME}手，卖出不锈钢{ss_volume}手")
                    hc_pos.set_target_volume(ORDER_VOLUME)
                    ss_pos.set_target_volume(-ss_volume)
                    in_position = True
            else:
                # 平仓条件
                if abs(z_score) < CLOSE_THRESHOLD:
                    print("比价回归，平仓所有头寸")
                    hc_pos.set_target_volume(0)
                    ss_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 2. Glass-Soda Ash Arbitrage (玻璃-纯碱套利/化工产业链策略)

This strategy tracks fluctuations in a "glass production profit spread" constructed between glass (FG) and soda ash (SA) futures prices, leveraging mean reversion principles to capitalize on deviations from historical averages.

```python
#!/usr/bin/env python
# coding=utf-8
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TargetPosTask, TqBacktest, BacktestFinished
import numpy as np
from datetime import date

FG = "CZCE.FG409"  # 玻璃合约
SA = "CZCE.SA409"  # 纯碱合约
START_DATE = date(2023, 11, 1)
END_DATE = date(2024, 4, 10)
LOOKBACK_DAYS = 30
STD_THRESHOLD = 2.0
ORDER_VOLUME = 200  # 下单手数
CLOSE_THRESHOLD = 0.5

# 生产比例
FG_RATIO = 1
SA_RATIO = 0.2  # 生产1手玻璃需要0.2手纯碱

api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE), auth=TqAuth("快期账号", "快期密码"))

fg_quote = api.get_quote(FG)
sa_quote = api.get_quote(SA)
fg_klines = api.get_kline_serial(FG, 60*60*24, LOOKBACK_DAYS)
sa_klines = api.get_kline_serial(SA, 60*60*24, LOOKBACK_DAYS)
fg_pos = TargetPosTask(api, FG)
sa_pos = TargetPosTask(api, SA)

try:
    in_position = False
    while True:
        api.wait_update()
        if api.is_changing(fg_klines) or api.is_changing(sa_klines):
            # 计算历史利润价差
            spreads = []
            for i in range(len(fg_klines) - 1):
                fg_price = fg_klines.close.iloc[i] * fg_quote.volume_multiple * FG_RATIO
                sa_price = sa_klines.close.iloc[i] * sa_quote.volume_multiple * SA_RATIO
                spreads.append(fg_price - sa_price)
            mean_spread = np.mean(spreads)
            std_spread = np.std(spreads)
            fg_price = fg_klines.close.iloc[-1] * fg_quote.volume_multiple * FG_RATIO
            sa_price = sa_klines.close.iloc[-1] * sa_quote.volume_multiple * SA_RATIO
            current_spread = fg_price - sa_price
            z_score = (current_spread - mean_spread) / std_spread
            print(f"当前玻璃-纯碱利润价差: {current_spread:.2f}, Z-score: {z_score:.2f}")

            fg_position = api.get_position(FG)
            sa_position = api.get_position(SA)
            current_fg_pos = fg_position.pos_long - fg_position.pos_short
            current_sa_pos = sa_position.pos_long - sa_position.pos_short

            # 按生产比例计算纯碱下单手数
            sa_volume = int(ORDER_VOLUME * SA_RATIO / FG_RATIO)
            if sa_volume < 1:
                sa_volume = 1  # 至少1手

            if not in_position:
                if z_score < -STD_THRESHOLD:
                    print(f"做空利润：卖出玻璃{ORDER_VOLUME}手，买入纯碱{sa_volume}手")
                    fg_pos.set_target_volume(-ORDER_VOLUME)
                    sa_pos.set_target_volume(sa_volume)
                    in_position = True
                elif z_score > STD_THRESHOLD:
                    print(f"做多利润：买入玻璃{ORDER_VOLUME}手，卖出纯碱{sa_volume}手")
                    fg_pos.set_target_volume(ORDER_VOLUME)
                    sa_pos.set_target_volume(-sa_volume)
                    in_position = True
            else:
                if abs(z_score) < CLOSE_THRESHOLD:
                    print("利润价差回归，平仓所有头寸")
                    fg_pos.set_target_volume(0)
                    sa_pos.set_target_volume(0)
                    in_position = False
                # 止损逻辑
                if (z_score < -STD_THRESHOLD * 1.5 and current_fg_pos < 0) or \
                   (z_score > STD_THRESHOLD * 1.5 and current_fg_pos > 0):
                    print("止损：价差向不利方向进一步偏离")
                    fg_pos.set_target_volume(0)
                    sa_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---

---

### 3. White Sugar - Corn Starch Arbitrage (白糖--淀粉糖/玉米淀粉代表套利策略)

This strategy monitors the relative value spread between white sugar (SR) and corn starch (CS) futures. It exploits price discrepancies between these substitutable commodities by tracking Z-score deviations from historical means.

```python
#!/usr/bin/env python
# coding=utf-8
__author__ = "Chaos"

from tqsdk import TqApi, TqAuth, TargetPosTask, TqBacktest, BacktestFinished
import numpy as np
from datetime import date

# === 用户参数 ===
# 合约参数
SR = "CZCE.SR409"  # 白糖期货合约
CS = "DCE.cs2409"  # 玉米淀粉期货合约
START_DATE = date(2024, 3, 20)   # 回测开始日期
END_DATE = date(2024, 5, 15)     # 回测结束日期

# 套利参数
LOOKBACK_DAYS = 30        # 计算历史价差的回溯天数
STD_THRESHOLD = 2.0       # 标准差阈值，超过此阈值视为套利机会
ORDER_VOLUME = 200         # 白糖的下单手数
CLOSE_THRESHOLD = 0.5     # 平仓阈值(标准差)

# 替代比例参数（可根据实际需要调整）
SR_RATIO = 1      # 1手白糖
CS_RATIO = 2.5    # 2.5手玉米淀粉

# === 初始化API ===
api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

# 获取合约行情和K线
sr_quote = api.get_quote(SR)
cs_quote = api.get_quote(CS)

sr_klines = api.get_kline_serial(SR, 60*60*24, LOOKBACK_DAYS)
cs_klines = api.get_kline_serial(CS, 60*60*24, LOOKBACK_DAYS)

# 创建目标持仓任务
sr_pos = TargetPosTask(api, SR)
cs_pos = TargetPosTask(api, CS)

# 获取合约乘数
sr_volume_multiple = sr_quote.volume_multiple
cs_volume_multiple = cs_quote.volume_multiple

# 初始化状态变量
in_position = False      # 是否有持仓
mean_spread = 0          # 历史价差均值
std_spread = 0           # 历史价差标准差

print(f"策略启动，监控合约: {SR}, {CS}")
print(f"参数: 标准差阈值={STD_THRESHOLD}, 平仓阈值={CLOSE_THRESHOLD}, 白糖:{CS}替代比例={SR_RATIO}:{CS_RATIO}")

# === 主循环 ===
try:
    # 初始计算历史统计值
    spreads = []
    for i in range(len(sr_klines) - 1):
        sr_value = sr_klines.close.iloc[i] * sr_volume_multiple * SR_RATIO
        cs_value = cs_klines.close.iloc[i] * cs_volume_multiple * CS_RATIO
        spread = sr_value - cs_value
        spreads.append(spread)
    
    mean_spread = np.mean(spreads)
    std_spread = np.std(spreads)
    print(f"历史白糖-玉米淀粉价差均值: {mean_spread:.2f}, 标准差: {std_spread:.2f}")

    # 主循环
    while True:
        api.wait_update()
        
        # 当K线数据有变化时进行计算
        if api.is_changing(sr_klines) or api.is_changing(cs_klines):
            # 重新计算历史价差统计
            spreads = []
            for i in range(len(sr_klines) - 1):
                sr_value = sr_klines.close.iloc[i] * sr_volume_multiple * SR_RATIO
                cs_value = cs_klines.close.iloc[i] * cs_volume_multiple * CS_RATIO
                spread = sr_value - cs_value
                spreads.append(spread)
            
            mean_spread = np.mean(spreads)
            std_spread = np.std(spreads)
            
            # 计算当前白糖-玉米淀粉价差
            sr_value = sr_klines.close.iloc[-1] * sr_volume_multiple * SR_RATIO
            cs_value = cs_klines.close.iloc[-1] * cs_volume_multiple * CS_RATIO
            current_spread = sr_value - cs_value
            
            # 计算z-score (标准化的价差)
            z_score = (current_spread - mean_spread) / std_spread
            
            print(f"当前白糖-玉米淀粉相对价值价差: {current_spread:.2f}, Z-score: {z_score:.2f}")
            
            # 获取当前持仓
            sr_position = api.get_position(SR)
            cs_position = api.get_position(CS)
            
            current_sr_pos = sr_position.pos_long - sr_position.pos_short
            current_cs_pos = cs_position.pos_long - cs_position.pos_short
            
            # 计算玉米淀粉实际下单手数（根据替代比例）
            cs_volume = int(ORDER_VOLUME * CS_RATIO / SR_RATIO)
            if cs_volume < 1:
                cs_volume = 1  # 保证至少1手
            
            # === 交易信号判断 ===
            if not in_position:  # 如果没有持仓
                if z_score > STD_THRESHOLD:  # 白糖相对价值显著高于玉米淀粉
                    # 反向：做空白糖，做多玉米淀粉
                    print(f"白糖相对价值偏高: 卖出白糖{ORDER_VOLUME}手，买入玉米淀粉{cs_volume}手")
                    sr_pos.set_target_volume(-ORDER_VOLUME)
                    cs_pos.set_target_volume(cs_volume)
                    in_position = True

                elif z_score < -STD_THRESHOLD:  # 白糖相对价值显著低于玉米淀粉
                    # 反向：做多白糖，做空玉米淀粉
                    print(f"白糖相对价值偏低: 买入白糖{ORDER_VOLUME}手，卖出玉米淀粉{cs_volume}手")
                    sr_pos.set_target_volume(ORDER_VOLUME)
                    cs_pos.set_target_volume(-cs_volume)
                    in_position = True
            
            else:  # 已有持仓
                # 平仓条件1: 价差回归至均值区域
                if abs(z_score) < CLOSE_THRESHOLD:
                    print("价差回归至均值区域，平仓所有头寸")
                    sr_pos.set_target_volume(0)
                    cs_pos.set_target_volume(0)
                    in_position = False
                
                # 平仓条件2: 止损 - 如果价差向不利方向继续扩大
                if (current_sr_pos > 0 and z_score < -STD_THRESHOLD) or \
                   (current_sr_pos < 0 and z_score > STD_THRESHOLD):
                    print("止损：价差向不利方向扩大，平仓所有头寸")
                    sr_pos.set_target_volume(0)
                    cs_pos.set_target_volume(0)
                    in_position = False

except BacktestFinished as e:
    print("回测结束")
    api.close()
```

---