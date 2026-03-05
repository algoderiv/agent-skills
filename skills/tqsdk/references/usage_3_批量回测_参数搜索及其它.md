# Usage - Part 3

**Topics:** 批量回测, 参数搜索及其它, 策略程序结构, tqsdk.TqBacktest - 策略回测, 技术指标与序列计算函数, 基本使用

---

## 批量回测, 参数搜索及其它

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/backtest.html

**Contents:**
- 批量回测, 参数搜索及其它
- 参数优化/参数搜索
- 多进程并发执行多个回测任务

在阅读本文档前, 请确保您已经熟悉了 策略程序回测

TqSdk 并不提供专门的参数优化机制. 您可以按照自己的需求, 针对可能的每个参数值安排一个回测, 观察它们的回测结果, 以简单的双均线策略为例:

如果您有大量回测任务想要尽快完成, 您首先需要一台给力的电脑(可以考虑到XX云上租一台32核的, 一小时几块钱). 然后您就可以并发执行N个回测了. 还是以上面的策略为例:

注意: 由于服务器流控限制, 同时执行的回测任务请勿超过10个

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth, TqSim, TargetPosTask, BacktestFinished, TqBacktest
from tqsdk.tafunc import ma
from datetime import date

LONG = 60
SYMBOL = "SHFE.cu1907"

for SHORT in range(20, 40): # 短周期参数从20-40分别做回测
  acc = TqSim()             # 每次回测都创建一个新的模拟账户
  try:
    api = TqApi(acc, backtest=TqBacktest(start_dt=date(2019, 5, 6), end_dt=date(2019, 5, 10)), auth=TqAuth("快期账户", "账户密码"))
    account = api.get_account()
    klines = api.get_kline_serial(SYMBOL, duration_seconds=60, data_length=LONG + 2)
    target_pos = TargetPosTask(api, SYMBOL)
    while True:
      api.wait_update()
      if api.is_changing(klines.iloc[-1], "datetime"):
        short_avg = ma(klines.close, SHORT)
        long_avg = ma(klines.close, LONG)
        if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
          target_pos.set_target_volume(-1)
        if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
          target_pos.set_target_volume(1)
  except BacktestFinished:
    api.close()
    print("SHORT=", SHORT, "最终权益=", account["balance"])   # 每次回测结束时, 输出使用的参数和最终权益
```

Example 2 (python):
```python
from tqsdk import TqApi, TqAuth, TqSim, TargetPosTask, BacktestFinished, TqBacktest
from tqsdk.tafunc import ma
from datetime import date
import multiprocessing
from multiprocessing import Pool

def MyStrategy(SHORT):
  LONG = 60
  SYMBOL = "SHFE.cu1907"
  acc = TqSim()
  try:
    api = TqApi(acc, backtest=TqBacktest(start_dt=date(2019, 5, 6), end_dt=date(2019, 5, 10)), auth=TqAuth("快期账户", "账户密码"))
    data_length = LONG + 2
    klines = api.get_kline_serial(SYMBOL, duration_seconds=60, data_length=data_length)
    target_pos = TargetPosTask(api, SYMBOL)
    while True:
      api.wait_update()
      if api.is_changing(klines.iloc[-1], "datetime"):
        short_avg = ma(klines.close, SHORT)
        long_avg = ma(klines.close, LONG)
        if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
          target_pos.set_target_volume(-3)
        if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
          target_pos.set_target_volume(3)
  except BacktestFinished:
    api.close()
    print("SHORT=", SHORT, "最终权益=", acc.get_account().balance)  # 每次回测结束时, 输出使用的参数和最终权益


if __name__ == '__main__':
  multiprocessing.freeze_support()
  p = Pool(4)                               # 进程池, 建议小于cpu数
  for s in range(20, 40):
    p.apply_async(MyStrategy, args=(s,))  # 把20个回测任务交给进程池执行
  print('Waiting for all subprocesses done...')
  p.close()
  p.join()
  print('All subprocesses done.')
