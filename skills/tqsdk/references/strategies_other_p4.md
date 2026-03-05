# TqSdk 策略示例 - 均值回归与其他策略 (Part 4)

**包含策略:** Random Forest Price Prediction, ATR Strategy, Turtle Trading Strategy, VWAP Algorithm

---

### 11. Random Forest Price Prediction (应用随机森林对某交易日涨跌情况的预测)

Applies machine learning's random forest algorithm to predict daily price movements in futures trading. Uses technical indicators (SMA, WMA, MOM) calculated from closing prices as features to train a classifier.

```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'limin'

import pandas as pd
import datetime
from contextlib import closing
from tqsdk import TqApi, TqBacktest, BacktestFinished, TargetPosTask
from tqsdk.tafunc import sma, ema2, trma
from sklearn.ensemble import RandomForestClassifier

pd.set_option('display.max_rows', None)  # 设置Pandas显示的行数
pd.set_option('display.width', None)  # 设置Pandas显示的宽度

'''
应用随机森林对某交易日涨跌情况的预测(使用sklearn包)
参考：[JoinQuant文章](https://www.joinquant.com/post/1571)
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''

symbol = "SHFE.ru1811"  # 交易合约代码
close_hour, close_minute = 14, 50  # 预定收盘时间(因为真实收盘后无法进行交易, 所以提前设定收盘时间)


def get_prediction_data(klines, n):
    """获取用于随机森林的n个输入数据(n为数据长度): n天中每天的特征参数及其涨跌情况"""
    close_prices = klines.close[- 30 - n:]  # 获取本交易日及以前的收盘价(此时在预定的收盘时间: 认为本交易日已收盘)
    # 计算所需指标
    sma_data = sma(close_prices, 30, 0.02)[-n:]  # SMA指标, 函数默认时间周期参数:30
    wma_data = ema2(close_prices, 30)[-n:]  # WMA指标
    mom_data = trma(close_prices, 30)[-n:]  # MOM指标
    x_all = list(zip(sma_data, wma_data, mom_data))  # 样本特征组
    y_all = list(klines.close.iloc[i] >= klines.close.iloc[i - 1] for i in list(reversed(range(-1, -n - 1, -1))))  # 样本标签组
    # x_all:            大前天指标 前天指标 昨天指标 (今天指标)
    # y_all:   (大前天)    前天     昨天    今天      -明天-
    # 准备算法需要用到的数据
    x_train = x_all[: -1]  # 训练数据: 特征
    x_predict = x_all[-1]  # 预测数据(用本交易日的指标预测下一交易日的涨跌)
    y_train = y_all[1:]  # 训练数据: 标签 (去掉第一个数据后让其与指标隔一位对齐(例如: 昨天的特征 -> 对应预测今天的涨跌标签))

    return x_train, y_train, x_predict


predictions = []  # 用于记录每次的预测结果(在每个交易日收盘时用收盘数据预测下一交易日的涨跌,并记录在此列表里)
api = TqApi(backtest=TqBacktest(start_dt=datetime.date(2018, 7, 2), end_dt=datetime.date(2018, 9, 26)))
quote = api.get_quote(symbol)
klines = api.get_kline_serial(symbol, duration_seconds=24 * 60 * 60)  # 日线
target_pos = TargetPosTask(api, symbol)
with closing(api):
    try:
        while True:
            while not api.is_changing(klines.iloc[-1], "datetime"):  # 等到达下一个交易日
                api.wait_update()
            while True:
                api.wait_update()
                # 在收盘后预测下一交易日的涨跌情况
                if api.is_changing(quote, "datetime"):
                    now = datetime.datetime.strptime(quote.datetime, "%Y-%m-%d %H:%M:%S.%f")  # 当前quote的时间
                    # 判断是否到达预定收盘时间: 如果到达 则认为本交易日收盘, 此时预测下一交易日的涨跌情况, 并调整为对应仓位
                    if now.hour == close_hour and now.minute >= close_minute:
                        # 1- 获取数据
                        x_train, y_train, x_predict = get_prediction_data(klines, 75)  # 参数1: K线, 参数2:需要的数据长度

                        # 2- 利用机器学习算法预测下一个交易日的涨跌情况
                        # n_estimators 参数: 选择森林里（决策）树的数目; bootstrap 参数: 选择建立决策树时，是否使用有放回抽样
                        clf = RandomForestClassifier(n_estimators=30, bootstrap=True)
                        clf.fit(x_train, y_train)  # 传入训练数据, 进行参数训练
                        predictions.append(bool(clf.predict([x_predict])))  # 传入测试数据进行预测, 得到预测的结果

                        # 3- 进行交易
                        if predictions[-1] == True:  # 如果预测结果为涨: 买入
                            print(quote.datetime, "预测下一交易日为 涨")
                            target_pos.set_target_volume(10)
                        else:  # 如果预测结果为跌: 卖出
                            print(quote.datetime, "预测下一交易日为 跌")
                            target_pos.set_target_volume(-10)
                        break

    except BacktestFinished:  # 回测结束, 获取预测结果，统计正确率
        klines["pre_close"] = klines["close"].shift(1)  # 增加 pre_close(上一交易日的收盘价) 字段
        klines = klines[-len(predictions) + 1:]  # 取出在回测日期内的K线数据
        klines["prediction"] = predictions[:-1]  # 增加预测的本交易日涨跌情况字段(向后移一个数据目的: 将 本交易日对应下一交易日的涨跌 调整为 本交易日对应本交易日的涨跌)
        results = (klines["close"] - klines["pre_close"] >= 0) == klines["prediction"]

        print(klines)
        print("----回测结束----")
        print("预测结果正误:\n", results)
        print("预测结果数目统计: 总计", len(results),"个预测结果")
        print(pd.value_counts(results))
        print("预测的准确率:")
        print((pd.value_counts(results)[True]) / len(results))
```

