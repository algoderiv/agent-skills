# TqSdk 策略示例 - 趋势策略 (Part 6)

**包含策略:** Aberration, Fairy Four Price, Escalator

---

### 3. Aberration Strategy (Aberration策略)

经典布林带趋势跟踪策略，价格突破上轨开多，跌破下轨开空，回归中轨平仓。

```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = "Ringo"

from tqsdk import TqApi, TargetPosTask
from tqsdk.ta import BOLL

SYMBOL = "DCE.i2001"
api = TqApi()
quote = api.get_quote(SYMBOL)
klines = api.get_kline_serial(SYMBOL, 60*60*24)
position = api.get_position(SYMBOL)
target_pos = TargetPosTask(api, SYMBOL)

def boll_line(klines):
    boll = BOLL(klines, 26, 2)
    midline = boll["mid"].iloc[-1]
    topline = boll["top"].iloc[-1]
    bottomline = boll["bottom"].iloc[-1]
    return midline, topline, bottomline

midline, topline, bottomline = boll_line(klines)

while True:
    api.wait_update()
    if api.is_changing(klines.iloc[-1], "datetime"):
        midline, topline, bottomline = boll_line(klines)

    if api.is_changing(quote, "last_price"):
        if position.pos_long == 0 and position.pos_short == 0:
            if quote.last_price > topline:
                target_pos.set_target_volume(20)
            elif quote.last_price < bottomline:
                target_pos.set_target_volume(-20)
        elif position.pos_long > 0:
            if quote.last_price < midline:
                target_pos.set_target_volume(0)
        elif position.pos_short > 0:
            if quote.last_price > midline:
                target_pos.set_target_volume(0)
```

---

### 4. Fairy Four Price Strategy (菲阿里四价策略)

日内突破策略，基于昨日最高价/最低价突破开仓，以开盘价为止损，收盘前平仓。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = 'limin'

from tqsdk import TqApi, TargetPosTask
from datetime import datetime
import time

symbol = "SHFE.au1912"
close_hour, close_minute = 14, 50

api = TqApi()
quote = api.get_quote(symbol)
klines = api.get_kline_serial(symbol, 24 * 60 * 60)
position = api.get_position(symbol)
target_pos = TargetPosTask(api, symbol)

top_rail = klines.high.iloc[-2]
bottom_rail = klines.low.iloc[-2]

while True:
    api.wait_update()
    if api.is_changing(klines.iloc[-1], "datetime"):
        top_rail = klines.high.iloc[-2]
        bottom_rail = klines.low.iloc[-2]

    if api.is_changing(quote, "last_price"):
        if quote.last_price > top_rail and position.pos_long == 0:
            target_pos.set_target_volume(3)
        elif quote.last_price < bottom_rail and position.pos_short == 0:
            target_pos.set_target_volume(-3)

        # 止损: 回破当日开盘价
        if (quote.highest > top_rail and quote.last_price <= quote.open) or \
           (quote.lowest < bottom_rail and quote.last_price >= quote.open):
            target_pos.set_target_volume(0)

    if api.is_changing(quote, "datetime"):
        now_time = datetime.strptime(quote.datetime, "%Y-%m-%d %H:%M:%S.%f")
        if now_time.hour == close_hour and now_time.minute >= close_minute:
            target_pos.set_target_volume(0)
            deadline = time.time() + 60
            while api.wait_update(deadline=deadline):
                pass
            api.close()
            break
```

---

### 5. Escalator Strategy (自动扶梯策略)

利用连续两根K线的趋势特征，结合双均线过滤，进行开平仓操作。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Ringo"

from tqsdk import TqApi, TargetPosTask
from tqsdk.ta import MA

SYMBOL = "SHFE.au1912"
MA_SLOW, MA_FAST = 8, 40

api = TqApi()
quote = api.get_quote(SYMBOL)
klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)
position = api.get_position(SYMBOL)
target_pos = TargetPosTask(api, SYMBOL)

def kline_range(num):
    kl_range = (klines.iloc[num].close - klines.iloc[num].low) / (klines.iloc[num].high - klines.iloc[num].low)
    return kl_range

def ma_caculate(klines):
    ma_slow = MA(klines, MA_SLOW).iloc[-1].ma
    ma_fast = MA(klines, MA_FAST).iloc[-1].ma
    return ma_slow, ma_fast

ma_slow, ma_fast = ma_caculate(klines)

while True:
    api.wait_update()
    if api.is_changing(klines.iloc[-1], "datetime"):
        ma_slow, ma_fast = ma_caculate(klines)

    if api.is_changing(quote, "last_price"):
        if position.pos_long == 0 and position.pos_short == 0:
            kl_range_cur = kline_range(-2)
            kl_range_pre = kline_range(-3)
            # 开多: 收盘价在均线上方，前K线底部25%→当前K线顶部75%
            if klines.iloc[-2].close > max(ma_slow, ma_fast) and kl_range_pre <= 0.25 and kl_range_cur >= 0.75:
                target_pos.set_target_volume(100)
            # 开空: 收盘价在均线下方，前K线顶部75%→当前K线底部25%
            elif klines.iloc[-2].close < min(ma_slow, ma_fast) and kl_range_pre >= 0.75 and kl_range_cur <= 0.25:
                target_pos.set_target_volume(-100)

        elif position.pos_long > 0:
            kline_low = min(klines.iloc[-2].low, klines.iloc[-3].low)
            if klines.iloc[-1].close <= kline_low - quote.price_tick:
                target_pos.set_target_volume(0)

        elif position.pos_short > 0:
            kline_high = max(klines.iloc[-2].high, klines.iloc[-3].high)
            if klines.iloc[-1].close >= kline_high + quote.price_tick:
                target_pos.set_target_volume(0)
```
