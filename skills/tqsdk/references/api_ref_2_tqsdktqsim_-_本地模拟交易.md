# Api Ref - Part 2

**Topics:** tqsdk.TqSim - 本地模拟交易, tqsdk.TqMultiAccount - 多账户, 在外出环境下的紧急停止方案, TqSdk 企业版, TianQin Python Sdk User Guide

---

## tqsdk.TqSim - 本地模拟交易

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.sim.html

**Contents:**
- tqsdk.TqSim - 本地模拟交易
- tqsdk.TqSimStock - 本地股票模拟交易

该类实现了一个本地的模拟账户，并且在内部完成撮合交易，在回测模式下，只能使用 TqSim 账户来交易。

限价单要求报单价格达到或超过对手盘价格才能成交, 成交价为报单价格, 如果没有对手盘(涨跌停)则无法成交

市价单使用对手盘价格成交, 如果没有对手盘(涨跌停)则自动撤单

模拟交易不会有部分成交的情况, 要成交就是全部成交

init_balance (float): [可选]初始资金, 默认为一千万

account_id (str): [可选]帐号, 默认为 TQSIM

commission (float): 每手手续费

symbol (str): 合约代码 (只支持期货合约)

margin (float): 每手保证金

float: 返回合约模拟交易的每手保证金

float: 返回合约模拟交易的每手手续费

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

该类实现了一个本地的股票模拟交易账户，并且在内部完成撮合交易，在回测模式下，只能使用 TqSimStock 账户来交易股票合约。

股票模拟交易只支持 ins_class 字段为 'STOCK' 的合约，且不支持 T+0 交易。

限价单要求报单价格达到或超过对手盘价格才能成交, 成交价为报单价格, 如果没有对手盘(涨跌停)则无法成交

市价单使用对手盘价格成交, 如果没有对手盘(涨跌停)则自动撤单

模拟交易不会有部分成交的情况, 要成交就是全部成交

init_balance (float): [可选]初始资金, 默认为一千万

account_id (str): [可选]帐号, 默认为 TQSIM_STOCK

SecurityAccount: 返回一个账户对象引用. 其内容将在 wait_update() 时更新

order_id (str): [可选]单号, 不填单号则返回所有委托单

SecurityOrder: 当指定了 order_id 时, 返回一个委托单对象引用。 其内容将在 wait_update() 时更新。

不填 order_id 参数调用本函数, 将返回包含用户所有委托单的一个 tqsdk.objs.Entity 对象引用, 使用方法与 dict 一致, 其中每个元素的 key 为委托单号, value为 SecurityOrder

注意: 在刚下单后, tqsdk 还没有收到回单信息时, 此对象中各项内容为空

symbol (str): [可选]合约代码, 不填则返回所有持仓

SecurityPosition: 当指定了 symbol 时, 返回一个持仓对象引用。 其内容将在 wait_update() 时更新。

不填 symbol 参数调用本函数, 将返回包含用户所有持仓的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的 key 为合约代码, value 为 SecurityPosition。

trade_id (str): [可选]成交号, 不填成交号则返回所有委托单

SecurityTrade: 当指定了trade_id时, 返回一个成交对象引用. 其内容将在 wait_update() 时更新.

不填trade_id参数调用本函数, 将返回包含用户当前交易日所有成交记录的一个 tqsdk.objs.Entity 对象引用, 使用方法与dict一致, 其中每个元素的key为成交号, value为 SecurityTrade

