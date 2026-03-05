# Api Ref - Part 8

**Topics:** tqsdk.TqCtp - 直连 CTP 交易类, tqsdk.TqZq - 众期交易类, tqsdk.TqYida - 易达交易类

---

## tqsdk.TqCtp - 直连 CTP 交易类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tqctp.html

**Contents:**
- tqsdk.TqCtp - 直连 CTP 交易类

front_broker (str): CTP 柜台代码

front_url (str): CTP 柜台地址，格式为 tcp://ip:port，如 tcp://129.211.138.170:10001

app_id (str): CTP AppID

auth_code (str): CTP AuthCode

使用 TqCtp 账户需要安装 tqsdk_zq_otg 包： pip install -U tqsdk_zq_otg

front_broker, front_url, app_id 和 auth_code 信息需要向期货公司申请程序化外接后取得

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
from tqsdk import TqApi, TqCtp
account = TqCtp(account_id="CTP 账户", password="CTP 密码", front_broker="CTP 柜台代码", front_url="CTP 柜台地址", app_id="CTP AppID", auth_code="CTP AuthCode")
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


## tqsdk.TqZq - 众期交易类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tqzq.html

**Contents:**
- tqsdk.TqZq - 众期交易类

td_url (str): 众期交易服务器地址, eg: "ws://1.2.3.4:8765/"

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
from tqsdk import TqApi, TqZq
account = TqZq(account_id="众期账户", password="众期密码", td_url="众期柜台地址")
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


## tqsdk.TqYida - 易达交易类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.tqyida.html

**Contents:**
- tqsdk.TqYida - 易达交易类

front_url (str): 易达柜台地址，格式为 tcp://ip:port，如 tcp://129.211.138.170:10001

app_id (str): 易达 AppID

auth_code (str): 易达 AuthCode

使用 TqYida 账户需要安装 tqsdk_zq_otg 包： pip install -U tqsdk_zq_otg

front_url, app_id 和 auth_code 信息需要向易达申请程序化外接后取得

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
from tqsdk import TqApi, TqYida
account = TqYida(account_id="易达账户", password="易达密码", front_url="易达柜台地址", app_id="易达 AppID", auth_code="易达 AuthCode")
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

