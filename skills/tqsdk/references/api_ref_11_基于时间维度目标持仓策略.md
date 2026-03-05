# Api Ref - Part 11

**Topics:** 基于时间维度目标持仓策略, TianQin Python Sdk User Guide, 与Gui库共同工作, 天勤用户论坛, 交易策略的多实例运行

---

## 基于时间维度目标持仓策略

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/scheduler.html

**Contents:**
- 基于时间维度目标持仓策略
- time_table 目标持仓任务列表
- TargetPosScheduler 执行目标持仓任务列表
- 简单示例
- 基于 TargetPosScheduler 的 twap 策略示例

本篇文档假设您已经了解 TargetPosTask 的用法，文档参考 交易辅助工具。

简单来说，TargetPosTask 会创建 task，负责将指定合约调整到目标头寸（默认为账户的该合约净持仓）。

对于简单的大单拆分功能，可以在 TargetPosTask 类中设置拆分手数的上下限，TargetPosTask 实例在下单过程中就会将下单手数随机的拆分，以减少对市场冲击。

但是，对于比较复杂的下单策略，例如 twap（基于时间拆分手数），vwap（基于成交量拆分手数）等，使用 TargetPosTask 来构造策略不是很方便。

我们提供 TargetPosScheduler 类帮助用户完成复杂的下单策略，同时提供给用户极大的调整空间。

TargetPosScheduler 使用 time_table 参数来描述具体的下单策略。

time_table 为 pandas.DataFrame 类型。每一行表示一项目标持仓任务，每项任务按照顺序一个个执行。其应该包含以下几列：

注意1：对于最后一项任务，会按照当前项参数，调整到目标持仓后立即退出（时间参数不对最后一项任务起作用）

注意2：时间长度可以跨非交易时间段（可以跨小节等待），但是不可以跨交易日

target_pos: 当前这项任务的目标净持仓手数

Callable (direction: str) -> Union[float, int]: 传入函数作为价格参数，函数参数为下单方向，函数返回值是下单价格。如果返回 nan，程序会抛错。

TargetPosScheduler 类创建 target_pos_scheduler 实例，首先会将 time_table 中 interval 间隔时间列转为 deadline，即这项任务结束时间的纳秒数。

然后，依次为 time_table 中的每一项任务创建 TargetPosTask 实例，调整目标持仓，并在到达 deadline 时退出。每一项未完成的目标持仓都会留都下一项任务中。

需要注意的是，最后一项任务，是以手数达到目标的，会按照当前项参数，调整到目标持仓再退出。如果最后一项 price 参数为 None （表示不下单），由于无法调整持仓，那么会立即退出。

到此为止，您可以根据您的具体策略构造出任意的 time_table 对象，然后调用 TargetPosScheduler 来执行。

为了方便用户使用，我们提供了 twap_table() 来生成一个默认的符合 twap 策略的 time_table 实例。

我们在 tqsdk.algorithm - 算法模块 模块中提供了 twap_table()，可方便的生成一个基于 twap 策略的 time_table 实例。

在执行算法之前，您还可以定制化的调整 time_table 中的具体任务项。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (json):
```json
time_table = DataFrame([
    [25, 10, "PASSIVE"]
    [5, 10, "ACTIVE"]
    [30, 18, "PASSIVE"]
    [5, 18, "ACTIVE"]
], columns=['interval', 'target_pos', 'price'])

target_pos_scheduler = TargetPosScheduler(api, "SHFE.cu2112", time_table)

# 这个 time_table 表示的下单策略依次是：
# 1. 使用排队价下单，调整 "SHFE.cu2112" 到 10 手，到达 25s 时退出（无论目标手数是否达到，都不会继续下单）
# 2. 使用对价下单，调整 "SHFE.cu2112" 到 10 手，到达 5s 时退出
#    如果上一步结束时目标持仓已经达到 10 手，这一步什么都不会做，等待 5s 到下一步；
#    如果上一步结束时目标持仓没有达到 10 手，这一步会继续调整目标持仓到 10 手
# 3. 使用排队价下单，调整 "SHFE.cu2112" 到 18 手，到达 30s 时退出（无论目标手数是否达到，都不会继续下单）
# 4. 使用对价下单，调整 "SHFE.cu2112" 到 18 手
#    如果上一步结束时目标持仓已经达到 18 手，这一步什么都不会做，立即退出；
#    如果上一步结束时目标持仓没有达到 18 手，这一步会继续调整目标持仓到 18 手后退出
```

