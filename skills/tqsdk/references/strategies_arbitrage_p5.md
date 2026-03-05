# TqSdk 策略示例 - 套利策略 (Part 5)

**包含策略:** Steel Mill Profit Arbitrage, Multi-Commodity Oil Arbitrage

---

### 14. Steel Mill Profit Arbitrage (钢厂利润套利策略/螺纹钢、铁矿石、焦炭套利)

This strategy exploits the relationship between rebar, iron ore, and coke prices. The core formula is: Steel Mill Profit = 1 x Rebar Price - 1.6 x Iron Ore Price - 0.5 x Coke Price. The strategy identifies profit extremes using Bollinger Band-like logic with moving averages and standard deviations.

```python
from tqsdk import TqApi, TargetPosTask
from tqsdk.tafunc import ma
import numpy as np

api = TqApi()

SYMBOL_rb = "SHFE.rb2001"
SYMBOL_i = "DCE.i2001"
SYMBOL_j = "DCE.j2001"

klines_rb = api.get_kline_serial(SYMBOL_rb, 86400)
klines_i = api.get_kline_serial(SYMBOL_i, 86400)
klines_j = api.get_kline_serial(SYMBOL_j, 86400)

target_pos_rb = TargetPosTask(api, SYMBOL_rb)
target_pos_i = TargetPosTask(api, SYMBOL_i)
target_pos_j = TargetPosTask(api, SYMBOL_j)

# 计算钢厂利润线，并将利润线画到副图
def cal_spread(klines_rb, klines_i, klines_j):
    index_spread = klines_rb.close - 1.6 * klines_i.close - 0.5 * klines_j.close
    # 使用15日均值，与注释保持一致
    ma_spread = ma(index_spread, 15)
    # 计算标准差
    spread_std = np.std(index_spread)
    klines_rb["index_spread"] = index_spread
    klines_rb["index_spread.board"] = "index_spread"

    return index_spread, ma_spread, spread_std


# 初始计算利润线
index_spread, ma_spread, spread_std = cal_spread(klines_rb, klines_i, klines_j)

print("ma_spread是%.2f,index_spread是%.2f,spread_std是%.2f" % (ma_spread.iloc[-1], index_spread.iloc[-1], spread_std))

# 记录当前持仓状态，避免重复发出信号
current_position = 0  # 0表示空仓，1表示多螺纹空焦炭焦煤，-1表示空螺纹多焦炭焦煤

while True:
    api.wait_update()
    
    # 每次有新日线生成时重新计算利润线
    if api.is_changing(klines_j.iloc[-1], "datetime"):
        index_spread, ma_spread, spread_std = cal_spread(klines_rb, klines_i, klines_j)
        
        # 计算上下轨
        upper_band = ma_spread.iloc[-1] + 0.5 * spread_std
        lower_band = ma_spread.iloc[-1] - 0.5 * spread_std
        
        print("ma_spread是%.2f,index_spread是%.2f,spread_std是%.2f" % (
            ma_spread.iloc[-1], index_spread.iloc[-1], spread_std))
        print("上轨是%.2f,下轨是%.2f" % (upper_band, lower_band))
        
        # 确保有足够的历史数据
        if len(index_spread) >= 2:
            # 1. 检测下穿上轨：前一个K线在上轨之上，当前K线在上轨之下或等于上轨
            if index_spread.iloc[-2] > upper_band and index_spread.iloc[-1] <= upper_band:
                if current_position != -1:  # 避免重复开仓
                    # 价差序列下穿上轨，利润冲高回落进行回复，策略空螺纹钢、多焦煤焦炭
                    target_pos_rb.set_target_volume(-100)
                    target_pos_i.set_target_volume(100)
                    target_pos_j.set_target_volume(100)
                    current_position = -1
                    print("下穿上轨：空螺纹钢、多焦煤焦炭")
            
            # 2. 检测上穿下轨：前一个K线在下轨之下，当前K线在下轨之上或等于下轨
            elif index_spread.iloc[-2] < lower_band and index_spread.iloc[-1] >= lower_band:
                if current_position != 1:  # 避免重复开仓
                    # 价差序列上穿下轨，利润过低回复上升，策略多螺纹钢、空焦煤焦炭
                    target_pos_rb.set_target_volume(100)
                    target_pos_i.set_target_volume(-100)
                    target_pos_j.set_target_volume(-100)
                    current_position = 1
                    print("上穿下轨：多螺纹钢、空焦煤焦炭")
    
    # 实时监控价差变化情况
    if api.is_changing(klines_rb.iloc[-1], "close") or api.is_changing(klines_i.iloc[-1], "close") or api.is_changing(klines_j.iloc[-1], "close"):
        # 实时更新价差
        current_spread = klines_rb.close.iloc[-1] - 1.6 * klines_i.close.iloc[-1] - 0.5 * klines_j.close.iloc[-1]
        # 可以添加实时监控输出
        # print("当前价差: %.2f" % current_spread)
```

