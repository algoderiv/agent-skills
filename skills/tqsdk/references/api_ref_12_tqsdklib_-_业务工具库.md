# Api Ref - Part 12

**Topics:** tqsdk.lib - 业务工具库, tqsdk.tools.DataDownloader - 数据下载工具, 高级委托指令

---

## tqsdk.lib - 业务工具库

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.lib.html

**Contents:**
- tqsdk.lib - 业务工具库
- tqsdk.TqNotify - 收集通知信息工具
- tqsdk.TargetPosTask - 目标持仓工具
- tqsdk.TargetPosScheduler - 基于时间维度的目标持仓工具

api (tqsdk.api.TqApi): TqApi 实例

目标持仓 task, 该 task 可以将指定合约调整到目标头寸

创建目标持仓task实例，负责调整归属于该task的持仓 (默认为整个账户的该合约净持仓).

TargetPosTask 在 set_target_volume 时并不下单或撤单, 它的下单和撤单动作, 是在之后的每次 wait_update 时执行的. 因此, 需保证 set_target_volume 后还会继续调用wait_update() 。

TargetPosTask 在每次下单后会根据后续的行情计算下单的最新的价格，当该价格发生变化则首先撤回由该程序发出的在场的委托，然后根据最新的价格重新下单。

请勿在使用 TargetPosTask 的同时使用 insert_order() 函数, 否则将导致 TargetPosTask 报错或错误下单。

TargetPosTask 如果同时设置 min_volume（每笔最小下单手数），max_volume（每笔最大下单的手数）两个参数，表示采用 大单拆分模式 下单。

在 大单拆分模式 下，每次下单的手数为随机生成的正整数，值介于 min_volume、max_volume 之间。

具体说明：调用 set_target_volume 后，首先会根据目标持仓手数、开平仓顺序计算出，需要平今、平昨、开仓的目标下单手数及顺序。

如果在调整持仓的目标下单手数小于 max_volume，则直接以目标下单手数下单。

如果在调整持仓的目标下单手数大于等于 max_volume，则会以 min_volume、max_volume 之间的随机手数下一笔委托单，手数全部成交后，会接着处理剩余的手数； 继续以随机手数下一笔委托单，全部成交后，继续处理剩余的手数，直至剩余手数小于 max_volume 时，直接以剩余手数下单。

当使用大单拆分模式下单时，必须同时填写 min_volume、max_volume，且需要满足 max_volume >= min_volume > 0。

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 负责调整的合约代码

"ACTIVE"：对价下单，在持仓调整过程中，若下单方向为买，对价为卖一价；若下单方向为卖，对价为买一价。

"PASSIVE"：排队价下单，在持仓调整过程中，若下单方向为买，对价为买一价；若下单方向为卖，对价为卖一价，该种方式可能会造成较多撤单.

Callable[[str], Union[float, int]]: 函数参数为下单方向，函数返回值是下单价格。如果返回 nan，程序会抛错。

offset_priority (str): [可选]开平仓顺序，昨=平昨仓，今=平今仓，开=开仓，逗号=等待之前操作完成

对于下单指令区分平今/昨的交易所(如上期所)，按照今/昨仓的数量计算是否能平今/昨仓 对于下单指令不区分平今/昨的交易所(如中金所)，按照“先平当日新开仓，再平历史仓”的规则计算是否能平今/昨仓，如果这些交易所设置为"昨开"在有当日新开仓和历史仓仓的情况下，会自动跳过平昨仓进入到下一步

"今昨,开" 表示先平今仓，再平昨仓，等待平仓完成后开仓，对于没有单向大边的品种避免了开仓保证金不足

"今昨开" 表示先平今仓，再平昨仓，并开仓，所有指令同时发出，适合有单向大边的品种

"昨开" 表示先平昨仓，再开仓，禁止平今仓，适合股指这样平今手续费较高的品种

"开" 表示只开仓，不平仓，适合需要进行锁仓操作的品种

min_volume (int): [可选] 大单拆分模式下 每笔最小下单的手数，默认不启用 大单拆分模式

max_volume (int): [可选] 大单拆分模式下 每笔最大下单的手数，默认不启用 大单拆分模式

trade_chan (TqChan): [可选]成交通知channel, 当有成交发生时会将成交手数(多头为正数，空头为负数)发到该channel上

trade_objs_chan (TqChan): [可选]成交对象通知channel, 当有成交发生时会将成交对象发送到该channel上

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

当 price 参数为函数类型时，该函数应该返回一个有效的价格值，应该避免返回 nan。以下为 price 参数是函数类型时的示例。

volume (int): 目标持仓手数，正数表示多头，负数表示空头，0表示空仓

取消当前 TargetPosTask 实例，会将该实例已经发出但还是未成交的委托单撤单，并且如果后续调用此实例的 set_target_volume 函数会报错。

