# Api Ref - Part 5

**Topics:** 交易策略示例

---

## 交易策略示例

**URL:** https://doc.shinnytech.com/tqsdk/latest/demo/strategy.html

**Contents:**
- 交易策略示例
- 经典策略
  - Aberration 策略 (难度：初级)
  - Doublema 双均线策略 (难度：初级)
  - 价格动量策略 (难度：初级)
  - 自动扶梯策略 (难度：初级)
  - 菲阿里四价 策略 (难度：初级)
  - R-Breaker 交易策略 - 隔夜留仓 (难度：初级)
  - R-Breaker 交易策略 - 非隔夜留仓 (难度：初级)
  - Dual Thrust 策略 (难度：中级)

Aberration 策略 (难度：初级)

Doublema 双均线策略 (难度：初级)

R-Breaker 交易策略 - 隔夜留仓 (难度：初级)

R-Breaker 交易策略 - 非隔夜留仓 (难度：初级)

Dual Thrust 策略 (难度：中级)

网格交易策略 - 异步代码 (难度：中级)

Volume Weighted Average Price 策略 (难度：高级)

三重指数平滑平均线 (TRIX) 趋势策略 (难度：初级)

基于钱德动量震荡指标(CMO)的期货量化交易策略 (难度：初级)

Keltner Channel策略 (难度：初级)

PTA产业链套利 (涤纶短纤生产利润)策略 (难度：初级)

双焦（焦煤-焦炭）套利策略 (难度：初级)

甲醇制烯烃（MTO）产业链套利策略 (难度：初级)

菜油、豆油、棕榈油多品种套利策略 (难度：初级)

Z-Score均值回归策略 (难度：初级)

策略说明 https://www.shinnytech.com/blog/aberration/

策略说明 https://www.shinnytech.com/blog/momentum-strategy/

策略说明 https://www.shinnytech.com/blog/escalator/

策略说明 https://www.shinnytech.com/blog/fairy-four-price/

策略说明 https://www.shinnytech.com/blog/r-breaker/

策略说明 https://www.shinnytech.com/blog/r-breaker/

策略说明 https://www.shinnytech.com/blog/dual-thrust/

策略说明 https://www.shinnytech.com/blog/grid-trading/

策略说明 https://www.shinnytech.com/blog/grid-trading/

策略说明 https://www.shinnytech.com/blog/turtle/

策略说明 https://www.shinnytech.com/blog/vwap/

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/trix-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/fractal-trend-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/cmo-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/vortex-indicator-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/hull-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/keltner-channel-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/qstick-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/aroon-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/trend-following/vpt-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/pta-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/smash-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/jm-j-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/mto-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/cu-al-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/crush-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/crack-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/stock-future-cross-period-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/rb-i-jt-spread

策略说明 https://www.shinnytech.com/articles/trading-strategy/arbitrage/oi-y-p-spreads

策略说明 https://www.shinnytech.com/articles/trading-strategy/mean-reversion/kalman-filter-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/mean-reversion/distance-based-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/mean-reversion/pivot-point-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/mean-reversion/cci-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/mean-reversion/rsi-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/mean-reversion/zscore-strategy

策略说明 https://www.shinnytech.com/articles/trading-strategy/other/iceberg-order

策略说明 https://www.shinnytech.com/articles/trading-strategy/other/handicap

策略说明 https://www.shinnytech.com/articles/trading-strategy/other/pov

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = "Ringo"

