# Api Ref - Part 4

**Topics:** tqsdk.objs - 业务对象, tqsdk.TqRohon - 融航资管交易类, tqsdk.TqJees - 杰宜斯资管交易类

---

## tqsdk.objs - 业务对象

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.objs.html

**Contents:**
- tqsdk.objs - 业务对象

行情从交易所发出的时间(北京时间), 格式为 "2017-07-26 23:04:21.000001"

到期具体日，以秒为单位的 timestamp 值

期货交割日年份，只对期货品种有效。期权推荐使用最后行权日年份

期货交割日月份，只对期货品种有效。期权推荐使用最后行权日月份

期权最后行权日，以秒为单位的 timestamp 值

ETF实时单位基金净值，由于上游交易所协议变动，预计在2026年4月以后不再提供此字段

除权表 ["20190601,0.15","20200107,0.2"…]

除息表 ["20190601,0.15","20200107,0.2"…]

距离到期日的剩余天数（自然日天数），正数表示距离到期日的剩余天数，0表示到期日当天，负数表示距离到期日已经过去的天数

标的合约 underlying_symbol 所指定的合约对象，若没有标的合约则为 None

CategoryInfo 是一个板块信息对象

TradingTime 是一个交易时间对象 它不是一个可单独使用的类，而是用于定义 Quote 的 trading_time 字段的类型

(每个连续的交易时间段是一个列表，包含两个字符串元素，分别为这个时间段的起止点)

夜盘（注意：本字段中过了 24：00 的时间则在其基础往上加，如凌晨1点为 '25:00:00' ）

TradingStatus 是一个交易状态对象

交易状态, AUCTIONORDERING: 集合竞价报单; CONTINOUS: 连续交易; NOTRADING: 非交易时段

K线起点时间(按北京时间)，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数

tick从交易所发出的时间(按北京时间)，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数

静态权益 （静态权益 = 昨日结算的权益 + 今日入金 - 今日出金, 以服务器查询ctp后返回的金额为准）(不包含期权)

账户权益 （账户权益 = 动态权益 = 静态权益 + 平仓盈亏 + 持仓盈亏 - 手续费 + 权利金 + 期权市值）

可用资金（可用资金 = 账户权益 - 冻结保证金 - 保证金 - 冻结权利金 - 冻结手续费 - 期权市值）

期货公司返回的balance（ctp_balance = 静态权益 + 平仓盈亏 + 持仓盈亏 - 手续费 + 权利金）

期货公司返回的available（ctp_available = ctp_balance - 保证金 - 冻结保证金 - 冻结手续费 - 冻结权利金）

风险度（风险度 = 保证金 / 账户权益）

期货公司查询的多头今仓手数 (不推荐, 推荐使用pos_long_today)

期货公司查询的多头老仓手数 (不推荐, 推荐使用pos_long_his)

期货公司查询的多头手数 (不推荐, 推荐使用pos_long)

期货公司查询的空头今仓手数 (不推荐, 推荐使用pos_short_today)

期货公司查询的空头老仓手数 (不推荐, 推荐使用pos_short_his)

期货公司查询的空头手数 (不推荐, 推荐使用pos_short)

多头持仓成本,为今仓的开仓价乘以手数加上昨仓的昨结算价乘以手数的和

空头持仓成本,为今仓的开仓价乘以手数加上昨仓的昨结算价乘以手数的和

浮动盈亏 （浮动盈亏: 相对于开仓价的盈亏）

持仓盈亏 （持仓盈亏: 相对于上一交易日结算价的盈亏），期权持仓盈亏为 0

净持仓手数, ==0表示无持仓或多空持仓手数相等. <0表示空头持仓大于多头持仓, >0表示多头持仓大于空头持仓

多头持仓手数, ==0表示无多头持仓. >0表示多头持仓手数

空头持仓手数, ==0表示无空头持仓. >0表示空头持仓手数

与此持仓相关的且目前委托单状态为ALIVE的开仓/平仓挂单

dict, 其中每个元素的key为委托单ID, value为 Order

委托单ID, 对于一个用户的所有委托单，这个ID都是不重复的

