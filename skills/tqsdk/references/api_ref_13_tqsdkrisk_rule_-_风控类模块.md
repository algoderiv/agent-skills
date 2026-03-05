# Api Ref - Part 13

**Topics:** tqsdk.risk_rule - 风控类模块, tqsdk.TqKq - 快期模拟交易类, 算法模块示例, 在 TqSdk 中调用 TqSdk2 查询保证金, 进阶主题

---

## tqsdk.risk_rule - 风控类模块

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.risk_rule.html

**Contents:**
- tqsdk.risk_rule - 风控类模块

api (TqApi): TqApi 实例

open_counts_limit (int): 交易日内开仓次数上限

account (TqAccount/TqKq/TqZq/TqKqStock/TqSim/TqSimStock/TqCtp/TqRohon/TqJees/TqYida/TqTradingUnit): [可选] 指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

api (TqApi): TqApi 实例

open_volumes_limit (int): 交易日内开仓手数上限

account (TqAccount/TqKq/TqZq/TqKqStock/TqSim/TqSimStock/TqCtp/TqRohon/TqJees/TqYida/TqTradingUnit): [可选] 指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

api (TqApi): TqApi 实例

open_volumes_limit (int): 交易日内开仓手数之和上限

account (TqAccount/TqKq/TqZq/TqKqStock/TqSim/TqSimStock/TqCtp/TqRohon/TqJees/TqYida/TqTradingUnit): [可选] 指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

风控规则类 - 一个 API 实例每秒最大订单操作次数限制。

针对特定的交易所和交易账户，设置每秒订单操作次数阈值（包括报单和撤单）。 当操作次数达到阈值时触发风控。如果指定多个交易所，则每个交易所分别限制。

api (TqApi): TqApi 实例

limit_per_second (int): 每秒订单操作次数上限（当操作次数超过此值时触发风控）

str: 指定交易所代码，如 "SHFE", "DCE", "CZCE", "CFFEX" 等

list of str: 交易所代码列表，如 ["DCE", "SHFE"]，每个交易所分别限制

account (TqAccount/TqKq/TqZq/TqKqStock/TqSim/TqSimStock/TqCtp/TqRohon/TqJees/TqYida/TqTradingUnit): [可选] 指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi
from tqsdk.risk_rule import TqRuleOpenCountsLimit

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

rule = TqRuleOpenCountsLimit(api, open_counts_limit=10, symbol="DCE.m2112")  # 创建风控规则实例
api.add_risk_rule(rule)  # 添加风控规则

quote = api.get_quote("DCE.m2112")
try:
    # 每次最新价变动，下一笔订单，直到超过开仓次数风控限制
    while True:
        api.wait_update()
        if api.is_changing(quote, ['last_price']):
            order = api.insert_order(symbol="DCE.m2112", direction="BUY", offset="OPEN", volume=1)
            while order.status != "FINISHED":
                api.wait_update()
except TqRiskRuleError as e:
    print('!!!', e)
api.close()
```

Example 2 (python):
```python
from tqsdk import TqApi
from tqsdk.risk_rule import TqRuleOpenVolumesLimit

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

rule = TqRuleOpenVolumesLimit(api, open_volumes_limit=10, symbol="DCE.m2112")  # 创建风控规则实例
api.add_risk_rule(rule)  # 添加风控规则

# 下单 5 手，不会触发风控规则
order1 = api.insert_order(symbol="DCE.m2112", direction="BUY", offset="OPEN", volume=5)
while order1.status != "FINISHED":
    api.wait_update()

# 继续下单 8 手，会触发风控规则
order2 = api.insert_order(symbol="DCE.m2112", direction="BUY", offset="OPEN", volume=8)
while order2.status != "FINISHED":
    api.wait_update()
api.close()
```

Example 3 (python):
```python
from tqsdk import TqApi, TqKq, TqRiskRuleError
from tqsdk.risk_rule import TqRuleOpenVolumesLimit

account = TqKq()
api = TqApi(account=account, auth=TqAuth("快期账户", "账户密码"))

rule = TqRuleOpenVolumesLimit(api, open_volumes_limit=10, symbol="DCE.m2112", account=account)  # 创建风控规则实例
api.add_risk_rule(rule)  # 添加风控规则

