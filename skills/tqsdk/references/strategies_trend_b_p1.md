# TqSdk 策略示例 - 趋势策略 (Part 5)

**包含策略:** Momentum Ranking, VPT

---

### 1. Momentum Ranking Strategy (动量排名策略)

动量排名策略通过计算多品种过去N天的收益率，选择表现最强的品种做多，每日调仓。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished

# 参数设置
SYMBOLS = [
    "SHFE.rb2405", "SHFE.ru2405",
    "DCE.m2405", "DCE.p2405",
    "CZCE.MA405", "CZCE.TA405",
    "SHFE.cu2405", "SHFE.ni2405"
]
LOOKBACK_DAYS = 15
TOP_N = 2
POSITION_SIZE = 100
START_DATE = date(2023, 10, 1)
END_DATE = date(2023, 12, 31)

api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
            auth=TqAuth("快期账号", "快期密码"))

klines = {}
quotes = {}
target_pos = {}

for symbol in SYMBOLS:
    klines[symbol] = api.get_kline_serial(symbol, 24 * 60 * 60)
    quotes[symbol] = api.get_quote(symbol)
    target_pos[symbol] = TargetPosTask(api, symbol)

current_holdings = set()

def calculate_momentum():
    momentum_scores = {}
    for symbol in SYMBOLS:
        if len(klines[symbol].close) >= LOOKBACK_DAYS:
            returns = (klines[symbol].close.iloc[-1] / klines[symbol].close.iloc[-LOOKBACK_DAYS] - 1) * 100
            momentum_scores[symbol] = returns
    ranked_symbols = sorted(momentum_scores.items(), key=lambda x: x[1], reverse=True)
    return ranked_symbols

def update_positions(ranked_symbols):
    new_holdings = set([symbol for symbol, score in ranked_symbols[:TOP_N]])
    for symbol in current_holdings - new_holdings:
        target_pos[symbol].set_target_volume(0)
    for symbol in new_holdings - current_holdings:
        target_pos[symbol].set_target_volume(POSITION_SIZE)
    current_holdings.clear()
    current_holdings.update(new_holdings)

try:
    day_count = 0
    while True:
        api.wait_update()
        for symbol in SYMBOLS:
            if api.is_changing(klines[symbol].iloc[-1], "datetime"):
                day_count += 1
                if day_count >= LOOKBACK_DAYS:
                    ranked_symbols = calculate_momentum()
                    update_positions(ranked_symbols)
                break
except BacktestFinished as e:
    api.close()
    print("回测结束")
```

---

### 2. VPT Strategy (量价趋势策略)

通过成交量与价格变化的关系判断市场趋势有效性，配合均线交叉和量能确认进行交易。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = "Chaos"

from datetime import date
import numpy as np
from tqsdk import TqApi, TqAuth, TqBacktest, TargetPosTask, BacktestFinished

SYMBOL = "SHFE.au2306"
POSITION_SIZE = 30
START_DATE = date(2023, 1, 15)
END_DATE = date(2023, 5, 15)
VPT_MA_PERIOD = 14
VOLUME_THRESHOLD = 1.5

api = None
try:
    api = TqApi(backtest=TqBacktest(start_dt=START_DATE, end_dt=END_DATE),
                auth=TqAuth("快期账号", "快期密码"))
    klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)
    target_pos = TargetPosTask(api, SYMBOL)

    position = 0
    entry_price = 0
    vpt_values = []

    while True:
        api.wait_update()
        if api.is_changing(klines):
            if len(klines) < VPT_MA_PERIOD + 1:
                continue
            close = klines.close.values
            volume = klines.volume.values

            if len(vpt_values) == 0:
                vpt_values.append(volume[-1])
            else:
                price_change_pct = (close[-1] - close[-2]) / close[-2]
                new_vpt = vpt_values[-1] + volume[-1] * price_change_pct
                vpt_values.append(new_vpt)

            if len(vpt_values) > len(klines):
                vpt_values.pop(0)

            if len(vpt_values) >= VPT_MA_PERIOD:
                vpt_ma = np.mean(vpt_values[-VPT_MA_PERIOD:])
                current_price = float(close[-1])
                current_volume = float(volume[-1])
                avg_volume = np.mean(volume[-VPT_MA_PERIOD:-1])
                volume_increased = current_volume > avg_volume * VOLUME_THRESHOLD
                vpt_trend_up = vpt_values[-1] > vpt_ma
                vpt_trend_down = vpt_values[-1] < vpt_ma

                if position == 0:
                    if vpt_trend_up and volume_increased and close[-1] > close[-2]:
                        position = POSITION_SIZE
                        entry_price = current_price
                        target_pos.set_target_volume(position)
                    elif vpt_trend_down and volume_increased and close[-1] < close[-2]:
                        position = -POSITION_SIZE
                        entry_price = current_price
                        target_pos.set_target_volume(position)
                elif position > 0:
                    if vpt_trend_down and volume_increased:
                        target_pos.set_target_volume(0)
                        position = 0
                elif position < 0:
                    if vpt_trend_up and volume_increased:
                        target_pos.set_target_volume(0)
                        position = 0

except BacktestFinished as e:
    api.close()
    print("回测结束")
```