---

---

### 15. Multi-Commodity Oil Arbitrage (菜油、豆油、棕榈油多品种套利策略)

This strategy exploits price cointegration among three oil futures (rapeseed oil OI, soybean oil Y, palm oil P) by monitoring their relative price spreads. When price gaps deviate from historical norms, the strategy initiates arbitrage positions, profiting from mean reversion toward equilibrium.

```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = "Ringo"

"""
豆油、棕榈油、菜油套利策略
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
"""
from tqsdk import TqApi, TargetPosTask
from tqsdk.tafunc import ma

# 设定豆油，菜油，棕榈油指定合约
SYMBOL_Y = "DCE.y2001"
SYMBOL_OI = "CZCE.OI001"
SYMBOL_P = "DCE.p2001"

api = TqApi()

klines_y = api.get_kline_serial(SYMBOL_Y, 24 * 60 * 60)
klines_oi = api.get_kline_serial(SYMBOL_OI, 24 * 60 * 60)
klines_p = api.get_kline_serial(SYMBOL_P, 24 * 60 * 60)

target_pos_oi = TargetPosTask(api, SYMBOL_OI)
target_pos_y = TargetPosTask(api, SYMBOL_Y)
target_pos_p = TargetPosTask(api, SYMBOL_P)


# 设置指标计算函数，计算三种合约品种的相对位置，并将指标画在副图
def cal_spread(klines_y, klines_p, klines_oi):
    index_spread = ((klines_y.close - klines_p.close) - (klines_oi.close - klines_y.close)) / (
            klines_oi.close - klines_p.close)
    klines_y["index_spread"] = index_spread
    ma_short = ma(index_spread, 5)
    ma_long = ma(index_spread, 15)
    return index_spread, ma_short, ma_long


index_spread, ma_short, ma_long = cal_spread(klines_y, klines_p, klines_oi)

klines_y["index_spread.board"] = "index_spread"

print("ma_short是%.2f,ma_long是%.2f，index_spread是%.2f" % (ma_short.iloc[-2], ma_long.iloc[-2], index_spread.iloc[-2]))

while True:
    api.wait_update()
    if api.is_changing(klines_y.iloc[-1], "datetime"):
        index_spread, ma_short, ma_long = cal_spread(klines_y, klines_p, klines_oi)
        print("日线更新，ma_short是%.2f,ma_long是%.2f，index_spread是%.2f" % (
            ma_short.iloc[-2], ma_long.iloc[-2], index_spread.iloc[-2]))

    # 指数上涨，短期上穿长期，则认为相对于y，oi被低估，做多oi，做空y
    if (ma_short.iloc[-2] > ma_long.iloc[-2]) and (ma_short.iloc[-3] < ma_long.iloc[-3]) and (
            index_spread.iloc[-2] > 1.02 * ma_short.iloc[-2]):
        target_pos_y.set_target_volume(-100)
        target_pos_oi.set_target_volume(100)
    # 指数下跌，短期下穿长期，则认为相对于y，p被高估，做多y，做空p
    elif (ma_short.iloc[-2] < ma_long.iloc[-2]) and (ma_short.iloc[-3] > ma_long.iloc[-3]) and (
            index_spread.iloc[-2] < 0.98 * ma_short.iloc[-2]):
        target_pos_y.set_target_volume(100)
        target_pos_p.set_target_volume(-100)
    # 现在策略表现平稳，则平仓，赚取之前策略收益
    elif ma_short.iloc[-2] * 0.98 < index_spread.iloc[-2] < ma_long.iloc[-2] * 1.02:
        target_pos_oi.set_target_volume(0)
        target_pos_p.set_target_volume(0)
        target_pos_y.set_target_volume(0)
```

---

**Summary:** All 15 arbitrage strategies have been extracted from shinnytech.com. They all use the TianQin SDK (tqsdk) for Chinese futures market trading. The strategies fall into several categories:

- **Industrial chain profit arbitrage** (strategies 2, 4, 5, 6, 7, 9, 10, 14): Trade production profit margins between raw materials and finished products
- **Substitution/relative value arbitrage** (strategies 1, 3, 8, 15): Trade relative price ratios between substitute or correlated commodities
- **Calendar/cross-period arbitrage** (strategies 11, 13): Trade price spreads between different delivery months
- **Options arbitrage** (strategy 12): Risk-free box spread identification using options