Example 2 (python):
```python
from tqsdk import TqApi, TargetPosScheduler
from tqsdk.algorithm import twap_table

api = TqApi(auth="快期账户,用户密码")
quote = api.get_quote("CZCE.MA109")

# 设置 twap 任务参数，
time_table = twap_table(api, "CZCE.MA105", -100, 600, 1, 5)  # 目标持仓 -100 手，600s 内完成

# 定制化调整 time_table，例如希望第一项任务延迟 10s 再开始下单
# 可以在 time_table 的头部加一行
time_table = pandas.concat([
    DataFrame([[10, 10, None]], columns=['interval', 'target_pos', 'price']),
    time_table
], ignore_index=True)

target_pos_sch = TargetPosScheduler(api, "CZCE.MA105", time_table)
while not target_pos_sch.is_finished():
    api.wait_update()

# 获取 target_pos_sch 实例所有的成交列表
print(target_pos_sch.trades_df)

# 利用成交列表，您可以计算出策略的各种表现指标，例如：
average_trade_price = sum(scheduler.trades_df['price'] * scheduler.trades_df['volume']) / sum(scheduler.trades_df['volume'])
print("成交均价:", average_trade_price)
api.close()
```

---


## TianQin Python Sdk User Guide

**URL:** https://doc.shinnytech.com/tqsdk/latest/

**Contents:**
- TianQin Python Sdk User Guide

本文档是 TqSdk 的使用说明. 从 TqSdk 中的一些关键概念开始, 逐步介绍如何充分利用 TqSdk 全部功能。

© 版权所有 2018-2026, TianQin。

---


## 与Gui库共同工作

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/gui.html

**Contents:**
- 与Gui库共同工作
- 先后使用GUI库和TqSdk
- 在TqSdk任务中驱动Gui消息循环

某些情况下, 我们可能需要在一个 Python GUI 程序中使用TqSdk库. TqSdk 可以与Tkinter, PyQt, WxPython, PySimpleGui 等大多数常见 Python Gui 库配合工作.

下面以 PySimpleGui 为例, 介绍 Gui 库与 TqSdk 组合使用的方式.

参见示例程序 param_input.py. 这个程序先使用 PySimpleGui 创建一个参数输入对话框, 用户输入参数后, 关闭对话框, 开始使用 TqSdk:

参见示例程序 loop_integrate.py.

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (swift):
```swift
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'limin'

"""
本程序演示如何为策略程序增加一个GUI参数输入框, 方便用户在运行策略前输入运行参数
"""

import PySimpleGUI as sg
from tqsdk import TqApi, TargetPosTask, TqAccount
from tqsdk.tafunc import ma

# 创建一个参数输入对话框
layout = [[sg.Text('交易账户')],
          [sg.Text('期货公司'), sg.Input("快期模拟", key="broker_id")],
          [sg.Text('账号'), sg.Input("111", key="user_id")],
          [sg.Text('密码'), sg.Input("111", key="password")],
          [sg.Text('策略参数')],
          [sg.Text('合约代码'), sg.Input("SHFE.bu1912", key="symbol")],
          [sg.Text('短周期'), sg.Input(30, key="short")],
          [sg.Text('长周期'), sg.Input(60, key="long")],
          [sg.OK(), sg.Cancel()]]
window = sg.Window('请输入策略运行参数', layout)

# 读取用户输入值
event, values = window.Read()
print(event, values)
window.close()


# 正常运行策略代码
SHORT = int(values["short"])  # 短周期
LONG = int(values["long"])  # 长周期
SYMBOL = values["symbol"]

api = TqApi(TqAccount(values["broker_id"], values["user_id"], values["password"]))
print("策略开始运行")

data_length = LONG + 2  # k线数据长度
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

Example 2 (python):
```python
import asyncio
import sys
import PySimpleGUI as sg
from tqsdk import TqApi, TqAuth

loop = asyncio.get_event_loop()
api = TqApi(loop=loop, auth=TqAuth("快期账户", "账户密码"))
quote_a = api.get_quote("SHFE.rb1910")
quote_b = api.get_quote("SHFE.rb2001")


async def gui_task():
    layout = [[sg.Text('rb1910'), sg.Text("99999", key="rb1910.last")],
              [sg.Text('rb2001'), sg.Text("99999", key="rb2001.last")],
              [sg.Text('spread'), sg.Text("99999", key="spread")],
              ]

    window = sg.Window('价差显示', layout)

    while True:
        event, values = window.Read(timeout=0)
        if event is None or event == 'Exit':
            sys.exit(0)
        window.Element('rb1910.last').Update(quote_a.last_price)
        window.Element('rb2001.last').Update(quote_b.last_price)
        window.Element('spread').Update(quote_b.last_price - quote_a.last_price)
        await asyncio.sleep(1)  # 注意, 这里必须使用 asyncio.sleep, 不能用time.sleep


api.create_task(gui_task())

while True:
    api.wait_update()