```

---


## 策略程序结构

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/framework.html

**Contents:**
- 策略程序结构
- TqApi
- 关键函数: wait_update
- 内存数据及数据更新
- 一个典型程序的结构

tqsdk.TqApi 是 TqSdk 的核心类. 通常情况下, 每个使用了 TqSdk 的程序都应该包括 一个 TqApi 实例:

在内存中建立数据存储区, 接收行情和交易业务数据包, 并自动维护数据更新.

TqApi 创建时, 需要提供一个account参数. 它可以是:

一个 tqsdk.TqAccount 实例: 使用实盘帐号, 直连行情和交易服务器, 需提供期货公司/帐号/密码

一个 tqsdk.TqSim 实例: 使用 Api 自带的模拟功能, 直连行情服务器接收行情数据

如果未提供 account 参数, 或者 account == None, 则会自动创建并使用一个 tqsdk.TqSim 实例

此外还需要传入用户的快期账户，参见 快期账户

TqApi 的其它构建参数请见 tqsdk.TqApi

wait_update() 是 TqApi 中最重要的一个函数. 每次调用 wait_update() 函数时将发生这些事:

实际发出网络数据包. 例如, 策略程序用 insert_order 函数下单, 实际的报单指令是在 insert_order 后调用 wait_update() 时发出的

让正在运行中的后台任务获得动作机会．例如, 策略程序中创建了一个后台调仓任务, 这个任务只会在 wait_update() 时发出交易指令

尝试从服务器接收一个数据包, 并用收到的数据包更新内存中的业务数据截面.

如果没有收到数据包，则挂起等待，如果要避免长时间挂起，可通过设置 wait_update() 中的deadline参数，设置等待截止时间

因此, TqSdk 要求策略程序必须反复调用 wait_update(), 才能保证整个程序正常运行. 一般会将 wait_update() 放在一个循环中反复调用 （注: 若跳出循环，程序结束前需调用 api.close() 释放资源):

TqApi 实例内存中保存了一份完整业务数据截面, 包括行情/K线和交易账户数据. 这些数据可以通过 TqApi 提供的数据引用函数获取，以获取资金账户为例:

值得注意的是, get_account 返回资金账户的一个动态引用, 而不是具体的数值. 因此只需调用一次 get_account 得到 account 引用，之后任何时刻都可以使用 account.balance 获得最新的账户权益. 当 wait_update() 函数返回时业务截面即完成了从上一个时间截面推进到下一个时间截面。

wait_update() 会在任何数据更新时返回. 如果想知道 wait_update() 到底更新了哪些业务数据可以调用 is_changing() 函数判断感兴趣的业务对象是否有更新，例如:

合约信息不能及时更新（如：有新上市的合约,保持登录的第二个交易日就没有这个合约信息)

如果使用了交易辅助工具 TargetPosTask 并且收盘后有挂单，导致 TargetPosTask 在下一交易日无法继续执行

以一个通常的策略流程为例：判断开仓条件，开仓，判断平仓条件，平仓，使用 TqSdk 写出的代码:

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (unknown):
```unknown
while True:             #一个循环
    api.wait_update()   #总是调用 wait_update, 当数据有更新时 wait_update 函数返回, 执行下一句
    do_some_thing()     #每当数据有变更时, 执行自己的代码, 然后循环回去再做下一次 wait_update

#注：若跳出循环并运行到程序末尾，在结束运行前需调用 api.close() 函数以关闭天勤接口实例并释放相应资源，请见下文 “一个典型程序的结构”
```

Example 3 (swift):
```swift
account = api.get_account()  # 获取账户信息引用
print(account.balance)    # 显示账户信息
```

Example 4 (python):
```python
if api.is_changing(account):
    print("账户变化")                    #任何资金账户中任意信息变化的时候打出 "账户变化"

if api.is_changing(account, "balance"):
    print("账户权益变化")                    #只有资金账户中的权益值变化的时候打出 "账户权益变化"