---

---

### 12. ATR Strategy (真实波幅均值 / Average True Range)

ATR is a technical indicator that calculates the moving average of price volatility over a specific period. It helps traders manage position sizing, set dynamic stop losses, and adjust positions based on market volatility.

**Code Block 1: Capital Allocation Example**
```python
from tqsdk import TqApi
from tqsdk.ta import ATR

api = TqApi()
# 创建SHFE.au1912和DCE.i2001的日K线引用
klines1 = api.get_kline_serial("SHFE.au1912",86400)
klines2 = api.get_kline_serial("DCE.i2001",86400)
# 计算SHFE.au1912和DCE.i2001的ATR值
atr1 = ATR(klines1,14) 
atr2 = ATR(klines2,14)


# 预计输出为SHFE.au1912的ATR为： 6.617857142857141 DCE.i2001的ATR为： 52.642857142857146
print("SHFE.au1912的ATR为：",atr1.atr.iloc[-1],"DCE.i2001的ATR为：",atr2.atr.iloc[-1])
# 假设资产为100万，输出等于总资产1%的1个ATR波动的手数
print("SHFE.au1912的总资产1%的1个ATR波动为",10000/(atr1.atr.iloc[-1]*1000),"DCE.i2001的总资产1%的1个ATR波动为",10000/(atr2.atr.iloc[-1]*100))

api.close()
```

**Code Block 2: ATR Calculation**
```python
# 获取 SHFE.au1912 合约的平均真实波幅，导入TqApi和对应的技术指标函数ATR
from tqsdk import TqApi 
from tqsdk.ta import ATR

api = TqApi()
klines = api.get_kline_serial("SHFE.au1912", 24 * 60 * 60)
atr = ATR(klines, 14)
print(atr.tr)  # 真实波幅
print(atr.atr)  # 真实波幅均值
# 预计的输出是这样的:
[..., 143.0, 48.0, 80.0, ...]
[..., 95.20000000000005, 92.0571428571429, 95.21428571428575, ...]
```

**Code Block 3: Complete Strategy Implementation**
```python
#!/usr/bin/env python 
# -*- coding: utf-8 -*- 
__author__ = "Ringo"

from tqsdk import TqApi,TargetPosTask
from tqsdk.ta import ATR

# ART计算使用天数
ART_DAY_LENGTH = 20 
# 合约代码
SYMBOL = "SHFE.au1912" 

api = TqApi()
klines = api.get_kline_serial(SYMBOL,24*60*60,100)
target_pos = TargetPosTask(api, SYMBOL)
position = api.get_position(SYMBOL)
quote = api.get_quote(SYMBOL)
# 真实波幅均值(n值)
n = ATR(klines,ART_DAY_LENGTH)["atr"].iloc[-1] 
# 使用ATR技术指标加仓或止损前，账户下单多头1手
order = api.insert_order(SYMBOL,"BUY", "OPEN",1) 

while True:
    api.wait_update()
    # 判断多头1手委托单已经完成
    if order.status == "FINISHED" and order.volume_left == 0: 
        if api.is_changing(quote, "last_price"):
            print("最新价: ", quote.last_price)
            # 判断是否持多单
            if position.pos_long > 0:  
                # 加仓策略: 如果是多仓且行情最新价在建仓的基础上涨了0.5n，就调整仓位为目标仓位多头2手
                if quote.last_price >= order.trade_price + 0.5 *n :
                    print("目标仓位多头2手")
                    target_pos.set_target_volume(2)
                # 止损策略: 如果是多仓且行情最新价在建仓的基础上下跌了2n，就卖出全部头寸止损
                elif quote.last_price <= order.trade_price - 2 * n:
                    print("止损:卖出全部头寸")
                    target_pos.set_target_volume(0)
```