```

---


## 天勤用户论坛

**URL:** https://doc.shinnytech.com/tqsdk/latest/qa.html

**Contents:**
- 天勤用户论坛

在学习TqSdk的过程中可能会碰到一些疑惑，我们相信用户论坛能够帮助到你。

© 版权所有 2018-2026, TianQin。

---


## 交易策略的多实例运行

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/multi_strategy.html

**Contents:**
- 交易策略的多实例运行
- 每个进程执行一个策略实例
- 单线程创建多个异步任务

我们可能会将一个策略应用于不同的目标品种, 不同品种使用的策略参数也不同.

以简单的双均线策略为例. 一个简单的双均线策略代码大致是这样:

我们可能需要将这个策略运行多份, 每份的 SYMBOL, LONG, SHORT 都不同.

TqSdk 为这类需求提供两种解决方案, 您可任意选择一种.

最简单的办法是直接将上面的程序复制为N个文件, 手工修改每个文件中的 SYMBOL, SHORT, LONG 的值, 再把N个程序分别启动运行即可达到目的.

如果觉得代码复制N份会导致修改不方便, 可以简单的剥离一个函数文件, 每个策略实例文件引用它:

每个策略进程要建立一个单独的服务器连接, 数量过大时可能无法连接成功

TqSdk 内核支持以异步方式实现多任务。 如果用户策略代码实现为一个异步任务, 即可在单线程内执行多个策略。

TqSdk（2.6.1 版本）对几个常用接口 get_quote(), get_quote_list(), get_kline_serial(), get_tick_serial() 支持协程中调用。

对于 get_quote() 接口，在异步代码中可以写为 await api.get_quote('SHFE.cu2110')，代码更加紧凑，可读性更好。

下面是一个更完整的示例，用异步方式实现为每个合约创建双均线策略，示例代码如下:

单线程内执行多个策略, 只消耗一份网络连接

没有线程或进程切换成本, 性能高, 延时低, 内存消耗小, 性能最优

用户需熟练掌握 asyncio 异步编程, 学习成本高

example 中的 gridtrading_async.py 就是一个完全按异步框架实现的网格交易策略. 有意学习的同学可以与 gridtrading.py 对比一下

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (typescript):
```typescript
SYMBOL = "SHFE.bu1912"  # 合约代码
SHORT = 30  # 短周期
LONG = 60  # 长周期

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

klines = api.get_kline_serial(SYMBOL, duration_seconds=60, data_length=LONG + 2)
target_pos = TargetPosTask(api, SYMBOL)

while True:
    api.wait_update()
    if api.is_changing(klines.iloc[-1], "datetime"):
        short_avg = ma(klines["close"], SHORT)
        long_avg = ma(klines["close"], LONG)
        if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
            target_pos.set_target_volume(-3)
            print("均线下穿，做空")
        if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
            target_pos.set_target_volume(3)
            print("均线上穿，做多")
```

Example 2 (python):
```python
在函数文件 mylib.py 中:

def ma(SYMBOL, SHORT, LONG):
    api = TqApi(TqSim())

    klines = api.get_kline_serial(SYMBOL, duration_seconds=60, data_length=LONG + 2)
    target_pos = TargetPosTask(api, SYMBOL)

    while True:
        api.wait_update()
        if api.is_changing(klines.iloc[-1], "datetime"):
            short_avg = ma(klines["close"], SHORT)
            long_avg = ma(klines["close"], LONG)
            if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
                target_pos.set_target_volume(-3)
                print("均线下穿，做空")
            if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
                target_pos.set_target_volume(3)
                print("均线上穿，做多")

--------------------------------------------------
在策略文件 ma-股指.py 中:

from mylib import ma
ma("CFFEX.IF1906", 30, 60)

--------------------------------------------------
在策略文件 ma-玉米.py 中:

from mylib import ma
ma("DCE.c1906", 10, 20)
```

Example 3 (python):
```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument('--SYMBOL')
parser.add_argument('--SHORT')
parser.add_argument('--LONG')
args = parser.parse_args()

api = TqApi(TqSim())
klines = api.get_kline_serial(args.SYMBOL, duration_seconds=60, data_length=args.LONG + 2)
target_pos = TargetPosTask(api, args.SYMBOL)
while True:
    api.wait_update()
    if api.is_changing(klines.iloc[-1], "datetime"):
        short_avg = ma(klines["close"], args.SHORT)
        long_avg = ma(klines["close"], args.LONG)
        if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
            target_pos.set_target_volume(-3)
            print("均线下穿，做空")
        if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
            target_pos.set_target_volume(3)
            print("均线上穿，做多")
```

Example 4 (unknown):
```unknown
python ma.py --SYMBOL=SHFE.cu1901 --LONG=30 --SHORT=20
python ma.py --SYMBOL=SHFE.rb1901 --LONG=50 --SHORT=10
```

---

