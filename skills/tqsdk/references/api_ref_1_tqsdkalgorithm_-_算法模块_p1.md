# Api Ref - Part 1

**Topics:** tqsdk.algorithm - 算法模块, TargetPosTask 高级功能, 在无人监控环境下执行策略, tqsdk.TqAuth - 用户认证类, 将程序信息推送到手机端

---

## tqsdk.algorithm - 算法模块

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.algorithm.html

**Contents:**
- tqsdk.algorithm - 算法模块
- tqsdk.algorithm.twap - Twap 算法
- tqsdk.algorithm.time_table_generater - 生成 time_table 辅助函数

Twap 算法实现了在设定的交易时间段内，完成设定的下单手数。

构造 Twap 类的实例，该算法实例就会开始运行，根据以下逻辑下单：

将用户设置的总手数，拆分为一个随机手数列表，列表的值即为每次下单的手数，列表元素之和为总下单手数，同时每次下单手数也符合用户设置的每次下单手数的上下限；

将总的交易时间段拆分为随机时间间隔列表，列表的值即为每次下单的时间间隔，这些时间间隔相加应该等于总的下单时间；

每一次下单，在两个列表中分别取出下单手数、下单预计完成的时间，先用跟盘价下单，在当前时间间隔已经过去 2/3 或者只剩下 2s 时，主动撤掉未成交单，用对手价下单剩余手数；

在当前时间段已结束并且下单手数全部成交完，会开始下一次下单，重复第 3 步。

平均每次下单时间 = duration / 下单次数 > 3s

其中，下单次数 = 总的下单手数 / 平均每次下单手数 = 总的下单手数 / ((单次委托单最小下单手数 + 单次委托单最大下单手数) / 2)

时间段 duration，以 s 为单位，时长可以跨非交易时间段，但是不可以跨交易日。

比如，SHFE.cu2101 的白盘交易时间段为 ["09:00:00" ～ "10:15:00"], ["10:30:00", "11:30:00"], ["13:30:00", "15:00:00"]，duration 设置为 1200 (20分钟)。

如果当前行情时间是 2020-09-15 09:10:00，那么下单的时间应该在 2020-09-15 09:10:00 ～ 2020-09-15 09:30:00； 如果当前行情时间是 2020-09-15 10:10:00，那么下单的时间应该在 2020-09-15 10:10:00 ～ 2020-09-15 10:15:00，以及 2020-09-15 10:30:00 ～ 2020-09-15 10:45:00。

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 拟下单的合约symbol, 格式为 交易所代码.合约代码, 例如 "SHFE.cu1801"

direction (str): "BUY" 或 "SELL"

offset (str): "OPEN", "CLOSE"，"CLOSETODAY"

volume (int): 需要下单的总手数

duration (int): 算法执行的时长，以秒为单位，时长可以跨非交易时间段，但是不可以跨交易日 * 设置为 60*10, 可以是 10:10～10:15 + 10:30~10:35

min_volume_each_order (int):单笔最小委托单，每笔委托单数默认在最小和最大值中产生

max_volume_each_order (int):单笔最大委托单，每笔委托单数默认在最小和最大值中产生

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

取消当前 Twap 算法实例，会将该实例已经发出但还是未成交的委托单撤单。

返回当前 Twap 算法实例是否已经结束。即此实例不会再发出下单或者撤单的任何动作。

bool: 当前 Twap 算法实例是否已经结束

返回基于 twap 策略的计划任务时间表。下单需要配合 TargetPosScheduler 使用。

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 拟下单的合约 symbol, 格式为 交易所代码.合约代码, 例如 "SHFE.cu1801"

target_pos (int): 目标持仓手数

duration (int): 算法执行的时长，以秒为单位，时长可以跨非交易时间段，但是不可以跨交易日 * 设置为 60*10, 可以是 10:10～10:15 + 10:30~10:35

min_volume_each_step (int): 调整持仓手数最小值，每步调整的持仓手数默认在最小和最大值中产生

max_volume_each_step (int): 调整持仓手数最大值，每步调整的持仓手数默认在最小和最大值中产生

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

pandas.DataFrame: 本函数返回一个 pandas.DataFrame 实例. 表示一份计划任务时间表。每一行表示一项目标持仓任务，包含以下列:

interval: 当前这项任务的持续时间长度，单位为秒

target_pos: 当前这项任务的目标持仓

price: 当前这项任务的下单价格模式，支持 PASSIVE（排队价），ACTIVE（对价），None（不下单，表示暂停一段时间）

返回基于 vwap 策略的计划任务时间表。下单需要配合 TargetPosScheduler 使用。

调用 vwap_table 函数，根据以下逻辑生成 time_table：

根据 target_pos - 当前合约的净持仓，得到总的需要调整手数

请求 symbol 合约的 1min K 线

采样取用最近 10 日内，以合约当前行情时间的下一分钟为起点，每日 duration / 60 根 K 线, 例如当前合约时间为 14:35:35，那么采样是会使用 14:36:00 开始的分钟线 K 线

按日期分组，分别计算交易日内，每根 K 线成交量占总成交量的比例

