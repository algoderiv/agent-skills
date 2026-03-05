# TqSdk 策略示例 - 趋势策略 (Part 7)

**包含策略:** Dual Thrust, R-Breaker

---

### 6. Dual Thrust Strategy (Dual Thrust策略)

Michael Chalek开发的经典趋势跟踪日内策略，基于N日高低收计算波动区间，以开盘价加减区间设置上下轨。

```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'limin'

from tqsdk import TqApi, TargetPosTask

SYMBOL = "DCE.jd2001"
NDAY = 5
K1 = 0.2
K2 = 0.2

api = TqApi()
quote = api.get_quote(SYMBOL)
klines = api.get_kline_serial(SYMBOL, 24 * 60 * 60)
target_pos = TargetPosTask(api, SYMBOL)

def dual_thrust(quote, klines):
    current_open = klines.iloc[-1]["open"]
    HH = max(klines.high.iloc[-NDAY - 1:-1])
    HC = max(klines.close.iloc[-NDAY - 1:-1])
    LC = min(klines.close.iloc[-NDAY - 1:-1])
    LL = min(klines.low.iloc[-NDAY - 1:-1])
    range = max(HH - LC, HC - LL)
    buy_line = current_open + range * K1
    sell_line = current_open - range * K2
    return buy_line, sell_line

buy_line, sell_line = dual_thrust(quote, klines)

while True:
    api.wait_update()
    if api.is_changing(klines.iloc[-1], ["datetime", "open"]):
        buy_line, sell_line = dual_thrust(quote, klines)
    if api.is_changing(quote, "last_price"):
        if quote.last_price > buy_line:
            target_pos.set_target_volume(3)
        elif quote.last_price < sell_line:
            target_pos.set_target_volume(-3)
```

---

### 7. R-Breaker Strategy (R-Breaker策略)

短线日内策略，根据前日高低收计算六条价格线（突破买卖价、观察买卖价、反转买卖价），结合趋势突破和反转逻辑交易。

```python
# !/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'limin'

from datetime import datetime
from tqsdk import TqApi, TargetPosTask

SYMBOL = "SHFE.au1912"
CLOSE_HOUR, CLOSE_MINUTE = 14, 50
STOP_LOSS_PRICE = 10

api = TqApi()

def get_index_line(klines):
    high = klines.high.iloc[-2]
    low = klines.low.iloc[-2]
    close = klines.close.iloc[-2]
    pivot = (high + low + close) / 3
    bBreak = high + 2 * (pivot - low)      # 突破买入价
    sSetup = pivot + (high - low)           # 观察卖出价
    sEnter = 2 * pivot - low               # 反转卖出价
    bEnter = 2 * pivot - high              # 反转买入价
    bSetup = pivot - (high - low)           # 观察买入价
    sBreak = low - 2 * (high - pivot)      # 突破卖出价
    return pivot, bBreak, sSetup, sEnter, bEnter, bSetup, sBreak

quote = api.get_quote(SYMBOL)
klines = api.get_kline_serial(SYMBOL, 24 * 60 * 60)
position = api.get_position(SYMBOL)
target_pos = TargetPosTask(api, SYMBOL)
target_pos_value = position.pos_long - position.pos_short
open_position_price = position.open_price_long if target_pos_value > 0 else position.open_price_short
pivot, bBreak, sSetup, sEnter, bEnter, bSetup, sBreak = get_index_line(klines)

while True:
    target_pos.set_target_volume(target_pos_value)
    api.wait_update()
    if api.is_changing(klines.iloc[-1], "datetime"):
        pivot, bBreak, sSetup, sEnter, bEnter, bSetup, sBreak = get_index_line(klines)

    if api.is_changing(quote, "datetime"):
        now = datetime.strptime(quote.datetime, "%Y-%m-%d %H:%M:%S.%f")
        if now.hour == CLOSE_HOUR and now.minute >= CLOSE_MINUTE:
            target_pos_value = 0
            pivot = bBreak = sSetup = sEnter = bEnter = bSetup = sBreak = float("nan")

    if api.is_changing(quote, "last_price"):
        # 止损
        if (target_pos_value > 0 and open_position_price - quote.last_price >= STOP_LOSS_PRICE) or \
                (target_pos_value < 0 and quote.last_price - open_position_price >= STOP_LOSS_PRICE):
            target_pos_value = 0

        # 反转逻辑
        if target_pos_value > 0:
            if quote.highest > sSetup and quote.last_price < sEnter:
                target_pos_value = -3
                open_position_price = quote.last_price
        elif target_pos_value < 0:
            if quote.lowest < bSetup and quote.last_price > bEnter:
                target_pos_value = 3
                open_position_price = quote.last_price

        # 突破逻辑
        elif target_pos_value == 0:
            if quote.last_price > bBreak:
                target_pos_value = 3
                open_position_price = quote.last_price
            elif quote.last_price < sBreak:
                target_pos_value = -3
                open_position_price = quote.last_price
```
