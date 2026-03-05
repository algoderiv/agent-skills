# TqSdk 策略示例 - 均值回归与其他策略 (Part 5)

**包含策略:** Grid Trading Strategy

---

### 15. Grid Trading Strategy (网格交易策略)

Buys on dips and sells on rallies in oscillating markets. Mechanically establishes price grids in both long and short directions, triggering buy orders when prices fall through grid levels and sell orders when prices rise through them.

```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'limin'

"""
网格交易策略

注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
"""

from functools import reduce
from tqsdk import TqApi, TargetPosTask

SYMBOL = "DCE.jd2001"  # 合约代码
START_PRICE = 4247  # 起始价位
GRID_AMOUNT = 10  # 网格在多头、空头方向的格子(档位)数量

api = TqApi()
grid_region_long = [0.005] * GRID_AMOUNT  # 多头每格价格跌幅(网格密度)
grid_region_short = [0.005] * GRID_AMOUNT  # 空头每格价格涨幅(网格密度)
grid_volume_long = [i for i in range(GRID_AMOUNT + 1)]  # 多头每格持仓手数
grid_volume_short = [i for i in range(GRID_AMOUNT + 1)]  # 空头每格持仓手数
grid_prices_long = [reduce(lambda p, r: p*(1-r), grid_region_long[:i], START_PRICE) for i in range(GRID_AMOUNT + 1)]  # 多头每格的触发价位列表
grid_prices_short = [reduce(lambda p, r: p*(1+r), grid_region_short[:i], START_PRICE) for i in range(GRID_AMOUNT + 1)]  # 空头每格的触发价位列表

print("策略开始运行, 起始价位: %f, 多头每格持仓手数:%s, 多头每格的价位:%s, 空头每格的价位:%s" % (START_PRICE, grid_volume_long, grid_prices_long, grid_prices_short))
quote = api.get_quote(SYMBOL)  # 行情数据
target_pos = TargetPosTask(api, SYMBOL)
position = api.get_position(SYMBOL)  # 持仓信息

def wait_price(layer):
    """等待行情最新价变动到其他档位,则进入下一档位或回退到上一档位; 如果从下一档位回退到当前档位,则设置为当前对应的持仓手数;
        layer : 当前所在第几个档位层次; layer>0 表示多头方向, layer<0 表示空头方向
    """
    if layer > 0 or quote.last_price <= grid_prices_long[1]:  # 是多头方向
        while True:
            api.wait_update()
            # 如果当前档位小于最大档位,并且最新价小于等于下一个档位的价格: 则设置为下一档位对应的手数后进入下一档位层次
            if layer < GRID_AMOUNT and quote.last_price <= grid_prices_long[layer + 1]:
                target_pos.set_target_volume(grid_volume_long[layer + 1])
                print("最新价: %f, 进入: 多头第 %d 档" % (quote.last_price, layer + 1))
                wait_price(layer + 1)
                # 从下一档位回退到当前档位后, 设置回当前对应的持仓手数
                target_pos.set_target_volume(grid_volume_long[layer + 1])
            # 如果最新价大于当前档位的价格: 则回退到上一档位
            if quote.last_price > grid_prices_long[layer]:
                print("最新价: %f, 回退到: 多头第 %d 档" % (quote.last_price, layer))
                return
    elif layer < 0 or quote.last_price >= grid_prices_short[1]:  # 是空头方向
        layer = -layer  # 转为正数便于计算
        while True:
            api.wait_update()
            # 如果当前档位小于最大档位层次,并且最新价大于等于下一个档位的价格: 则设置为下一档位对应的持仓手数后进入下一档位层次
            if layer < GRID_AMOUNT and quote.last_price >= grid_prices_short[layer + 1]:
                target_pos.set_target_volume(-grid_volume_short[layer + 1])
                print("最新价: %f, 进入: 空头第 %d 档" % (quote.last_price, layer + 1))
                wait_price(-(layer + 1))
                # 从下一档位回退到当前档位后, 设置回当前对应的持仓手数
                target_pos.set_target_volume(-grid_volume_short[layer + 1])
            # 如果最新价小于当前档位的价格: 则回退到上一档位
            if quote.last_price < grid_prices_short[layer]:
                print("最新价: %f, 回退到: 空头第 %d 档" % (quote.last_price, layer))
                return

while True:
    api.wait_update()
    wait_price(0)  # 从第0层开始进入网格
    target_pos.set_target_volume(0)
```

---

## Summary

All 15 strategies have been extracted with complete Python code from shinnytech.com. They all use the **TqSdk** (TianQin SDK) framework for Chinese futures trading. Here is a quick reference:

| # | Strategy | Chinese Name | Category |
|---|----------|-------------|----------|
| 1 | Kalman Filter Pairs Trading | 卡尔曼滤波配对交易策略 | Mean Reversion |
| 2 | Distance-Based Pairs Trading | 基于距离的配对交易策略 | Mean Reversion |
| 3 | Pivot Point Reversal | 枢轴点反转策略 | Mean Reversion |
| 4 | CCI Mean Reversion | CCI均值回归策略 | Mean Reversion |
| 5 | RSI Overbought/Oversold | RSI超买/超卖交易策略 | Mean Reversion |
| 6 | Z-Score Mean Reversion | Z-Score均值回归策略 | Mean Reversion |
| 7 | Mean-Reversion (RSI+BOLL) | 反转策略 | Mean Reversion |
| 8 | Iceberg Order | 冰山订单算法 | Other |
| 9 | Order Book Placement | 盘口下单算法 | Other |
| 10 | POV (Percentage of Volume) | 跟量下单算法 | Other |
| 11 | Random Forest Prediction | 应用随机森林对某交易日涨跌情况的预测 | Other |
| 12 | ATR Strategy | 真实波幅均值(ATR) | Other |
| 13 | Turtle Trading | 海龟交易法则 | Other |
| 14 | VWAP Algorithm | VWAP算法(成交量加权平均价格) | Other |
| 15 | Grid Trading | 网格交易策略 | Other |