计算最近 10 日内相同分钟内的成交量占比的算术平均数，将第 1 步得到的总调整手数按照得到的比例分配

每一分钟，前 58s 以追加价格下单，后 2s 以对价价格下单

api (TqApi): TqApi实例，该task依托于指定api下单/撤单

symbol (str): 拟下单的合约 symbol, 格式为 交易所代码.合约代码, 例如 "SHFE.cu2201"

target_pos (int): 目标持仓手数

duration (int): 算法执行的时长，以秒为单位，必须是 60 的整数倍，时长可以跨非交易时间段，但是不可以跨交易日 * 设置为 60*10, 可以是 10:10～10:15 + 10:30~10:35

account (TqAccount/TqKq/TqSim): [可选]指定发送下单指令的账户实例, 多账户模式下，该参数必须指定

pandas.DataFrame: 本函数返回一个 pandas.DataFrame 实例. 表示一份计划任务时间表。每一行表示一项目标持仓任务，包含以下列:

interval: 当前这项任务的持续时间长度，单位为秒

target_pos: 当前这项任务的目标持仓

price: 当前这项任务的下单价格模式，支持 PASSIVE（排队价），ACTIVE（对价），None（不下单，表示暂停一段时间）

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi
from tqsdk.algorithm import Twap

api = TqApi(auth="快期账户,用户密码")
# 设置twap任务参数
target_twap = Twap(api,"SHFE.rb2012","BUY","OPEN",500,300,10,25)
# 启动循环
while True:
  api.wait_update()
  if target_twap.is_finished():
      break
api.close()
```

Example 2 (python):
```python
from tqsdk import TqApi
from tqsdk.algorithm import Twap

api = TqApi(auth="快期账户,用户密码")
target_twap = Twap(api,"SHFE.rb2012","BUY","OPEN",500,300,10,25)

num_of_trades = 0

while True:
  api.wait_update()

  if num_of_trades < len(target_twap.trades):
    # 最新的成交
    for i in range(num_of_trades - len(target_twap.trades), 0):
      print("新的成交", target_twap.trades[i])
    print(target_twap.average_trade_price)  # 打印出当前已经成交的平均价格
    num_of_trades = len(target_twap.trades)

  if target_twap.is_finished():
      break

print("打印出 twap 全部成交以及成交均价")
print(target_twap.trades)
print(target_twap.average_trade_price)
api.close()
```

Example 3 (python):
```python
from tqsdk import TqApi
from tqsdk.algorithm import Twap

api = TqApi(auth="快期账户,用户密码")
# 设置twap任务参数
quote = api.get_quote("SHFE.rb2012")
target_twap = Twap(api,"SHFE.rb2012","BUY","OPEN",500,300,10,25)
api.wait_update()
# 运行代码。。。
target_twap.cancel()
while True:
  api.wait_update()
  if target_twap.is_finished():
      break
api.close()
```

Example 4 (python):
```python
from tqsdk import TqApi, TargetPosScheduler
from tqsdk.algorithm import twap_table

api = TqApi(auth="快期账户,用户密码")
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

---


## TargetPosTask 高级功能

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/targetpostask2.html

**Contents:**
- TargetPosTask 高级功能
- 应用情景说明
- 如何实现这样的功能

本篇文档假设您已经了解 TargetPosTask 的用法，文档参考 交易辅助工具。

本篇文档主要介绍 TargetPosTask 的高级用法。如何使用cancel() 和 is_finished() 方法。

任何时刻，每个账户下一个合约只能有一个 TargetPosTask 实例，并且其构造参数不能修改。

但是在某些情况下，用户会希望可以管理 TargetPosTask 实例。

比如说，用户使用 TargetPosTask 的 PASSIVE 模式进行下单，希望在收盘前取消所有挂单（包含 TargetPosTask 实例的未成委托单），并平仓。

TargetPosTask 类提供了 cancel() 和 is_finished() 方法。

cancel() 方法会取消当前 TargetPosTask 实例，会将该实例已经发出但还未成交的委托单撤单此实例的 set_target_volume 函数不会再生效，并且此实例的 set_target_volume 函数不会再生效。

is_finished() 方法可以获取当前 TargetPosTask 实例是否已经结束。已经结束实例的 set_target_volume 函数不会再接受参数，此实例不会再下单或者撤单。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from datetime import datetime, time
from tqsdk import TqApi, TargetPosTask

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
quote = api.get_quote("SHFE.rb2110")
target_pos_passive = TargetPosTask(api, "SHFE.rb2110", price="PASSIVE")

while datetime.strptime(quote.datetime, "%Y-%m-%d %H:%M:%S.%f").time() < time(14, 50):
    api.wait_update()
    # ... 策略代码 ...

# 取消 TargetPosTask 实例
target_pos_passive.cancel()

while not target_pos_passive.is_finished():  # 此循环等待 target_pos_passive 处理 cancel 结束
    api.wait_update()  # 调用wait_update()，会对已经发出但还是未成交的委托单撤单