'''
Aberration策略 (难度：初级)
参考: https://www.shinnytech.com/blog/aberration/
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''

from tqsdk import TqApi, TqAuth, TargetPosTask
from tqsdk.ta import BOLL

# 设置合约代码
SYMBOL = "DCE.m2105"
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
quote = api.get_quote(SYMBOL)
klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)
position = api.get_position(SYMBOL)
target_pos = TargetPosTask(api, SYMBOL)


# 使用BOLL指标计算中轨、上轨和下轨，其中26为周期N  ，2为参数p
def boll_line(klines):
    boll = BOLL(klines, 26, 2)
    midline = boll["mid"].iloc[-1]
    topline = boll["top"].iloc[-1]
    bottomline = boll["bottom"].iloc[-1]
    print("策略运行，中轨：%.2f，上轨为:%.2f，下轨为:%.2f" % (midline, topline, bottomline))
    return midline, topline, bottomline


midline, topline, bottomline = boll_line(klines)

while True:
    api.wait_update()
    # 每次生成新的K线时重新计算BOLL指标
    if api.is_changing(klines.iloc[-1], "datetime"):
        midline, topline, bottomline = boll_line(klines)

    # 每次最新价发生变化时进行判断
    if api.is_changing(quote, "last_price"):
        # 判断开仓条件
        if position.pos_long == 0 and position.pos_short == 0:
            # 如果最新价大于上轨，K线上穿上轨，开多仓
            if quote.last_price > topline:
                print("K线上穿上轨，开多仓")
                target_pos.set_target_volume(20)
            # 如果最新价小于轨，K线下穿下轨，开空仓
            elif quote.last_price < bottomline:
                print("K线下穿下轨，开空仓")
                target_pos.set_target_volume(-20)
            else:
                print("当前最新价%.2f,未穿上轨或下轨，不开仓" % quote.last_price)

        # 在多头情况下，空仓条件
        elif position.pos_long > 0:
            # 如果最新价低于中线，多头清仓离场
            if quote.last_price < midline:
                print("最新价低于中线，多头清仓离场")
                target_pos.set_target_volume(0)
            else:
                print("当前多仓，未穿越中线，仓位无变化")

        # 在空头情况下，空仓条件
        elif position.pos_short > 0:
            # 如果最新价高于中线，空头清仓离场
            if quote.last_price > midline:
                print("最新价高于中线，空头清仓离场")
                target_pos.set_target_volume(0)
            else:
                print("当前空仓，未穿越中线，仓位无变化")
```

Example 2 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'limin'

'''
双均线策略
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''
from tqsdk import TqApi, TqAuth, TargetPosTask
from tqsdk.tafunc import ma

SHORT = 30  # 短周期
LONG = 60  # 长周期
SYMBOL = "SHFE.bu2012"  # 合约代码

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
print("策略开始运行")

data_length = LONG + 2  # k线数据长度
# "duration_seconds=60"为一分钟线, 日线的duration_seconds参数为: 24*60*60
klines = api.get_kline_serial(SYMBOL, duration_seconds=60, data_length=data_length)
target_pos = TargetPosTask(api, SYMBOL)

while True:
    api.wait_update()

    if api.is_changing(klines.iloc[-1], "datetime"):  # 产生新k线:重新计算SMA
        short_avg = ma(klines["close"], SHORT)  # 短周期
        long_avg = ma(klines["close"], LONG)  # 长周期

        # 均线下穿，做空
        if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
            target_pos.set_target_volume(-3)
            print("均线下穿，做空")

        # 均线上穿，做多
        if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
            target_pos.set_target_volume(3)
            print("均线上穿，做多")
```

Example 3 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = "Ringo"

'''
价格动量 策略 (难度：初级)
参考: https://www.shinnytech.com/blog/momentum-strategy/
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''

from tqsdk import TqApi, TqAuth, TargetPosTask

# 设置指定合约,获取N条K线计算价格动量
SYMBOL = "SHFE.au2012"
N = 15

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24, N)
quote = api.get_quote(SYMBOL)
target_pos = TargetPosTask(api, SYMBOL)
position = api.get_position(SYMBOL)


def AR(kline1):
    """价格动量函数AR，以前N-1日K线计算价格动量ar"""
    spread_ho = sum(kline1.high[:-1] - kline1.open[:-1])
    spread_oc = sum(kline1.open[:-1] - kline1.low[:-1])
    # spread_oc 为0时，设置为最小价格跳动值
    if spread_oc == 0:
        spread_oc = quote.price_tick
    ar = (spread_ho / spread_oc) * 100
    return ar


ar = AR(klines)
print("策略开始启动")

while True:
    api.wait_update()
    # 生成新K线时，重新计算价格动量值ar
    if api.is_changing(klines.iloc[-1], "datetime"):
        ar = AR(klines)
        print("价格动量是：", ar)
    # 每次最新价发生变动时，重新进行判断
    if api.is_changing(quote, "last_price"):
        # 开仓策略
        if position.pos_long == 0 and position.pos_short == 0:
            # 如果ar大于110并且小于150，开多仓
            if 110 < ar < 150:
                print("价值动量超过110，小于150，做多")
                target_pos.set_target_volume(100)
            # 如果ar大于50，小于90，开空仓
            elif 50 < ar < 90:
                print("价值动量大于50，小于90，做空")
                target_pos.set_target_volume(-100)

        # 止损策略，多头下当前ar值小于90则平仓止损，空头下当前ar值大于110则平仓止损
        elif (position.pos_long > 0 and ar < 90) or (position.pos_short > 0 and ar > 110):
            print("止损平仓")
            target_pos.set_target_volume(0)
```