try:
    # 下单 11 手，触发风控规则
    order1 = api.insert_order(symbol="DCE.m2112", direction="BUY", offset="OPEN", volume=11)
    while order1.status != "FINISHED":
        api.wait_update()
except TqRiskRuleError as e:
    print("!!!", e)

api.close()
```

Example 4 (python):
```python
from tqsdk import TqApi, TqKq, TqRiskRuleError
from tqsdk.risk_rule import TqRuleAccOpenVolumesLimit

account = TqKq()
api = TqApi(account=account, auth=TqAuth("快期账户", "账户密码"))

quote = api.get_quote("SSE.000300")
call_in, call_at, call_out = api.query_all_level_finance_options("SSE.000300", quote.last_price, "CALL", nearbys=0)
put_in, put_at, put_out = api.query_all_level_finance_options("SSE.000300", quote.last_price, "PUT", nearbys=0)
near_symbols = call_in + call_at + call_out + put_in + put_at + put_out  # 找到所有当月期权合约

symbols = api.query_options("SSE.000300", expired=False)  # 找到所有中金所期权合约

# 规则1: 中金所当月期权合约日内开仓不超过 100 手
# 规则2: 中金所所有期权合约日内合约开仓不超过 200 手
rule1 = TqRuleAccOpenVolumesLimit(api, open_volumes_limit=100, symbol=near_symbols, account=account)  # 创建风控规则实例
rule2 = TqRuleAccOpenVolumesLimit(api, open_volumes_limit=200, symbol=symbols, account=account)  # 创建风控规则实例
api.add_risk_rule(rule1)  # 添加风控规则
api.add_risk_rule(rule2)  # 添加风控规则

try:
    # 下单 101 手，触发风控规则
    order1 = api.insert_order(symbol="CFFEX.IO2111-C-4900", direction="BUY", offset="OPEN", volume=101, limit_price=35.6)
    while order1.status != "FINISHED":
        api.wait_update()
except TqRiskRuleError as e:
    print("!!!", e)  # 报错，当月期权合约日内合约开仓不超过 100 手, 已下单次数 0

api.close()
```

---


## tqsdk.TqKq - 快期模拟交易类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tqkq.html

**Contents:**
- tqsdk.TqKq - 快期模拟交易类
- tqsdk.TqKqStock - 快期股票模拟交易类

快期模拟的账户和交易信息可以在快期专业版, 快期v2, 快期v3, 快期APP查看

td_url (str): [可选]指定交易服务器的地址, 默认使用快期账户对应的交易服务地址

number (int): [可选]模拟交易账号编号, 默认为主模拟账号, 可以通过指定 1~99 的数字来使用辅模拟帐号, 各个帐号的数据完全独立, 使用该功能需要购买专业版的权限, 且对应的辅账户可以在快期专业版上登录并进行观察

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

快期股票模拟为专业版功能，可以点击 天勤量化专业版 申请试用或购买

td_url (str): [可选]指定交易服务器的地址, 默认使用快期账户对应的交易服务地址

number (int): [可选]模拟交易账号编号, 默认为主模拟账号, 可以通过指定 1~99 的数字来使用辅模拟帐号, 各个帐号的数据完全独立

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
from tqsdk import TqApi, TqAuth, TqKq

tq_kq = TqKq()
api = TqApi(account=tq_kq, auth=TqAuth("快期账户", "账户密码"))
quote = api.get_quote("SHFE.cu2206")
print(quote)
# 下单限价单
order = api.insert_order(symbol="SHFE.cu2206", direction='BUY', offset='OPEN', limit_price=quote.last_price, volume=1)
while order.status == 'ALIVE':
    api.wait_update()
    print(order)  # 打印委托单信息

print(tq_kq.get_account())  # 打印快期模拟账户信息

print(tq_kq.get_position("SHFE.cu2206"))  # 打印持仓信息

for trade in order.trade_records.values():
    print(trade)  # 打印委托单对应的成交信息
api.close()
```