---

---

### 13. Turtle Trading Strategy (海龟交易法则)

A famous public trading system that covers all aspects of trading with no room for subjective decision-making. A complete trend-following automated trading strategy using Donchian channels, ATR-based position sizing, and systematic add-on/stop-loss rules.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = 'limin'

'''
海龟策略
注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''

import json
import time
from tqsdk import TqApi, TargetPosTask
from tqsdk.ta import ATR


class Turtle:
    def __init__(self, symbol, account=None, donchian_channel_open_position=20, donchian_channel_stop_profit=10, atr_day_length=20, max_risk_ratio=0.5):
        self.account = account  # 交易账号
        self.symbol = symbol  # 合约代码
        self.donchian_channel_open_position = donchian_channel_open_position  # 唐奇安通道的天数周期(开仓)
        self.donchian_channel_stop_profit = donchian_channel_stop_profit  # 唐奇安通道的天数周期(止盈)
        self.atr_day_length = atr_day_length  # ATR计算所用天数
        self.max_risk_ratio = max_risk_ratio  # 最高风险度
        self.state = {
            "position": 0,  # 本策略净持仓数(正数表示多头，负数表示空头，0表示空仓)
            "last_price": float("nan"),  # 上次调仓价
        }

        self.n = 0  # 平均真实波幅(N值)
        self.unit = 0  # 买卖单位
        self.donchian_channel_high = 0  # 唐奇安通道上轨
        self.donchian_channel_low = 0  # 唐奇安通道下轨

        self.api = TqApi(self.account)
        self.quote = self.api.get_quote(self.symbol)
        # 由于ATR是路径依赖函数，因此使用更长的数据序列进行计算以便使其值稳定下来
        kline_length = max(donchian_channel_open_position + 1, donchian_channel_stop_profit + 1, atr_day_length * 5)
        self.klines = self.api.get_kline_serial(self.symbol, 24 * 60 * 60, data_length=kline_length)
        self.account = self.api.get_account()
        self.target_pos = TargetPosTask(self.api, self.symbol)

    def recalc_paramter(self):
        # 平均真实波幅(N值)
        self.n = ATR(self.klines, self.atr_day_length)["atr"].iloc[-1]
        # 买卖单位
        self.unit = int((self.account.balance * 0.01) / (self.quote.volume_multiple * self.n))
        # 唐奇安通道上轨：前N个交易日的最高价
        self.donchian_channel_high = max(self.klines.high[-self.donchian_channel_open_position - 1:-1])
        # 唐奇安通道下轨：前N个交易日的最低价
        self.donchian_channel_low = min(self.klines.low[-self.donchian_channel_open_position - 1:-1])
        print("唐其安通道上下轨: %f, %f" % (self.donchian_channel_high, self.donchian_channel_low))
        return True

    def set_position(self, pos):
        self.state["position"] = pos
        self.state["last_price"] = self.quote["last_price"]
        self.target_pos.set_target_volume(self.state["position"])

    def try_open(self):
        """开仓策略"""
        while self.state["position"] == 0:
            self.api.wait_update()
            if self.api.is_changing(self.klines.iloc[-1], "datetime"):  # 如果产生新k线,则重新计算唐奇安通道及买卖单位
                self.recalc_paramter()
            if self.api.is_changing(self.quote, "last_price"):
                print("最新价: %f" % self.quote.last_price)
                if self.quote.last_price > self.donchian_channel_high:  # 当前价>唐奇安通道上轨，买入1个Unit；(持多仓)
                    print("当前价>唐奇安通道上轨，买入1个Unit(持多仓): %d 手" % self.unit)
                    self.set_position(self.state["position"] + self.unit)
                elif self.quote.last_price < self.donchian_channel_low:  # 当前价<唐奇安通道下轨，卖出1个Unit；(持空仓)
                    print("当前价<唐奇安通道下轨，卖出1个Unit(持空仓): %d 手" % self.unit)
                    self.set_position(self.state["position"] - self.unit)

    def try_close(self):
        """交易策略"""
        while self.state["position"] != 0:
            self.api.wait_update()
            if self.api.is_changing(self.quote, "last_price"):
                print("最新价: ", self.quote.last_price)
                if self.state["position"] > 0:  # 持多单
                    # 加仓策略: 如果是多仓且行情最新价在上一次建仓（或者加仓）的基础上又上涨了0.5N，就再加一个Unit的多仓,并且风险度在设定范围内(以防爆仓)
                    if self.quote.last_price >= self.state["last_price"] + 0.5 * self.n and self.account.risk_ratio <= self.max_risk_ratio:
                        print("加仓:加1个Unit的多仓")
                        self.set_position(self.state["position"] + self.unit)
                    # 止损策略: 如果是多仓且行情最新价在上一次建仓（或者加仓）的基础上又下跌了2N，就卖出全部头寸止损
                    elif self.quote.last_price <= self.state["last_price"] - 2 * self.n:
                        print("止损:卖出全部头寸")
                        self.set_position(0)
                    # 止盈策略: 如果是多仓且行情最新价跌破了10日唐奇安通道的下轨，就清空所有头寸结束策略,离场
                    if self.quote.last_price <= min(self.klines.low[-self.donchian_channel_stop_profit - 1:-1]):
                        print("止盈:清空所有头寸结束策略,离场")
                        self.set_position(0)

                elif self.state["position"] < 0:  # 持空单
                    # 加仓策略: 如果是空仓且行情最新价在上一次建仓（或者加仓）的基础上又下跌了0.5N，就再加一个Unit的空仓,并且风险度在设定范围内(以防爆仓)
                    if self.quote.last_price <= self.state["last_price"] - 0.5 * self.n and self.account.risk_ratio <= self.max_risk_ratio:
                        print("加仓:加1个Unit的空仓")
                        self.set_position(self.state["position"] - self.unit)
                    # 止损策略: 如果是空仓且行情最新价在上一次建仓（或者加仓）的基础上又上涨了2N，就平仓止损
                    elif self.quote.last_price >= self.state["last_price"] + 2 * self.n:
                        print("止损:卖出全部头寸")
                        self.set_position(0)
                    # 止盈策略: 如果是空仓且行情最新价升破了10日唐奇安通道的上轨，就清空所有头寸结束策略,离场
                    if self.quote.last_price >= max(self.klines.high[-self.donchian_channel_stop_profit - 1:-1]):
                        print("止盈:清空所有头寸结束策略,离场")
                        self.set_position(0)

    def strategy(self):
        """海龟策略"""
        print("等待K线及账户数据...")
        deadline = time.time() + 5
        while not self.recalc_paramter():
            if not self.api.wait_update(deadline=deadline):
                raise Exception("获取数据失败，请确认行情连接正常并已经登录交易账户")
        while True:
            self.try_open()
            self.try_close()


turtle = Turtle("SHFE.hc1901")
print("策略开始运行")
try:
    turtle.state = json.load(open("turtle_state.json", "r"))  # 读取数据: 本策略目标净持仓数,上一次开仓价
except FileNotFoundError:
    pass
print("当前持仓数: %d, 上次调仓价: %f" % (turtle.state["position"], turtle.state["last_price"]))
try:
    turtle.strategy()
finally:
    turtle.api.close()
    json.dump(turtle.state, open("turtle_state.json", "w"))  # 保存数据
```

---

---

### 14. VWAP Algorithm (VWAP算法 / 成交量加权平均价格)

A passive algorithmic trading strategy that splits large orders into smaller ones based on predicted trading volume distribution to reduce market impact costs. Divides trading time into equal intervals and executes orders proportionally to historical volume patterns.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__author__ = 'limin'

'''
Volume Weighted Average Price策略