任何时刻，每个账户下一个合约只能有一个 TargetPosTask 实例，并且其构造参数不能修改。

如果对于同一个合约要构造不同参数的 TargetPosTask 实例，需要调用 cancel 方法销毁，才能创建新的 TargetPosTask 实例

返回当前 TargetPosTask 实例是否已经结束。即如果后续调用此实例的 set_target_volume 函数会报错，此实例不会再下单或者撤单。

bool: 当前 TargetPosTask 实例是否已经结束

追价下单task, 该task会在行情变化后自动撤单重下，直到全部成交 （注：此类主要在tqsdk内部使用，并非简单用法，不建议用户使用）

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 拟下单的合约symbol, 格式为 交易所代码.合约代码, 例如 "SHFE.cu1801"

direction (str): "BUY" 或 "SELL"

offset (str): "OPEN", "CLOSE" 或 "CLOSETODAY"

volume (int): 需要下单的手数

min_volume (int): [可选] 大单拆分模式下 每笔最小下单的手数，默认不启用 大单拆分模式

max_volume (int): [可选] 大单拆分模式下 每笔最大下单的手数，默认不启用 大单拆分模式

"ACTIVE"：对价下单，在持仓调整过程中，若下单方向为买，对价为卖一价；若下单方向为卖，对价为买一价。

"PASSIVE"：对价下单，在持仓调整过程中，若下单方向为买，对价为买一价；若下单方向为卖，对价为卖一价。

Callable[[str], Union[float, int]]: 函数参数为下单方向，函数返回值是下单价格。如果返回 nan，程序会抛错。

trade_chan (TqChan): [可选]成交通知channel, 当有成交发生时会将成交手数(多头为正数，空头为负数)发到该channel上

trade_objs_chan (TqChan): [可选]成交对象通知channel, 当有成交发生时会将成交对象发送到该channel上

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

下单task （注：此类主要在tqsdk内部使用，并非简单用法，不建议用户使用）

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 拟下单的合约symbol, 格式为 交易所代码.合约代码, 例如 "SHFE.cu1801"

direction (str): "BUY" 或 "SELL"

offset (str): "OPEN", "CLOSE" 或 "CLOSETODAY"

volume (int): 需要下单的手数

limit_price (float): [可选]下单价格, 默认市价单

order_chan (TqChan): [可选]委托单通知channel, 当委托单状态发生时会将委托单信息发到该channel上

trade_chan (TqChan): [可选]成交通知channel, 当有成交发生时会将成交手数(多头为正数，空头为负数)发到该channel上

trade_objs_chan (TqChan): [可选]成交对象通知channel, 当有成交发生时会将成交对象发送到该channel上

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

算法执行引擎，根据设定的目标持仓任务列表，调整指定合约到目标头寸

创建算法执行引擎实例，根据设定的目标持仓任务列表，调用 TargetPosTask 来调整指定合约到目标头寸。

TargetPosScheduler 创建后不会立即不下单或撤单, 它的下单和撤单动作, 是在之后的每次 wait_update 时执行的. 因此, 需保证后续还会调用wait_update() 。

请勿同时使用 TargetPosScheduler、TargetPosTask、insert_order() 函数, 否则将导致报错或错误下单。

symbol，offset_priority，min_volume，max_volume，trade_chan，trade_objs_chan，account 这几个参数会直接传给 TargetPosTask，请按照 TargetPosTask 的说明设置参数。

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 负责调整的合约代码

注意1：对于最后一项任务，会按照当前项参数，调整到目标持仓后立即退出（时间参数不对最后一项任务起作用）

注意2：时间长度可以跨非交易时间段（可以跨小节等待），但是不可以跨交易日

target_pos: 当前这项任务的目标净持仓手数

Callable (direction: str) -> Union[float, int]: 传入函数作为价格参数，函数参数为下单方向，函数返回值是下单价格。如果返回 nan，程序会抛错。

offset_priority (str): [可选]开平仓顺序，昨=平昨仓，今=平今仓，开=开仓，逗号=等待之前操作完成

对于下单指令区分平今/昨的交易所(如上期所)，按照今/昨仓的数量计算是否能平今/昨仓 对于下单指令不区分平今/昨的交易所(如中金所)，按照“先平当日新开仓，再平历史仓”的规则计算是否能平今/昨仓，如果这些交易所设置为"昨开"在有当日新开仓和历史仓仓的情况下，会自动跳过平昨仓进入到下一步

"今昨,开" 表示先平今仓，再平昨仓，等待平仓完成后开仓，对于没有单向大边的品种避免了开仓保证金不足

"今昨开" 表示先平今仓，再平昨仓，并开仓，所有指令同时发出，适合有单向大边的品种