Example 4 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = "Ringo"

'''
自动扶梯 策略 (难度：初级)
参考: https://www.shinnytech.com/blog/escalator/
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''

from tqsdk import TqApi, TqAuth, TargetPosTask
from tqsdk.ta import MA

# 设置合约
SYMBOL = "SHFE.rb2012"
# 设置均线长短周期
MA_SLOW, MA_FAST = 8, 40

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
klines = api.get_kline_serial(SYMBOL, 60 * 60 * 24)
quote = api.get_quote(SYMBOL)
position = api.get_position(SYMBOL)
target_pos = TargetPosTask(api, SYMBOL)


# K线收盘价在这根K线波动范围函数


def kline_range(num):
    kl_range = (klines.iloc[num].close - klines.iloc[num].low) / \
               (klines.iloc[num].high - klines.iloc[num].low)
    return kl_range


# 获取长短均线值
def ma_caculate(klines):
    ma_slow = MA(klines, MA_SLOW).iloc[-1].ma
    ma_fast = MA(klines, MA_FAST).iloc[-1].ma
    return ma_slow, ma_fast


ma_slow, ma_fast = ma_caculate(klines)
print("慢速均线为%.2f,快速均线为%.2f" % (ma_slow, ma_fast))

while True:
    api.wait_update()
    # 每次k线更新，重新计算快慢均线
    if api.is_changing(klines.iloc[-1], "datetime"):
        ma_slow, ma_fast = ma_caculate(klines)
        print("慢速均线为%.2f,快速均线为%.2f" % (ma_slow, ma_fast))

    if api.is_changing(quote, "last_price"):
        # 开仓判断
        if position.pos_long == 0 and position.pos_short == 0:
            # 计算前后两根K线在当时K线范围波幅
            kl_range_cur = kline_range(-2)
            kl_range_pre = kline_range(-3)
            # 开多头判断，最近一根K线收盘价在短期均线和长期均线之上，前一根K线收盘价位于K线波动范围底部25%，最近这根K线收盘价位于K线波动范围顶部25%
            if klines.iloc[-2].close > max(ma_slow, ma_fast) and kl_range_pre <= 0.25 and kl_range_cur >= 0.75:
                print("最新价为:%.2f 开多头" % quote.last_price)
                target_pos.set_target_volume(100)

            # 开空头判断，最近一根K线收盘价在短期均线和长期均线之下，前一根K线收盘价位于K线波动范围顶部25%，最近这根K线收盘价位于K线波动范围底部25%
            elif klines.iloc[-2].close < min(ma_slow, ma_fast) and kl_range_pre >= 0.75 and kl_range_cur <= 0.25:
                print("最新价为:%.2f 开空头" % quote.last_price)
                target_pos.set_target_volume(-100)
            else:
                print("最新价位:%.2f ，未满足开仓条件" % quote.last_price)

        # 多头持仓止损策略
        elif position.pos_long > 0:
            # 在两根K线较低点减一跳，进行多头止损
            kline_low = min(klines.iloc[-2].low, klines.iloc[-3].low)
            if klines.iloc[-1].close <= kline_low - quote.price_tick:
                print("最新价为:%.2f,进行多头止损" % (quote.last_price))
                target_pos.set_target_volume(0)
            else:
                print("多头持仓，当前价格 %.2f,多头离场价格%.2f" %
                      (quote.last_price, kline_low - quote.price_tick))

        # 空头持仓止损策略
        elif position.pos_short > 0:
            # 在两根K线较高点加一跳，进行空头止损
            kline_high = max(klines.iloc[-2].high, klines.iloc[-3].high)
            if klines.iloc[-1].close >= kline_high + quote.price_tick:
                print("最新价为:%.2f 进行空头止损" % quote.last_price)
                target_pos.set_target_volume(0)
            else:
                print("空头持仓，当前价格 %.2f,空头离场价格%.2f" %
                      (quote.last_price, kline_high + quote.price_tick))
```

---

