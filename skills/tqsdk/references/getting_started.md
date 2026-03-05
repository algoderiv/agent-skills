# Tqsdk-Dev-Guide - Getting Started

**Pages:** 2

---

## TqSdk 介绍

**URL:** https://doc.shinnytech.com/tqsdk/latest/intro.html

**Contents:**
- TqSdk 介绍
- TqSdk是什么
- 系统架构
- 功能要点
- 编程风格
- License

TqSdk 是一个由 信易科技 发起并贡献主要代码的开源 python 库. 依托 快期多年积累成熟的交易及行情服务器体系 , TqSdk 支持用户使用很少的代码量构建各种类型的量化交易策略程序, 并提供包含 历史数据-实时数据-开发调试-策略回测-模拟交易-实盘交易-运行监控-风险管理 的全套解决方案:

要快速了解如何使用TqSdk, 可以访问我们的 十分钟快速入门

行情网关 (Open Md Gateway) 负责提供实时行情和历史数据

交易中继网关 (Open Trade Gateway) 负责连接到期货公司交易系统

这两个网关统一以 Diff协议 对下方提供服务

TqSdk按照Diff协议连接到行情网关和交易中继网关, 实现行情和交易功能

TqSdk 提供的功能可以支持从简单到复杂的各类策略程序.

提供当前所有可交易合约从上市开始的 全部Tick数据和K线数据

支持 Tick级和K线级回测, 支持 复杂策略回测

用户无须建立和维护数据库, 行情和交易数据全在 内存数据库 , 无访问延迟

优化支持 pandas 和 numpy 库

无强制框架结构, 支持任意复杂度的策略, 在一个交易策略程序中使用多个品种的K线/实时行情并交易多个品种

TqSdk使用单线程异步模型, 它支持构建各类复杂结构的策略程序, 同时保持高性能及高可读性. 要了解 TqSdk 的编程框架, 请参见 策略程序结构

如果您曾经使用并熟悉过其它量化交易开发工具, 这些文件可以帮助您尽快了解TqSdk与它们的差异:

TqSdk与使用Ctp接口开发策略程序有哪些差别

TqSdk 在 Apache License 2.0 协议下提供, 使用者可在遵循此协议的前提下自由使用本软件.

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth, TqAccount, TargetPosTask

api = TqApi(TqAccount("H宏源期货", "4003242", "123456"), auth=TqAuth("快期账户", "账户密码"))      # 创建 TqApi 实例, 指定交易账户
q_1910 = api.get_quote("SHFE.rb1910")                         # 订阅近月合约行情
t_1910 = TargetPosTask(api, "SHFE.rb1910")                    # 创建近月合约调仓工具
q_2001 = api.get_quote("SHFE.rb2001")                         # 订阅远月合约行情
t_2001 = TargetPosTask(api, "SHFE.rb2001")                    # 创建远月合约调仓工具

while True:
  api.wait_update()                                           # 等待数据更新
  spread = q_1910.last_price - q_2001.last_price        # 计算近月合约-远月合约价差
  print("当前价差:", spread)
  if spread > 250:
    print("价差过高: 空近月，多远月")
    t_1910.set_target_volume(-1)                              # 要求把1910合约调整为空头1手
    t_2001.set_target_volume(1)                               # 要求把2001合约调整为多头1手
  elif spread < 200:
    print("价差回复: 清空持仓")                               # 要求把 1910 和 2001合约都调整为不持仓
    t_1910.set_target_volume(0)
    t_2001.set_target_volume(0)