Example 2 (python):
```python
from tqsdk import TqApi, TqAuth, TqKq, TqMultiAccount

# 创建快期模拟账户和辅模拟账户001
tq_kq = TqKq()
tq_kq001= TqKq(number=1)

# 使用多账户模块同时登录这两个模拟账户
api = TqApi(account=TqMultiAccount([tq_kq,tq_kq001]), auth=TqAuth("快期账户", "账户密码"))

print(tq_kq.get_account())  # 打印快期模拟账户信息

print(tq_kq001.get_account())  # 打印快期模拟001账户信息

api.close()
```

Example 3 (python):
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

Example 4 (python):
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

---


## 算法模块示例

**URL:** https://doc.shinnytech.com/tqsdk/latest/demo/algorithm.html

**Contents:**
- 算法模块示例
- twap_table - 时间平均加权算法
- vwap_table - 交易量平均加权算法

twap_table - 时间平均加权算法

vwap_table - 交易量平均加权算法

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'yanqiong'

from tqsdk import TqApi, TargetPosScheduler
from tqsdk.algorithm import twap_table

"""
在 300s 时间内调整净持仓到 -100，每次下单手数在 5～10 手之间
"""

api = TqApi(auth="快期账号,用户密码")
quote = api.get_quote("CZCE.MA109")

# 设置twap任务参数
time_table = twap_table(api, "CZCE.MA109", -100, 600, 1, 5)  # 目标持仓 -100 手，600s 内完成
print(time_table.to_string())

target_pos_sch = TargetPosScheduler(api, "CZCE.MA109", time_table)
# 启动循环
while not target_pos_sch.is_finished():
    api.wait_update()
api.close()
```

Example 2 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'yanqiong'

from tqsdk import TqApi, TargetPosScheduler
from tqsdk.algorithm import vwap_table

"""
在 600s 时间内，按照历史成交量比例调整目标净持仓到 -100
"""

api = TqApi(auth="快期账号,用户密码")
quote = api.get_quote("CZCE.MA109")

# 设置twap任务参数
time_table = vwap_table(api, "CZCE.MA109", -100, 600)  # 目标持仓 -100 手，600s 内完成
print(time_table.to_string())

target_pos_sch = TargetPosScheduler(api, "CZCE.MA109", time_table)
# 启动循环
while not target_pos_sch.is_finished():
    api.wait_update()
api.close()
```

---


## 在 TqSdk 中调用 TqSdk2 查询保证金

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/tqsdk2ctptest.html

**Contents:**
- 在 TqSdk 中调用 TqSdk2 查询保证金

TqSdk 没有直接提供查询保证金的接口，但是你可以通过使用 TqSdk2 的直连功能来做到这个效果。tqsdk和tqsdk2可以在一个py文件中同时运行。

该方法仅支持 TqSdk2 中直连CTP 柜台时使用。受限制于 CTP 柜台的流控机制(每秒 1 笔), 短时间发送大量查询指令后, 后续查询指令将会排队等待。 为了避免盘中的查询等待时间, 建议盘前启动程序, 对标的合约提前进行查询:

TqSdk2 的直连功能需要企业版权限，有关企业版的具体费用和功能，请参考 天勤官方网站 如果想了解更多关于 TqSdk2 的直连功能TqCtp，请参考 tqsdk2官方文档

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth, TqAccount
import tqsdk2

account = tqsdk2.TqCtp(front_url, front_broker, app_id, auth_code, account_id, password)
api_margin = tqsdk2.TqApi(account = account, auth=tqsdk2.TqAuth("快期账户", "账户密码"))
rate = api_margin.get_margin_rates("SHFE.cu2201")
print(rate)
api = TqApi(TqAccount("期货公司","账号","密码"),auth=TqAuth("快期账户", "账户密码"))
quote = api.get_quote("SHFE.cu2201")
while True:
    api.wait_update()
    print(quote.datetime)
    # 正常和tqsdk一样执行策略
```

---


## 进阶主题

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/index.html

**Contents:**
- 进阶主题

这一部分内容提供给有经验的 TqSdk 用户, 主要讲解将 TqSdk 用于实际工作时的一些重要问题的处理方案和最佳实践.

© 版权所有 2018-2026, TianQin。

---