开平标志, OPEN=开仓, CLOSE=平仓, CLOSETODAY=平今

委托价格, 仅当 price_type = LIMIT 时有效

价格类型, ANY=市价, LIMIT=限价

手数条件, ANY=任何数量, MIN=最小数量, ALL=全部数量

时间条件, IOC=立即完成，否则撤销, GFS=本节有效, GFD=当日有效, GTC=撤销前有效, GFA=集合竞价有效

下单时间，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数.

委托单状态, ALIVE=有效, FINISHED=已完

委托单是否确定已死亡（以后一定不会再产生成交）(注意，False 不代表委托单还存活，有可能交易所回来的信息还在路上或者丢掉了)

委托单是否确定已报入交易所并等待成交 (注意，返回 False 不代表确定未报入交易所，有可能交易所回来的信息还在路上或者丢掉了)

委托单是否确定是错单（即下单失败，一定不会有成交）(注意，返回 False 不代表确定不是错单，有可能交易所回来的信息还在路上或者丢掉了)

dict, 其中每个元素的key为成交ID, value为 Trade

委托单ID, 对于一个用户的所有委托单，这个ID都是不重复的

成交ID, 对于一个用户的所有成交，这个ID都是不重复的

开平标志, OPEN=开仓, CLOSE=平仓, CLOSETODAY=平今

成交时间，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数

SecurityAccount 是一个股票账户对象

币种, CNY=人民币, USD=美元, HKD=港币

当前资产 = 当前市值 + 可用金额 + 委托冻结金额 + 委托冻结费用

当前可用余额 = 期初可用 + 当日入金 - 当日出金 + 当日分红金额 - 当日买入金额 - 当日买入费用 + 当日卖出金额 - 当日卖出费用 - 委托冻结金额 - 委托冻结手续费

当前可取余额 = MAX(期初余额 + 当日入金 – 当日出金 + MIN(0，当日卖出释放资金 - 当日买入占用资金 - 委托冻结金额), 0)

当前交易冻结金额（不含费用）= sum(order.volume_orign * order.limit_price)

当前交易冻结费用 = sum(order.frozen_fee)

当日持仓盈亏 = 当前市值 - 当前买入成本

当日浮动盈亏 = SUM(持仓当日浮动盈亏)

当日实现盈亏 = SUM(持仓当日实现盈亏)

当日盈亏 = 当日浮动盈亏 + 当日实现盈亏

当日盈亏比 = 当日盈亏 / (当前买入成本 if 当前买入成本 > 0 else 期初资产)

SecurityPosition 是一个股票账户持仓对象

当前成本 = 昨买入成本 + 今买金额 + 今买费用 - 今卖数量 × (昨买入成本 / 昨持仓数量)

今持仓数量 = 昨持仓数量 + 今买数量 - 今卖数量 + 送股数量

当日浮动盈亏 = (昨持仓数量 - 今卖数量) * (最新价 - 昨收盘价) + (今持仓数量 - (昨持仓数量 - 今卖数量)) * (最新价 - 买入均价)

当日实现盈亏 = 今卖数量 * (最新价 - 昨收盘价) - 今卖费用 + 今派息金额

当日盈亏 = 当日浮动盈亏 + 当日实现盈亏

当日持仓盈亏 = 当前持仓市值 – 当前买入成本

累计收益率 = 总盈亏 / (期初成本 if 当前成本==0 else 当前成本)

SecurityOrder 是一个股票账户委托单对象

订单号,要求客户端保证其在一个交易日内的唯一性

委托股数（A 股委托数量必须为 100 倍数；科创板股票必须为 200 倍数；零股卖出: 由于部分成交或分红导致持仓数量小于 100 股时，该部分持仓可一次性卖出，数量不为 100 或 200 倍数）

报单价格类型，LIMIT=限价单，ANY=市价单

委托时间，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数.

dict, 其中每个元素的key为成交ID, value为 Trade

SecurityTrade 是一个股票账户成交对象

下单方向, BUY=买, SELL=卖，SHARED=送股，DEVIDEND=分红