```

---

## 十分钟快速入门

**URL:** https://doc.shinnytech.com/tqsdk/latest/quickstart.html

**Contents:**
- 十分钟快速入门
- 安装
- 注册快期账户
- 获取实时行情数据
- 使用K线数据
- 交易账户, 下单/撤单
- 构建一个自动交易程序
- 按照目标持仓自动交易
- 策略回测
- 实盘交易

希望快速开始使用天勤量化(TqSdk)？ 本页面将介绍如何开始使用 TqSdk.

如果您以前曾经使用过其它框架编写过策略程序, 这些内容可以快速帮助您了解 TqSdk 与它们的区别:

TqSdk与使用Ctp接口开发策略程序有哪些差别

注意: TqSdk 使用了 python3 的原生协程和异步通讯库 asyncio，部分 Python IDE 不支持 asyncio，例如:

spyder: 详见 https://github.com/spyder-ide/spyder/issues/7096

强烈推荐使用提供 AI 编辑能力的 IDE，例如 在 Cursor 中高效学习和使用 TqSdk 、在 Trae 中高效学习和使用 TqSdk 、VSCode等，可以大大提高开发效率。更多内容请见 通过AI编辑器来使用 TqSdk 。

天勤量化的核心是TqSdk开发包, 在安装天勤量化 (TqSdk) 前, 你需要先准备适当的环境和Python包管理工具, 包括:

Windows 7 以上版本, Mac Os, 或 Linux

你可以选择使用 pip 命令安装 TqSdk, 或者下载源代码安装. 对于一般用户, 我们推荐采用 pip 命令安装/升级 TqSdk:

但是由于 pip 使用的是国外的服务器，普通用户往往下载速度过慢或不稳定，对于使用 pip 命令下载速度较慢的用户，我们推荐采用切换国内源的方式安装/升级 TqSdk:

在使用 TqSdk 之前，用户需要先注册自己的 快期账户 ，传入快期账户是使用任何 TqSdk 程序的前提,点击 注册快期账户

快期账户可以使用注册时的手机号/用户名/邮箱号进行登录,详细介绍请点击 快期账户

在注册完快期账户后，让我们从一个简单的例子开始

通过 TqSdk 获取实时行情数据是很容易的.

获得上期所 ni2206 合约的行情引用:

现在, 我们获得了一个对象 quote. 这个对象总是指向 SHFE.ni2206 合约的最新行情. 我们可以通过 quote 的各个字段访问行情数据:

要等待行情数据更新, 我们还需要一些代码:

wait_update() 是一个阻塞函数, 程序在这行上等待, 直到收到数据包才返回.

上面这个例子的完整程序请见 t10 - 获取实时行情 . 你也可以在自己电脑python安装目录的 site_packages/tqsdk/demo 下找到它

很简单, 对吗? 到这里, 你已经了解用 TqSdk 开发程序的几个关键点:

用 api.get_quote() 或 其它函数获取数据引用对象

在循环中用 api.wait_update() 等待数据包.

下面我们将继续介绍 TqSdk 更多的功能. 无论使用哪个功能函数, 都遵循上面的结构.

你很可能会需要合约的K线数据. 在TqSdk中, 你可以很方便的获得K线数据. 我们来请求 ni2206 合约的10秒线:

klines是一个pandas.DataFrame对象. 跟 api.get_quote() 一样, api.get_kline_serial() 也是返回K线序列的引用对象. K线序列数据也会跟实时行情一起同步自动更新. 你也同样需要用 api.wait_update() 等待数据刷新.

一旦k线数据收到, 你可以通过 klines 访问 k线数据:

这部分的完整示例程序请见 t30 - 使用K线/Tick数据 .

到这里为止, 你已经知道了如何获取实时行情和K线数据, 下面一段将介绍如何访问你的交易账户并发送交易指令

要获得你的账户资金情况, 可以请求一个资金账户引用对象:

要获得你交易账户中某个合约的持仓情况, 可以请求一个持仓引用对象:

与行情数据一样, 它们也通过 api.wait_update() 获得更新, 你也同样可以访问它们的成员变量:

要在交易账户中发出一个委托单, 使用 api.insert_order() 函数:

这个函数调用后会立即返回, order 是一个指向此委托单的引用对象, 你总是可以通过它的成员变量来了解委托单的最新状态:

要撤销一个委托单, 使用 api.cancel_order() 函数:

这部分的完整示例程序请见 t40 - 下单/撤单 .

到这里为止, 我们已经掌握了 TqSdk 中行情和交易相关功能的基本使用. 我们将在下一节中, 组合使用它们, 创建一个自动交易程序

在这一节中, 我们将创建一个简单的自动交易程序: 每当行情最新价高于最近15分钟均价时, 开仓买进. 这个程序是这样的:

上面的代码中出现了一个新函数 api.is_changing(). 这个函数用于判定指定对象是否在最近一次 wait_update 中被更新.

这部分的完整示例程序请见 t60 - 单均线策略 .

在某些场景中, 我们可能会发现, 自己写代码管理下单撤单是一件很麻烦的事情. 在这种情况下, 你可以使用 tqsdk.TargetPosTask. 你只需要指定账户中预期应有的持仓手数, TqSdk 会自动通过一系列指令调整仓位直到达成目标. 请看例子:

这部分的完整示例程序请见 t80 - 价差回归策略 .

自己的交易程序写好以后, 我们总是希望在实盘运行前, 能先进行一下模拟测试. 要进行模拟测试, 只需要在创建TqApi实例时, 传入一个backtest参数:

这样, 程序运行时就会按照 TqBacktest 指定的时间范围进行模拟交易测试, 并输出测试结果.

此外 TqSdk 同时还支持股票的回测交易，请见 对股票合约进行回测

更多关于策略程序回测的详细信息, 请见 策略程序回测

要让策略程序在实盘账号运行, 请在创建TqApi时传入一个 TqAccount , 填入 期货公司, 账号, 密码 和快期账户信息(使用前请先 import TqAccount):

api = TqApi(TqAccount("H宏源期货", "412432343", "123456"), auth=TqAuth("快期账户", "账户密码"))

其中实盘交易是属于 TqSdk 的专业版功能，用户需要购买 TqSdk 专业版才可以进行实盘交易， 点击申请试用或者购买

于此同时，TqSdk 支持在部分的期货公司开户来进行免费的实盘交易，详细期货公司介绍请点击查看 TqSdk支持的期货公司列表

如果您需要使用能保存账户资金及持仓信息的模拟交易功能,通过 TqKq 对 auth 传入参数进行登录，可以得到一个长期有效的快期模拟账户，快期模拟账户在快期APP、快期专业版、快期v2、快期v3 和天勤量化上是互通的

快期模拟的资金可以通过快期APP、快期专业版的模拟银行进行出入金，也可以通过快期专业版对该账户进行重置:

特别的，如果创建TqApi实例时没有提供任何 TqAcccount 账户或 TqKq 模块，则每次会自动创建一个临时模拟账号，当程序运行结束时，临时账号内的记录将全部丢失:

TqSdk 提供简单易懂的十分钟上手视频 供用户学习

要完整了解TqSdk的使用, 请阅读 使用TqSdk

更多TqSdk的示例, 请见 交易策略示例

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
pip install tqsdk -U
```

Example 2 (unknown):
```unknown
pip install tqsdk -U -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host=pypi.tuna.tsinghua.edu.cn
```

Example 3 (python):
```python
from tqsdk import TqApi, TqAuth
```

Example 4 (unknown):
```unknown
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
```

---