# 创建新的 TargetPosTask 实例
target_pos_active = TargetPosTask(api, "SHFE.rb2110", price="ACTIVE")
target_pos_active.set_target_volume(0)  # 平所有仓位

while True:
    api.wait_update()
    # ... 策略代码 ...

api.close()
```

---


## 在无人监控环境下执行策略

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/unanttended.html

**Contents:**
- 在无人监控环境下执行策略
- 准备环境
- 在每个策略程序中设置实盘账号
- 检查策略程序
- 在 windows 环境下配置策略的定时启动/停止
- 在 linux 环境下配置策略的定时启动/停止
  - 将一个策略应用于多个合约或多个账户

对于已经过充分测试, 十分成熟的策略程序, 也可以选择以无人值守方式运行.

TqSdk可以在windows/linux或macosx环境下运行. 无论您选择使用windows或linux系统, 请确保

将每个策略程序配置为独立直连实盘账号. 在创建 TqApi 时, 传入TqAccount实例. 注意期货公司名称需要与天勤中的名称一致:

将策略代码投入无人监控运行前, 除对策略代码进行全面测试外, 还应注意以下事项:

使用 python 的 logging 模块输出日志信息到文件, 不要使用 print 打印日志信息

策略代码退出时记得调用 api.close() 函数, 或者用 with closing(api) 的格式确保退出时自动关闭

目前api在运行过程中抛出的异常, 默认处理都是整个策略进程直接退出. 如无特殊需求, 不要使用 expect: 的方式捕获异常并阻止程序退出, 这种情况如果没有正确处理, 可能产生难以预测的后果.

在 windows 下, 通常使用计划任务来管理策略的定时启动/停止, 下面的说明以 Windows 10 为例, 其它 windows 版本操作可能有少许差异.

为每个策略添加一个策略启动任务, [程序或脚本]处填 python.exe, [添加参数]处填策略代码py文件名和参数, [起始于]处填策略代码目录

最后添加一个任务, 用来停止所有策略进程. [程序或脚本]处填 taskkill, [添加参数]处填 /IM python.exe

在 linux 下, 通常使用 cron 服务来处理策略的定时启动/停止. 具体配置请参考您所使用linux发行版的相应文档.

将一个策略应用于多个合约或多个账户是一个常见需求. 我们推荐使用 命令行参数 来传递合约或账户信息. 请看下面例子:

上面的代码中固定了账户及合约代码 SHFE.rb1901. 我们可以利用 python 的 argparse 模块为这个程序添加一些参数:

要在 PyCharm 中同时执行此程序的多种参数版本, 可以通过 PyCharm 的 Run Configuration 实现.

先在 Edit Configuration 中, 为每组参数创建一个运行项

在 Edit Configuration 中配置好以后, 通过 Run... 菜单选择保存好的运行项, 即可实现带参数运行

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
api = TqApi(TqAccount("H宏源期货", "022631", "123456"), auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
#  -*- coding: utf-8 -*-

from tqsdk import TqApi, TqAccount

api = TqApi(TqAccount("H宏源期货", "0330203", "123456"), auth=TqAuth("快期账户", "账户密码"))
# 开仓两手并等待完成
order = api.insert_order(symbol="SHFE.rb1901", direction="BUY", offset="OPEN", limit_price=4310,volume=2)
while order.status != "FINISHED":
    api.wait_update()
print("已开仓")
```

Example 3 (python):
```python
#  -*- coding: utf-8 -*-

import argparse
from tqsdk import TqApi, TqSim, TqAccount

#解析命令行参数
parser = argparse.ArgumentParser()
parser.add_argument('--broker')
parser.add_argument('--user_name')
parser.add_argument('--password')
parser.add_argument('--symbol')
args = parser.parse_args()
print("策略参数为: ", args.user_name, args.symbol)

api = TqApi(TqAccount(args.broker, args.user_name, args.password), auth=TqAuth("快期账户", "账户密码"))
# 开仓两手并等待完成
order = api.insert_order(symbol=args.symbol, direction="BUY", offset="OPEN", limit_price=4310,volume=2)
while order.status != "FINISHED":
    api.wait_update()
print("已开仓")
```

Example 4 (unknown):
```unknown
python args.py --broker=H宏源期货 --user_name=0330203 --password=123456 --symbol=SHFE.cu1901
```

---


## tqsdk.TqAuth - 用户认证类

**URL:** https://doc.shinnytech.com/tqsdk/latest/reference/tqsdk.auth.html

**Contents:**
- tqsdk.TqAuth - 用户认证类

user_name (str): [必填]快期账户，可以是 邮箱、用户名、手机号

password (str): [必填]快期账户密码

返回授权到期时间，如果查询失败则抛出异常。

注意: 1. 对于免费用户，将返回到期时间："2099-12-31 23:59:59+08:00"，代表权限永不到期。

当用户升级版本或者续费后，将在下一次使用时获取最新到期时间。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
# 使用实盘帐号直连行情和交易服务器
from tqsdk import TqApi, TqAccount, TqAuth
api = TqApi(TqAccount("H海通期货", "022631", "123456"), auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
from tqsdk import TqApi, TqAuth