成交时间，自unix epoch(1970-01-01 00:00:00 GMT)以来的纳秒数.

© 版权所有 2018-2026, TianQin。

---


## tqsdk.TqRohon - 融航资管交易类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tqrohon.html

**Contents:**
- tqsdk.TqRohon - 融航资管交易类

front_broker (str): 融航柜台代码

front_url (str): 融航柜台地址，格式为 tcp://ip:port，如 tcp://129.211.138.170:10001

app_id (str): 融航 AppID

auth_code (str): 融航 AuthCode

使用 TqRohon 账户需要安装 tqsdk_zq_otg 包： pip install -U tqsdk_zq_otg

front_broker, front_url, app_id 和 auth_code 信息需要融航申请程序化外接后取得

Account: 返回一个账户对象引用. 其内容将在 wait_update() 时更新

order_id (str): [可选]单号, 不填单号则返回所有委托单

Order: 当指定了 order_id 时, 返回一个委托单对象引用。 其内容将在 wait_update() 时更新。

不填 order_id 参数调用本函数, 将返回包含用户所有委托单的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的key为委托单号, value为 Order

注意: 在刚下单后, tqsdk 还没有收到回单信息时, 此对象中各项内容为空

symbol (str): [可选]合约代码, 不填则返回所有持仓

Position: 当指定了 symbol 时, 返回一个持仓对象引用。 其内容将在 wait_update() 时更新。

不填 symbol 参数调用本函数, 将返回包含用户所有持仓的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的 key 为合约代码, value 为 Position。

注意: 为保留一些可供用户查询的历史信息, 如 volume_long_yd(本交易日开盘前的多头持仓手数) 等字段, 因此服务器会返回当天已平仓合约( pos_long 和 pos_short 等字段为0)的持仓信息

trade_id (str): [可选]成交号, 不填成交号则返回所有委托单

Trade: 当指定了trade_id时, 返回一个成交对象引用. 其内容将在 wait_update() 时更新.

不填trade_id参数调用本函数, 将返回包含用户当前交易日所有成交记录的一个tqsdk.objs.Entity对象引用, 使用方法与dict一致, 其中每个元素的key为成交号, value为 Trade