"昨开" 表示先平昨仓，再开仓，禁止平今仓，适合股指这样平今手续费较高的品种

"开" 表示只开仓，不平仓，适合需要进行锁仓操作的品种

min_volume (int): [可选] 大单拆分模式下 每笔最小下单的手数，默认不启用 大单拆分模式

max_volume (int): [可选] 大单拆分模式下 每笔最大下单的手数，默认不启用 大单拆分模式

trade_chan (TqChan): [可选]成交通知channel, 当有成交发生时会将成交手数(多头为正数，空头为负数)发到该channel上

trade_objs_chan (TqChan): [可选]成交对象通知channel, 当有成交发生时会将成交对象发送到该channel上

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

取消当前 TargetPosScheduler 实例，会将该实例已经发出但还是未成交的委托单撤单。

返回当前 TargetPosScheduler 实例是否已经结束。即此实例不会再发出下单或者撤单的任何动作。

bool: 当前 TargetPosScheduler 实例是否已经结束

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth, TqKq, TqNotify

api = TqApi(account=TqKq(), auth=TqAuth("快期账户", "账户密码"))
tqNotify = TqNotify(api)  # 构造实例类
while True:
    api.wait_update()
    # 每次调用返回距离上一次调用 tqNotify.get_notifies() 之后产生的通知列表，没有的话返回 []
    notify_list = tqNotify.get_notifies()
    for notify in notify_list:
        print(notify)  # 打印出通知内容
        # send_message(notify['content'])  可以发送通知到其他工具
```

Example 2 (python):
```python
# ... 用户代码 ...
quote = api.get_quote("SHFE.cu2012")
def get_price(direction):
    # 在 BUY 时使用买一价加一档价格，SELL 时使用卖一价减一档价格
    if direction == "BUY":
        price = quote.bid_price1 + quote.price_tick
    else:
        price = quote.ask_price1 - quote.price_tick
    # 如果 price 价格是 nan，使用最新价报单
    if price != price:
        price = quote.last_price
    return price

target_pos = TargetPosTask(api, "SHFE.cu2012", price=get_price)
# ... 用户代码 ...
```

Example 3 (python):
```python
# ... 用户代码 ...
quote_list = api.get_quote_list(["SHFE.cu2012","SHFE.au2012"])

def get_price_by_quote(quote):
    def get_price(direction):
        # 在 BUY 时使用涨停价，SELL 时使用跌停价
        if direction == "BUY":
            price = quote["upper_limit"]
        else:
            price = quote.lower_limit
        # 如果 price 价格是 nan，使用最新价报单
        if price != price:
            price = quote.last_price
        return price
    return get_price

for quote in quote_list:
    target_pos_active_dict[quote.instrument_id] = TargetPosTask(api, quote.instrument_id, price=get_price_by_quote(quote))

# ... 用户代码 ...
```

Example 4 (python):
```python
# 大单拆分模式用法示例

from tqsdk import TqApi, TqAuth, TargetPosTask
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
position = api.get_position('SHFE.rb2106')

# 同时设置 min_volume、max_volume 两个参数，表示使用大单拆分模式
t = TargetPosTask(api, 'SHFE.rb2106', min_volume=2, max_volume=10)
t.set_target_volume(50)
while True:
    api.wait_update()
    if position.pos_long == 50:
        break
api.close()