```

---


## tqsdk.TqBacktest - 策略回测

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.backtest.html

**Contents:**
- tqsdk.TqBacktest - 策略回测

将该类传入 TqApi 的构造函数, 则策略就会进入回测模式。

回测模式下 k线会在刚创建出来时和结束时分别更新一次, 在这之间 k线是不会更新的。

只要订阅了 tick, 则对应合约的 quote 就会使用 tick 生成, 更新频率也和 tick 一致, 但 只有下字段 : datetime/ask&bid_price1/ask&bid_volume1/last_price/highest/lowest/average/volume/amount/open_interest/ price_tick/price_decs/volume_multiple/max&min_limit&market_order_volume/underlying_symbol/strike_price

如果没有订阅 tick, 但是订阅了 k线, 则对应合约的 quote 会使用 k线生成, 更新频率和 k线的周期一致， 如果订阅了某个合约的多个周期的 k线, 则任一个周期的 k线有更新时, quote 都会更新. 使用 k线生成的 quote 的盘口由收盘价分别加/减一个最小变动单位, 并且 highest/lowest/average/amount 始终为 nan, volume 始终为0

如果即没有订阅 tick, 也没有订阅k线或 订阅的k线周期大于分钟线, 则 TqBacktest 会 自动订阅分钟线 来生成 quote

如果没有订阅 tick, 但是订阅了 k线, 则对应合约的 quote 只有下字段 : datetime/ask&bid_price1/ask&bid_volume1/last_price/open_interest/ price_tick/price_decs/volume_multiple/max&min_limit&market_order_volume/underlying_symbol/strike_price

注意 ：如果未订阅 quote，模拟交易在下单时会自动为此合约订阅 quote ，根据回测时 quote 的更新规则，如果此合约没有订阅K线或K线周期大于分钟线 则会自动订阅一个分钟线 。

模拟交易要求报单价格大于等于对手盘价格才会成交, 例如下买单, 要求价格大于等于卖一价才会成交, 如果不能立即成交则会等到下次行情更新再重新判断。

回测模式下 wait_update 每次最多推进一个行情时间。

回测结束后会抛出 BacktestFinished 例外。

对 组合合约 进行回测时需注意：只能通过订阅 tick 数据来回测，不能订阅K线，因为K线是由最新价合成的，而交易所发回的组合合约数据中无最新价。

创建天勤回测类，起始时间和结束时间都应该是北京时间

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

© 版权所有 2018-2026, TianQin。

---


## 技术指标与序列计算函数

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/ta.html

**Contents:**
- 技术指标与序列计算函数
- 技术指标
- 序列计算函数

tqsdk.ta 模块中包含了大量技术指标. 每个技术指标是一个函数, 函数名为全大写, 第一参数总是K线序列, 以pandas.DataFrame格式返回计算结果. 以MACD为例:

tqsdk.ta 中目前提供的技术指标详表，请见 tqsdk.ta - 技术指标计算函数

tqsdk.tafunc 模块中包含了一批序列计算函数. 它们是构成技术指标的基础. 在某些情况下, 您也可以直接使用这些序列计算函数以获取更大的灵活性.

例如, 技术指标MA(均线)总是按K线的收盘价来计算, 如果你需要计算最高价的均线, 可以使用ma函数:

tqsdk.tafunc 中目前提供的序列计算函数详表，请见 tqsdk.tafunc - 序列计算函数

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (swift):
```swift
from tqsdk.ta import MACD

klines = api.get_kline_serial("SHFE.cu1812", 60)   # 提取SHFE.cu1812的分钟线
result = MACD(klines, 12, 26, 9)                        # 计算MACD指标
print(result["diff"])                                   # MACD指标中的diff序列
```

Example 2 (swift):
```swift
from tqsdk.tafunc import ma

klines = api.get_kline_serial("SHFE.cu1812", 60)   # 提取SHFE.cu1812的分钟线
result = ma(klines.high, 9)                        # 按K线的最高价序列做9分钟的移动平均
print(result)                                      # 移动平均结果
```

---


## 基本使用

**URL:** https://doc.shinnytech.com/tqsdk/latest/demo/base.html

**Contents:**
- 基本使用
- t10 - 获取实时行情
- t20 - 识别行情更新
- t30 - 使用K线/Tick数据
- t40 - 下单/撤单
- t41 - 开仓/平仓
- t50 - 查询交易所合约每日结算价
- t51 - 查询合约成交排名/持仓排名
- t52 - 根据合约类型、交易所、品种等条件查询合约
- t53 - 查询主连合约对应的标的合约列表

t52 - 根据合约类型、交易所、品种等条件查询合约

t53 - 查询主连合约对应的标的合约列表

t57 - 根据条件查询全部的实值、平值、虚值期权

t58 - 针对ETF期权和股指期权查询全部的实值、平值、虚值期权

underlying_symbol - 获取主连映射主力合约

downloader_orders - 下载委托单和成交记录

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'chengzhi'

from tqsdk import TqApi, TqAuth

# 创建API实例,传入自己的快期账户
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
# 获得上期所 ni2206 的行情引用，当行情有变化时 quote 中的字段会对应更新
quote = api.get_quote("SHFE.ni2206")

# 输出 ni2206 的最新行情时间和最新价
print(quote.datetime, quote.last_price)

# 关闭api,释放资源
api.close()
```