注: 该示例策略仅用于功能示范, 实盘时请根据自己的策略/经验进行修改
'''

import datetime
from tqsdk import TqApi, TargetPosTask

TIME_CELL = 5*60  # 等时长下单的时间单元, 单位: 秒
TARGET_VOLUME = 300  # 目标交易手数 (>0: 多头, <0: 空头)
SYMBOL = "DCE.jd1909"  # 交易合约代码
HISTORY_DAY_LENGTH = 20  # 使用多少天的历史数据用来计算每个时间单元的下单手数
START_HOUR, START_MINUTE = 9, 35  # 计划交易时段起始时间点
END_HOUR, END_MINUTE = 10, 50  # 计划交易时段终点时间点

api = TqApi()
print("策略开始运行")
# 根据 HISTORY_DAY_LENGTH 推算出需要订阅的历史数据长度, 需要注意history_day_length与time_cell的比例关系以避免超过订阅限制
time_slot_start = datetime.time(START_HOUR, START_MINUTE)  # 计划交易时段起始时间点
time_slot_end = datetime.time(END_HOUR, END_MINUTE)  # 计划交易时段终点时间点
klines = api.get_kline_serial(SYMBOL, TIME_CELL, data_length=int(10*60*60/TIME_CELL*HISTORY_DAY_LENGTH))
target_pos = TargetPosTask(api, SYMBOL)
position = api.get_position(SYMBOL)  # 持仓信息

def get_kline_time(kline_datetime):
    """获取k线的时间(不包含日期)"""
    kline_time = datetime.datetime.fromtimestamp(kline_datetime//1000000000).time()  # 每根k线的时间
    return kline_time

def get_market_day(kline_datetime):
    """获取k线所对应的交易日"""
    kline_dt = datetime.datetime.fromtimestamp(kline_datetime//1000000000)  # 每根k线的日期和时间
    if kline_dt.hour >= 18:  # 当天18点以后: 移到下一个交易日
        kline_dt = kline_dt + datetime.timedelta(days=1)
    while kline_dt.weekday() >= 5:  # 是周六或周日,移到周一
        kline_dt = kline_dt + datetime.timedelta(days=1)
    return kline_dt.date()

# 添加辅助列: time及date, 分别为K线时间的时:分:秒和其所属的交易日
klines["time"] = klines.datetime.apply(lambda x: get_kline_time(x))
klines["date"] = klines.datetime.apply(lambda x: get_market_day(x))

# 获取在预设交易时间段内的所有K线, 即时间位于 time_slot_start 到 time_slot_end 之间的数据
if time_slot_end > time_slot_start:  # 判断是否类似 23:00:00 开始， 01:00:00 结束这样跨天的情况
    klines = klines[(klines["time"] >= time_slot_start) & (klines["time"] <= time_slot_end)]
else:
    klines = klines[(klines["time"] >= time_slot_start) | (klines["time"] <= time_slot_end)]

# 由于可能有节假日导致部分天并没有填满整个预设交易时间段
# 因此去除缺失部分交易时段的日期(即剩下的每个日期都包含预设的交易时间段内所需的全部时间单元)
date_cnt = klines["date"].value_counts()
max_num = date_cnt.max()  # 所有日期中最完整的交易时段长度
need_date = date_cnt[date_cnt == max_num].sort_index().index[-HISTORY_DAY_LENGTH - 1:-1]  # 获取今天以前的预设数目个交易日的日期
df = klines[klines["date"].isin(need_date)]  # 最终用来计算的k线数据

# 计算每个时间单元的成交量占比, 并使用算数平均计算出预测值
datetime_grouped = df.groupby(['date', 'time'])['volume'].sum()  # 将K线的volume按照date、time建立多重索引分组
# 计算每个交易日内的预设交易时间段内的成交量总和(level=0: 表示按第一级索引"data"来分组)后,将每根k线的成交量除以所在交易日内的总成交量,计算其所占比例
volume_percent = datetime_grouped / datetime_grouped.groupby(level=0).sum()
predicted_percent = volume_percent.groupby(level=1).mean()  # 将历史上相同时间单元的成交量占比使用算数平均计算出预测值
print("各时间单元成交量占比: %s" % predicted_percent)

# 计算每个时间单元的成交量预测值
predicted_volume = {}  # 记录每个时间单元需调整的持仓量
percentage_left = 1  # 剩余比例
volume_left = TARGET_VOLUME  # 剩余手数
for index, value in predicted_percent.items():
    volume = round(volume_left*(value/percentage_left))
    predicted_volume[index] = volume
    percentage_left -= value
    volume_left -= volume
print("各时间单元应下单手数: %s" % predicted_volume)

# 交易
current_volume = 0  # 记录已调整持仓量
while True:
    api.wait_update()
    # 新产生一根K线并且在计划交易时间段内: 调整目标持仓量
    if api.is_changing(klines.iloc[-1], "datetime"):
        t = datetime.datetime.fromtimestamp(klines.iloc[-1]["datetime"]//1000000000).time()
        if t in predicted_volume:
            current_volume += predicted_volume[t]
            print("到达下一时间单元,调整持仓为: %d" % current_volume)
            target_pos.set_target_volume(current_volume)
    # 用持仓信息判断是否完成所有目标交易手数
    if api.is_changing(position, "volume_long") or api.is_changing(position, "volume_short"):
        if position["volume_long"] - position["volume_short"] == TARGET_VOLUME:
            break
api.close()
```

---