# 说明：
# 以上代码使用 TqSim 交易，开始时用户没有 SHFE.cu2012 合约的任何持仓，那么在 t.set_target_volume(50) 之后应该开多仓 50 手
# 根据用户参数，下单使用大单拆分模式，每次下单手数在 2～10 之间，打印出的成交通知可能是这样的：
# 2021-03-15 11:29:48 -     INFO - 模拟交易成交记录
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.516138, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 7, 价格: 4687.000,手续费: 32.94
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.519699, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 8, 价格: 4687.000,手续费: 37.64
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.522848, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 10, 价格: 4687.000,手续费: 47.05
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.525617, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 8, 价格: 4687.000,手续费: 37.64
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.528151, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 7, 价格: 4687.000,手续费: 32.94
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.530930, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 7, 价格: 4687.000,手续费: 32.94
# 2021-03-15 11:29:48 -     INFO - 时间: 2021-03-15 11:29:47.533515, 合约: SHFE.rb2106, 开平: OPEN, 方向: BUY, 手数: 3, 价格: 4687.000,手续费: 14.12
```

---


## tqsdk.tools.DataDownloader - 数据下载工具

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tools.download.html

**Contents:**
- tqsdk.tools.DataDownloader - 数据下载工具

数据下载工具是 TqSdk 专业版中的功能，能让用户下载目前 TqSdk 提供的全部期货、期权和股票类的历史数据，下载数据支持 tick 级别精度和任意 kline 周期

如果想使用数据下载工具，可以点击 天勤量化专业版 申请使用或购买

api (TqApi): TqApi实例，该下载器将使用指定的api下载数据

symbol_list (str/list of str): 需要下载数据的合约代码，当指定多个合约代码时将其他合约按第一个合约的交易时间对齐

dur_sec (int): 数据周期，以秒为单位。例如: 1分钟线为60,1小时线为3600,日线为86400,Tick数据为0

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

datetime: 指的是具体时间点，如果没有指定时区信息，则默认为北京时间

StreamWriter: 直接将内容输出到 StreamWriter

write_mode (str): 写入模式，默认值为 "w"。"w" 表示覆盖写入，会写入标题行，再写入数据；"a" 表示追加写入，不写标题行，直接写入数据。

adj_type (str/None): 复权计算方式，默认值为 None。"F" 为前复权；"B" 为后复权；None 表示不复权。只对股票、基金合约有效。

bool: 如果数据下载完成则返回 True, 否则返回 False.

float: 下载进度,100表示下载完成

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from datetime import datetime, date
from contextlib import closing
from tqsdk import TqApi, TqAuth, TqSim
from tqsdk.tools import DataDownloader

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
download_tasks = {}
# 下载从 2018-01-01 到 2018-09-01 的 SR901 日线数据
download_tasks["SR_daily"] = DataDownloader(api, symbol_list="CZCE.SR901", dur_sec=24*60*60,
                    start_dt=date(2018, 1, 1), end_dt=date(2018, 9, 1), csv_file_name="SR901_daily.csv")
# 下载从 2017-01-01 到 2018-09-01 的 rb主连 5分钟线数据
download_tasks["rb_5min"] = DataDownloader(api, symbol_list="KQ.m@SHFE.rb", dur_sec=5*60,
                    start_dt=date(2017, 1, 1), end_dt=date(2018, 9, 1), csv_file_name="rb_5min.csv")
# 下载从 2018-01-01凌晨6点 到 2018-06-01下午4点 的 cu1805,cu1807,IC1803 分钟线数据，所有数据按 cu1805 的时间对齐
# 例如 cu1805 夜盘交易时段, IC1803 的各项数据为 N/A
# 例如 cu1805 13:00-13:30 不交易, 因此 IC1803 在 13:00-13:30 之间的K线数据会被跳过
download_tasks["cu_min"] = DataDownloader(api, symbol_list=["SHFE.cu1805", "SHFE.cu1807", "CFFEX.IC1803"], dur_sec=60,
                    start_dt=datetime(2018, 1, 1, 6, 0 ,0), end_dt=datetime(2018, 6, 1, 16, 0, 0), csv_file_name="cu_min.csv")
# 下载从 2018-05-01凌晨0点 到 2018-06-01凌晨0点 的 T1809 盘口Tick数据
download_tasks["T_tick"] = DataDownloader(api, symbol_list=["CFFEX.T1809"], dur_sec=0,
                    start_dt=datetime(2018, 5, 1), end_dt=datetime(2018, 6, 1), csv_file_name="T1809_tick.csv")
# 使用with closing机制确保下载完成后释放对应的资源
with closing(api):
    while not all([v.is_finished() for v in download_tasks.values()]):
        api.wait_update()
        print("progress: ", { k:("%.2f%%" % v.get_progress()) for k,v in download_tasks.items() })
```

---


## 高级委托指令

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/order.html

**Contents:**
- 高级委托指令

在实盘交易中, 除常见的限价委托指令外, tqsdk 提供了 FAK / FOK 两种高级市价指令。

insert_order 为用户提供了 limit_price，advanced 两个参数指定下单指令，两个参数支持的值的组合为：

limit_price 默认值为 None

对于市价单、BEST、FIVELEVEL，advanced="FAK" 与默认参数 None 的实际报单请求一样。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
# 当日有效限价单
api.insert_order("SHFE.cu2009", "BUY", "OPEN", 3, limit_price=14200)
# FAK 限价单
api.insert_order("SHFE.cu2009", "BUY", "OPEN", 3, limit_price=14200, advanced="FAK")
# FOK 限价单
api.insert_order("SHFE.cu2009", "BUY", "OPEN", 3, limit_price=14200, advanced="FOK")

# 市价单
api.insert_order("DCE.m2009", "BUY", "OPEN", 3)
# FOK 市价单
api.insert_order("DCE.m2009", "BUY", "OPEN", 3, advanced="FOK")

# BEST
api.insert_order("CFFEX.T2003", "BUY", "OPEN", 3, limit_price="BEST")
# FIVELEVEL
api.insert_order("CFFEX.T2003", "BUY", "OPEN", 3, limit_price="FIVELEVEL")
```

---