Example 2 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'chengzhi'

from tqsdk import TqApi, TqAuth

# 可以指定debug选项将调试信息写入指定的文件中
api = TqApi(debug="debug.log", auth=TqAuth("快期账户", "账户密码"))
quote = api.get_quote("CZCE.FG209")
print(quote.datetime, quote.last_price, quote.ask_price1, quote.ask_price2)

while True:
    # 调用 wait_update 等待业务信息发生变化，例如: 行情发生变化, 委托单状态变化, 发生成交等等
    # 注意：其他合约的行情的更新也会触发业务信息变化，因此下面使用 is_changing 判断 FG209 的行情是否有变化
    api.wait_update()
    # 如果 FG209 的任何字段有变化，is_changing就会返回 True
    if api.is_changing(quote):
        print("行情变化", quote)
    # 只有当 FG209 的最新价有变化，is_changing才会返回 True
    if api.is_changing(quote, "last_price"):
        print("最新价变化", quote.last_price)
    # 当 FG209 的买1价/买1量/卖1价/卖1量中任何一个有变化，is_changing都会返回 True
    if api.is_changing(quote, ["ask_price1", "ask_volume1", "bid_price1", "bid_volume1"]):
        print("盘口变化", quote.ask_price1, quote.ask_volume1, quote.bid_price1, quote.bid_volume1)
```

Example 3 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'chengzhi'

from tqsdk import TqApi, TqAuth
import datetime

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
# 获得 i2209 tick序列的引用
ticks = api.get_tick_serial("DCE.i2209")
# 获得 i2209 10秒K线的引用
klines = api.get_kline_serial("DCE.i2209", 10)
print(datetime.datetime.fromtimestamp(klines.iloc[-1]["datetime"] / 1e9))

while True:
    api.wait_update()
    # 判断整个tick序列是否有变化
    if api.is_changing(ticks):
        # ticks.iloc[-1]返回序列中最后一个tick
        print("tick变化", ticks.iloc[-1])
    # 判断最后一根K线的时间是否有变化，如果发生变化则表示新产生了一根K线
    if api.is_changing(klines.iloc[-1], "datetime"):
        # datetime: 自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数
        print("新K线", datetime.datetime.fromtimestamp(klines.iloc[-1]["datetime"] / 1e9))
    # 判断最后一根K线的收盘价是否有变化
    if api.is_changing(klines.iloc[-1], "close"):
        # klines.close返回收盘价序列
        print("K线变化", datetime.datetime.fromtimestamp(klines.iloc[-1]["datetime"] / 1e9), klines.close.iloc[-1])
```

Example 4 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'chengzhi'

from tqsdk import TqApi, TqAuth

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
# 获得 m2105 的持仓引用，当持仓有变化时 position 中的字段会对应更新
position = api.get_position("DCE.m2105")
# 获得资金账户引用，当账户有变化时 account 中的字段会对应更新
account = api.get_account()
# 下单并返回委托单的引用，当该委托单有变化时 order 中的字段会对应更新
order = api.insert_order(symbol="DCE.m2105", direction="BUY", offset="OPEN", volume=5, limit_price=2750)

while True:
    api.wait_update()
    if api.is_changing(order, ["status", "volume_orign", "volume_left"]):
        print("单状态: %s, 已成交: %d 手" % (order.status, order.volume_orign - order.volume_left))
    if api.is_changing(position, "pos_long_today"):
        print("今多头: %d 手" % (position.pos_long_today))
    if api.is_changing(account, "available"):
        print("可用资金: %.2f" % (account.available))
```

---