推荐优先使用 trade_records() 获取某个委托单的相应成交记录, 仅当确有需要时才使用本函数.

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqRohon
account = TqRohon(account_id="融航账户", password="融航密码", front_broker="融航柜台代码", front_url="融航柜台地址", app_id="融航 AppID", auth_code="融航 AuthCode")
api = TqApi(account, auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
# 获取当前浮动盈亏
from tqsdk import TqApi, TqAuth, TqAccount

tqacc = TqAccount("N南华期货", "123456", "123456")
api = TqApi(account=tqacc, auth=TqAuth("快期账户", "账户密码"))
account = tqacc.get_account()
print(account.float_profit)

# 预计的输出是这样的:
2180.0
...
```

Example 3 (python):
```python
# 多账户模式下, 分别获取各账户浮动盈亏
from tqsdk import TqApi, TqAuth, TqMultiAccount, TqAccount, TqKq, TqSim

account = TqAccount("N南华期货", "123456", "123456")
tqkq = TqKq()
tqsim = TqSim()
api = TqApi(TqMultiAccount([account, tqkq, tqsim]), auth=TqAuth("快期账户", "账户密码"))
account1 = account.get_account()
account2 = tqkq.get_account()
account3 = tqsim.get_account()
print(f"账户 1 浮动盈亏 {account1.float_profit}, 账户 2 浮动盈亏 {account2.float_profit}, 账户 3 浮动盈亏 {account3.float_profit}")
api.close()
```

Example 4 (python):
```python
# 获取当前总挂单手数
from tqsdk import TqApi, TqAuth, TqAccount

tqacc = TqAccount("N南华期货", "123456", "123456")
api = TqApi(account=tqacc, auth=TqAuth("快期账户", "账户密码"))
orders = tqacc.get_order()
while True:
    api.wait_update()
    print(sum(order.volume_left for oid, order in orders.items() if order.status == "ALIVE"))

# 预计的输出是这样的:
3
3
0
...
```

---


## tqsdk.TqJees - 杰宜斯资管交易类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tqjees.html

**Contents:**
- tqsdk.TqJees - 杰宜斯资管交易类

front_broker (str): 杰宜斯柜台代码

front_url (str): 杰宜斯柜台地址，格式为 tcp://ip:port，如 tcp://129.211.138.170:10001

app_id (str): 杰宜斯 AppID

auth_code (str): 杰宜斯 AuthCode

使用 TqJees 账户需要安装 tqsdk_zq_otg 包： pip install -U tqsdk_zq_otg

front_broker, front_url, app_id 和 auth_code 信息需要向杰宜斯申请程序化外接后取得

Account: 返回一个账户对象引用. 其内容将在 wait_update() 时更新

order_id (str): [可选]单号, 不填单号则返回所有委托单

Order: 当指定了 order_id 时, 返回一个委托单对象引用。 其内容将在 wait_update() 时更新。

不填 order_id 参数调用本函数, 将返回包含用户所有委托单的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的key为委托单号, value为 Order

注意: 在刚下单后, tqsdk 还没有收到回单信息时, 此对象中各项内容为空

symbol (str): [可选]合约代码, 不填则返回所有持仓

Position: 当指定了 symbol 时, 返回一个持仓对象引用。 其内容将在 wait_update() 时更新。

不填 symbol 参数调用本函数, 将返回包含用户所有持仓的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的 key 为合约代码, value 为 Position。

注意: 为保留一些可供用户查询的历史信息, 如 volume_long_yd(本交易日开盘前的多头持仓手数) 等字段, 因此服务器会返回当天已平仓合约( pos_long 和 pos_short 等字段为0)的持仓信息

trade_id (str): [可选]成交号, 不填成交号则返回所有委托单

Trade: 当指定了trade_id时, 返回一个成交对象引用. 其内容将在 wait_update() 时更新.

不填trade_id参数调用本函数, 将返回包含用户当前交易日所有成交记录的一个tqsdk.objs.Entity对象引用, 使用方法与dict一致, 其中每个元素的key为成交号, value为 Trade

推荐优先使用 trade_records() 获取某个委托单的相应成交记录, 仅当确有需要时才使用本函数.

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqJees
account = TqJees(account_id="杰宜斯账户", password="杰宜斯密码", front_broker="杰宜斯柜台代码", front_url="杰宜斯柜台地址", app_id="杰宜斯 AppID", auth_code="杰宜斯 AuthCode")
api = TqApi(account, auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
# 获取当前浮动盈亏
from tqsdk import TqApi, TqAuth, TqAccount

tqacc = TqAccount("N南华期货", "123456", "123456")
api = TqApi(account=tqacc, auth=TqAuth("快期账户", "账户密码"))
account = tqacc.get_account()
print(account.float_profit)

# 预计的输出是这样的:
2180.0
...
```

Example 3 (python):
```python
# 多账户模式下, 分别获取各账户浮动盈亏
from tqsdk import TqApi, TqAuth, TqMultiAccount, TqAccount, TqKq, TqSim

account = TqAccount("N南华期货", "123456", "123456")
tqkq = TqKq()
tqsim = TqSim()
api = TqApi(TqMultiAccount([account, tqkq, tqsim]), auth=TqAuth("快期账户", "账户密码"))
account1 = account.get_account()
account2 = tqkq.get_account()
account3 = tqsim.get_account()
print(f"账户 1 浮动盈亏 {account1.float_profit}, 账户 2 浮动盈亏 {account2.float_profit}, 账户 3 浮动盈亏 {account3.float_profit}")
api.close()
```

Example 4 (python):
```python
# 获取当前总挂单手数
from tqsdk import TqApi, TqAuth, TqAccount

tqacc = TqAccount("N南华期货", "123456", "123456")
api = TqApi(account=tqacc, auth=TqAuth("快期账户", "账户密码"))
orders = tqacc.get_order()
while True:
    api.wait_update()
    print(sum(order.volume_left for oid, order in orders.items() if order.status == "ALIVE"))

# 预计的输出是这样的:
3
3
0
...
```

---