推荐优先使用 trade_records() 获取某个委托单的相应成交记录, 仅当确有需要时才使用本函数.

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
# 修改TqSim模拟帐号的初始资金为100000
from tqsdk import TqApi, TqSim, TqAuth
api = TqApi(TqSim(init_balance=100000), auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
from tqsdk import TqSim, TqApi, TqAuth

sim = TqSim()
api = TqApi(sim, auth=TqAuth("快期账户", "账户密码"))

sim.set_commission("SHFE.cu2112", 50)

print(sim.get_commission("SHFE.cu2112"))
```

Example 3 (python):
```python
from tqsdk import TqSim, TqApi, TqAuth

sim = TqSim()
api = TqApi(sim, auth=TqAuth("快期账户", "账户密码"))

sim.set_margin("SHFE.cu2112", 26000)

print(sim.get_margin("SHFE.cu2112"))
```

Example 4 (python):
```python
from tqsdk import TqSim, TqApi, TqAuth

sim = TqSim()
api = TqApi(sim, auth=TqAuth("快期账户", "账户密码"))

quote = api.get_quote("SHFE.cu2112")
print(sim.get_margin("SHFE.cu2112"))
```

---


## tqsdk.TqMultiAccount - 多账户

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.multiaccount.html

**Contents:**
- tqsdk.TqMultiAccount - 多账户

天勤多账户 - TqMultiAccount

天勤多账户模块提供了单 api 同时操作不同账户及其组合的功能支持，目前已支持实盘账户、模拟账户和快期模拟账户的任意组合。 使用天勤多账户进行跨市场或跨账户交易时，可以在不引入多进程和多线程的前提下, 比较方便的传递账户信息进行策略编写, 同时, 也更方便对不同账户的交易数据进行统计分析。

多账户模式下, 对于 get_position，account，insert_order，set_target_volume 等函数必须指定 account 参数

多账户模式下, 实盘账户的数量受限于快期账户支持实盘账户数, 详见:更多的实盘交易账户数

accounts (List[Union[TqAccount, TqKq, TqKqStock, TqSim, TqSimStock, TqZq, TqCtp, TqRohon, TqJees, TqYida, TqTradingUnit]]): [可选] 多账户列表, 若未指定任何账户, 则为 [TqSim()]

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAccount, TqMultiAccount

account1 = TqAccount("H海通期货", "123456", "123456")
account2 = TqAccount("H宏源期货", "654321", "123456")
api = TqApi(TqMultiAccount([account1, account2]), auth=TqAuth("快期账户", "账户密码"))
# 分别获取账户资金信息
order1 = api.insert_order(symbol="DCE.m2101", direction="BUY", offset="OPEN", volume=3, account=account1)
order2 = api.insert_order(symbol="SHFE.au2012C308", direction="BUY", offset="OPEN", volume=3, limit_price=78.0, account=account2)
while order1.status != "FINISHED" or order2.status != "FINISHED":
     api.wait_update()
# 分别获取账户资金信息
account_info1 = account1.get_account()
account_info2 = account2.get_account()
api.close()
```

Example 2 (python):
```python
# 多账户模式下使用 TargetPosTask
from tqsdk import TqApi, TqAccount, TqMultiAccount, TqAuth, TargetPosTask

account1 = TqAccount("H海通期货", "123456", "123456")
account2 = TqAccount("H宏源期货", "654321", "123456")
api = TqApi(TqMultiAccount([account1, account2]), auth=TqAuth("快期账户", "账户密码"))
symbol1 = "DCE.m2105"
symbol2 = "DCE.i2101"
position1 = account1.get_position(symbol1)
position2 = account2.get_position(symbol2)
# 多账户模式下, 调仓工具需要指定账户实例
target_pos1 = TargetPosTask(api, symbol1, account=account1)
target_pos2 = TargetPosTask(api, symbol2, account=account2)
target_pos1.set_target_volume(30)
target_pos2.set_target_volume(80)
while position1.volume_long != 30 or position2.volume_long != 80:
    api.wait_update()

api.close()
```

---


## 在外出环境下的紧急停止方案

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/emergency_stop.html

**Contents:**
- 在外出环境下的紧急停止方案
- 适用场景
- 推荐做法: 使用远程遥控插座
- 重要提示

在实际交易中, 难免会遇到人在外面、无法第一时间回到电脑前, 但又需要 立刻阻止策略继续报单 的情况。 对于这类极端场景, 可以准备一套 从物理链路上阻断报单 的“最后一道防线”——使用远程遥控插座控制电脑或网络设备的供电。

如果可以正常远程登录到运行环境, 仍然 优先通过程序自身的退出/风控逻辑 来停止策略; 本文介绍的方案只用于“网络中断、远程工具失效、程序卡死/死循环”等, 无法正常操作时的 兜底紧急手段 。

下面这些场景都可以考虑使用远程遥控插座作为补充手段:

人不在机房/办公室, 无法直接操作运行策略的电脑

远程桌面/VPN 无法连接, 不能登录服务器手动停止程序

运行环境网络异常, 与远程服务器或交易终端的会话建立/恢复失败

程序出现死循环、界面无响应, 无法通过常规方式优雅退出

需要在数秒～数十秒内阻止程序继续发出新的报单

市面上有很多可以通过手机 App 控制的 远程遥控插座/智能插座 , 一般具备以下能力:

支持 4G/5G 或通过家庭/公司网络连接互联网

在手机上安装 App 后, 可以远程开/关插座电源

对于无人监控环境下运行的策略, 可以从物理链路上做如下配置:

将 运行策略的电脑/服务器电源 接在远程遥控插座上, 需要紧急停机时, 在手机 App 中关断插座电源, 电脑随之断电, 程序立即停止, 不会再继续报单

或者, 将 用于联网的路由器/交换机等网络设备电源 接在远程遥控插座上, 紧急情况下关闭插座, 网络被切断, 程序虽然暂时仍在运行, 但已无法向交易服务器发送新的报单

通过远程遥控插座断电, 对电脑来说相当于“强制断电”, 可能造成未保存数据丢失, 请只在 确属紧急情况 时使用。

该方案的主要目的在于 尽快阻止程序继续发送报单 , 而不是优雅关闭系统; 日常风险控制仍应依赖合理的仓位管理、风控规则和监控告警, 远程插座仅作为极端情况下的补充。

© 版权所有 2018-2026, TianQin。

---


## TqSdk 企业版

**URL:** https://doc.shinnytech.com/tqsdk/latest/enterprise.html

**Contents:**
- TqSdk 企业版
- TqSdk 本地多策略功能
- TqSdk 直连功能
- TqSdk 连接平台功能

除了 TqSdk 专业版以外，我们还提供 TqSdk 企业版本

企业版和专业版相比的主要区别是柜台支持上的区别，企业版支持直连 CTP/融航/杰宜斯等柜台，专业版只能通过中继的方式去进行连接

如果想使用 TqSdk 企业版功能，可以点击 个人中心 申请15天试用或购买

随着对收益曲线稳定的追求，较多用户需求在一个账户下去运行多个策略，当多个策略交易同一标的时，则面临着不同策略的持仓管理，绩效归因等问题

为了解决该问题， tqsdk 在企业版中提供了本地众期多策略系统，支持用户在本地将一个实盘账户拆分为多个策略（多个前端账户），每个策略交易数据相互隔离，且跨日有效

同时该方案会提供多策略的管理界面，支持可视化观察各个多策略的持仓，委托，资金和盈亏情况

TqSdk 本地多策略功能的详细介绍，请点击 TqSdk 多策略使用手册

在 TqSdk 企业版支持用户通过直连模式接入任意一家指定期货公司

除了接入指定期货公司的优点以外，直连模式还带来了一下好处:

交易指令直达期货公司，省去中继服务器路径，交易延迟平均减少10ms左右

TqSdk 直连CTP模式的详细介绍，请点击 TqCtp

TqSdk 提供了资管平台的对接支持，支持用户连接到指定资管平台，例如杰宜斯或者融航资管系统等

其中融航的 模拟账户 、 模拟账户密码 、 app_id 和 auth_code 需要自行和融航联系获取，其他参数在融航模拟下为

front_url="tcp://129.211.138.170:10001"

front_broker="RohonDemo"

融航资管平台连接模式的详细介绍，请点击 TqRohon

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqRohon, TqAuth

account = TqRohon(account_id="融航账户", password="融航密码", front_broker="融航柜台代码", front_url="融航柜台地址", app_id="融航 AppID", auth_code="融航 AuthCode")
api = TqApi(account, auth=TqAuth("快期账户", "账户密码"))
```

---


## TianQin Python Sdk User Guide

**URL:** https://doc.shinnytech.com/tqsdk/latest/index.html

**Contents:**
- TianQin Python Sdk User Guide

本文档是 TqSdk 的使用说明. 从 TqSdk 中的一些关键概念开始, 逐步介绍如何充分利用 TqSdk 全部功能。

© 版权所有 2018-2026, TianQin。

---


## tqsdk.TqAccount - 实盘账户类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.account.html

**Contents:**
- tqsdk.TqAccount - 实盘账户类

broker_id (str): 期货公司，支持的期货公司列表 https://www.shinnytech.com/blog/tq-support-broker/

td_url(str): [可选]用于指定账户连接的交易服务器地址, eg: "tcp://1.2.3.4:1234/"

sm(bool): [可选]是否通过国密连接到服务器

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

Example 2 (python):
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

Example 3 (python):
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

Example 4 (python):
```python
# 多账户模式下, 分别获取各账户挂单手数
from tqsdk import TqApi, TqAuth, TqMultiAccount, TqAccount, TqKq, TqSim

account = TqAccount("N南华期货", "123456", "123456")
tqkq = TqKq()
tqsim = TqSim()
api = TqApi(TqMultiAccount([account, tqkq, tqsim]), auth=TqAuth("快期账户", "账户密码"))
orders1 = account.get_order()
orders2 = tqkq.get_order()
orders3 = tqsim.get_order()
print(f"账户 1 挂单手数 {sum(order.volume_left for order in orders1.values() if order.status == "ALIVE")}, ",
      f"账户 2 挂单手数 {sum(order.volume_left for order in orders2.values() if order.status == "ALIVE")}, ",
      f"账户 3 挂单手数 {sum(order.volume_left for order in orders3.values() if order.status == "ALIVE")}")

order = account.get_order(order_id="订单号")
print(order)
api.close()
```

---

