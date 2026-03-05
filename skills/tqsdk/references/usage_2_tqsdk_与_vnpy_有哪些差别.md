# Usage - Part 2

**Topics:** TqSdk 与 vn.py 有哪些差别, TqSdk与使用Ctp接口开发策略程序有哪些差别, 交易辅助工具

---

## TqSdk 与 vn.py 有哪些差别

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/for_vnpy_user.html

**Contents:**
- TqSdk 与 vn.py 有哪些差别
- 系统整体架构
- 每个策略是一个单独运行的py文件
- K线数据与指标计算
- 数据接收和更新
- 图形界面
- 回测
- 推荐学习步骤

TqSdk 与 vn.py 有非常多的差别. 如果您是一位有经验的 vn.py 用户, 刚开始接触 TqSdk, 下面的信息将帮助您尽快理解 TqSdk.

vn.py 是一套 all-in-one 的结构, 在一个Python软件包中包含了数据库, 行情接收/存储, 交易接口, 图形界面等功能.

TqSdk 则使用基于网络协作的组件设计. 如下图:

如图所示, 整个系统结构包括这些关键组件:

行情网关 (Open Md Gateway) 负责提供实时行情和历史数据

交易中继网关 (Open Trade Gateway) 负责连接到期货公司交易系统

上面两个网关统一以 Diff 协议对下方提供服务

天勤终端和TqSdk按照Diff协议连接到行情网关和交易中继网关, 实现行情和交易功能

TqSdk 很小, 安装也很方便, 只要简单 pip install tqsdk 即可

官方专门运维行情数据库, 用户可以直接使用, 不需要自己接收和存储数据

交易相关接口被大幅度简化, 不再需要处理CTP接口的复杂回调, 也不需要发起任何查询请求

对于需要直连期货公司交易的用户, TqSdk 也提供了 TqCtp() 模块支持用户直连

在 vn.py 中, 要实现一个策略程序, 通常是从 CtaTemplate 等基类派生一个子类, 像这样:

这个 DoubleMaStrategy 类写好以后, 由vn.py的策略管理器负责加载运行. 整个程序结构中, vn.py作为调用方, 用户代码作为被调用方, 结构图是这样的:

而在 TqSdk 中, 策略程序并没有一个统一的基类. TqSdk只是提供一些行情和交易函数, 用户可以任意组合它们来实现自己的策略程序, 还是以双均线策略为例:

以上代码文件单独运行, 即可执行一个双均线交易策略. 整个程序结构中, 用户代码作为调用方, TqSdk库代码作为被调用方, 每个策略是完全独立的. 结构是这样:

TqSdk将每个策略作为一个独立进程运行, 这样就可以:

在运行多策略时可以充分利用多CPU的计算能力

每个策略都可以随时启动/停止/调试/修改代码, 而不影响其它策略程序的运行

在策略程序中, 用户代码可以随意调用 TqSdk 包中的任意函数, 这带来了更大的自由度, 比如:

在一个策略程序中使用多个合约或周期的K线数据, 盘口数据和Tick数据. 对于某些类型的策略来说这是很方便的

在这个程序中, 我们同时跟踪两个合约的行情信息, 并为两个合约各创建一个调仓任务, 可以方便的实现套利策略

使用vn.py时, K线是由vn.py接收实时行情, 并在用户电脑上生成K线, 存储于用户电脑上的数据库中.

而在TqSdk中, K线数据和其它行情数据一样是由行情网关生成并推送的. 这带来了一些差别:

用户不再需要维护K线数据库. 用户电脑实时行情中断后, 也不再需要补历史数据

行情服务器生成K线时, 采用了按K线时间严格补全对齐的算法. 这与vn.py或其它软件有明显区别, 详见 https://www.shinnytech.com/blog/why-our-kline-different/

行情数据只在每次程序运行时通过网络获取, 不在用户硬盘保存. 如果策略研究工作需要大量静态历史数据, 我们推荐使用数据下载工具, 另行下载csv文件使用.

TqSdk中的K线序列采用 pandas.DataFrame 格式. pandas 提供了 非常丰富的数据处理函数 , 使我们可以非常方便的进行数据处理, 例如:

TqSdk 也通过 tqsdk.tafunc 提供了一批行情分析中常用的计算函数, 例如:

vn.py按照事件回调模型设计, 使用 CtaTemplate 的 on_xxx 回调函数进行行情数据和回单处理:

TqSdk则不使用事件回调机制. wait_update() 函数设计用来获取任意数据更新, 像这样:

一次 wait_update 可能更新多个实体, 在这种情况下, is_changing() 被用来判断某个实体是否有变更:

TqSdk针对行情数据和交易信息都采用相同的 wait_update/is_changing 方案. 用户需要记住的要点包括:

get_quote, get_kline_serial, insert_order 等业务函数返回的是一个引用(refrence, not value), 它们的值总是在 wait_update 时更新.

用户程序除执行自己业务逻辑外, 需要反复调用 wait_update. 在两次 wait_update 间, 所有数据都不更新

用 insert_order 函数下单, 报单指令实际是在 insert_order 后调用 wait_update 时发出的.

用户程序中需要避免阻塞, 不要使用 sleep 暂停程序

关于 wait_update 机制的详细说明, 请见 策略程序结构

TqSdk 提供 策略程序图形化界面 来供有图形化需求的用户使用:

策略运行时, 交易记录和持仓记录自动在行情图上标记, 可以快速定位跳转, 可以跨周期缩放定位

策略回测时, 提供回测报告/图上标记和对应的回测分析报告.

策略运行和回测信息自动保存, 可事后随时查阅显示

TqSdk配合web_gui使用时, 还支持自定义绘制行情图表, 像这样:

TqSdk 允许在一个策略中使用任意多个数据序列. 回测框架将正确识别并处理这种情况.

关于策略回测的详细说明, 请见 策略程序回测

要学习使用 TqSdk, 推荐从 十分钟快速入门 开始 使用过程中有任何问题可以 询问天勤 AI 助手！ ,尝试帮助解答用户以下问题：

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
class DoubleMaStrategy(CtaTemplate):

  parameters = ["fast_window", "slow_window"]
  variables = ["fast_ma0", "fast_ma1", "slow_ma0", "slow_ma1"]

  def __init__(self, cta_engine, strategy_name, vt_symbol, setting):
    ...

  def on_tick(self, tick: TickData):
    ...

  def on_bar(self, bar: BarData):
    ...
```

Example 2 (python):
```python
'''
双均线策略
'''
from tqsdk import TqApi, TqAuth, TqSim, TargetPosTask
from tqsdk.tafunc import ma

SHORT = 30
LONG = 60
SYMBOL = "SHFE.bu1912"

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

data_length = LONG + 2
klines = api.get_kline_serial(SYMBOL, duration_seconds=60, data_length=data_length)
target_pos = TargetPosTask(api, SYMBOL)

while True:
    api.wait_update()

    if api.is_changing(klines.iloc[-1], "datetime"):  # 产生新k线:重新计算SMA
        short_avg = ma(klines.close, SHORT)  # 短周期
        long_avg = ma(klines.close, LONG)  # 长周期

        # 均线下穿，做空
        if long_avg.iloc[-2] < short_avg.iloc[-2] and long_avg.iloc[-1] > short_avg.iloc[-1]:
            target_pos.set_target_volume(-3)
            print("均线下穿，做空")

        # 均线上穿，做多
        if short_avg.iloc[-2] < long_avg.iloc[-2] and short_avg.iloc[-1] > long_avg.iloc[-1]:
            target_pos.set_target_volume(3)
            print("均线上穿，做多")
```

Example 3 (typescript):
```typescript
'''
价差回归
当近月-远月的价差大于200时做空近月，做多远月
当价差小于150时平仓
'''
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
quote_near = api.get_quote("SHFE.rb1910")
quote_deferred = api.get_quote("SHFE.rb2001")
# 创建 rb1910 的目标持仓 task，该 task 负责调整 rb1910 的仓位到指定的目标仓位
target_pos_near = TargetPosTask(api, "SHFE.rb1910")
# 创建 rb2001 的目标持仓 task，该 task 负责调整 rb2001 的仓位到指定的目标仓位
target_pos_deferred = TargetPosTask(api, "SHFE.rb2001")

while True:
    api.wait_update()
    if api.is_changing(quote_near) or api.is_changing(quote_deferred):
        spread = quote_near.last_price - quote_deferred.last_price
        print("当前价差:", spread)
        if spread > 250:
            print("目标持仓: 空近月，多远月")
            # 设置目标持仓为正数表示多头，负数表示空头，0表示空仓
            target_pos_near.set_target_volume(-1)
            target_pos_deferred.set_target_volume(1)
        elif spread < 200:
            print("目标持仓: 空仓")
            target_pos_near.set_target_volume(0)
            target_pos_deferred.set_target_volume(0)
```

Example 4 (r):
```r
ks = api.get_kline_serial("SHFE.cu1901", 60)
print(ks.iloc[-1])            # <- 最后一根K线
print(ks.close)               # <- 收盘价序列
ks.high - ks.high.shift(1)    # <- 每根K线最高价-前一根K线最高价, 形成一个新序列
```

---


## TqSdk与使用Ctp接口开发策略程序有哪些差别

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/for_ctp_user.html

**Contents:**
- TqSdk与使用Ctp接口开发策略程序有哪些差别
- 系统整体架构
- K线数据与指标计算
- 数据接收和更新

如果您曾经直接使用CTP接口开发过交易策略程序, 目前刚开始接触 TqSdk, 下面的信息将帮助您尽快理解 TqSdk.

CTP接口直接连接到期货公司交易系统, 从期货公司系统获取行情并执行交易指令.

TqSdk 则使用基于网络协作的组件设计. 如下图:

如图所示, 整个系统结构包括这些关键组件:

行情网关 (Open Md Gateway) 负责提供实时行情和历史数据

交易中继网关 (Open Trade Gateway) 负责连接到期货公司交易系统

上面两个网关统一以 Diff 协议对下方提供服务

天勤终端和TqSdk按照Diff协议连接到行情网关和交易中继网关, 实现行情和交易功能

TqSdk 很小, 安装也很方便, 只要简单 pip install tqsdk 即可

官方专门运维行情数据库, 用户可以直接使用, 不需要自己接收和存储数据

交易相关接口被大幅度简化, 不再需要处理CTP接口的复杂回调, 也不需要发起任何查询请求

任何语言只要支持websocket协议, 都可以用来进行策略开发

也有一些不如直接使用CTP接口方便的地方:

由于交易指令经交易网关转发, 用户无法直接指定CTP服务器地址. 用户如果需要连接到官方交易网关不支持的期货公司, 需要自行部署交易网关.

在TqSdk中, K线数据和其它行情数据一样是由行情网关生成并推送的:

用户不再需要维护K线数据库. 用户电脑实时行情中断后, 也不再需要补历史数据

行情服务器生成K线时, 采用了按K线时间严格补全对齐的算法. 这与其它软件有明显区别, 详见 https://www.shinnytech.com/blog/why-our-kline-different/

行情数据只在每次程序运行时通过网络获取, 不在用户硬盘保存. 如果策略研究工作需要大量静态历史数据, 我们推荐使用数据下载工具, 另行下载csv文件使用.

TqSdk中的K线序列采用 pandas.DataFrame 格式. pandas 提供了 非常丰富的数据处理函数 , 使我们可以非常方便的进行数据处理, 例如:

TqSdk 也通过 tqsdk.tafunc 提供了一批行情分析中常用的计算函数, 例如:

Ctp接口按照事件回调模型设计, 使用 CThostFtdcTraderSpi 的 OnXXX 回调函数进行行情数据和回单处理:

TqSdk则不使用事件回调机制. wait_update() 函数设计用来获取任意数据更新, 像这样:

一次 wait_update 可能更新多个实体, 在这种情况下, is_changing() 被用来判断某个实体是否有变更:

TqSdk针对行情数据和交易信息都采用相同的 wait_update/is_changing 方案. 用户需要记住的要点包括:

get_quote, get_kline_serial, insert_order 等业务函数返回的是一个引用(refrence, not value), 它们的值总是在 wait_update 时更新.

用户程序除执行自己业务逻辑外, 需要反复调用 wait_update. 在两次 wait_update 间, 所有数据都不更新

用 insert_order 函数下单, 报单指令实际是在 insert_order 后调用 wait_update 时发出的.

用户程序中需要避免阻塞, 不要使用 sleep 暂停程序

关于 wait_update 机制的详细说明, 请见 策略程序结构

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (r):
```r
ks = api.get_kline_serial("SHFE.cu1901", 60)
print(ks.iloc[-1])            # <- 最后一根K线
print(ks.close)               # <- 收盘价序列
ks.high - ks.high.shift(1)    # <- 每根K线最高价-前一根K线最高价, 形成一个新序列
```

Example 2 (python):
```python
from tqsdk import tafunc
ks = api.get_kline_serial("SHFE.cu1901", 60)
ms = tafunc.max(ks.open, ks.close)           # <- 取每根K线开盘价和收盘价的高者构建一个新序列
median3 = tafunc.median(ks.close, 100)       # <- 求最近100根K线收盘价的中间值
ss = tafunc.std(ks.close, 5)                 # <- 每5根K线的收盘价标准差
```

Example 3 (swift):
```swift
class MySpiHandler
  : public CThostFtdcTraderSpi
{
public:
  ///当客户端与交易后台建立起通信连接时（还未登录前），该方法被调用。
  virtual void OnFrontConnected();

  ///报单通知
  virtual void OnRtnOrder(CThostFtdcOrderField *pOrder);

  ///成交通知
  virtual void OnRtnTrade(CThostFtdcTradeField *pTrade);
}
```

Example 4 (r):
```r
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
x = api.insert_order("SHFE.cu1901", direction="BUY", offset="OPEN", volume=1, limit_price=50000)

while True:
  api.wait_update()       # <- 这个 wait_update 将尝试更新所有数据. 如果没有任何新信息, 程序会阻塞在这一句. 一旦有任意数据被更新, 程序会继续往下执行
  print(x)                # <- 显示委托单最新状态
```

---


## 交易辅助工具

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/targetpostask.html

**Contents:**
- 交易辅助工具

TargetPosTask 是按照目标持仓手数自动调整 账户持仓中某合约的净持仓 的工具, 使用示例如下:

使用 TargetPosTask 时, 需注意以下要点：

TargetPosTask 在 set_target_volume 时并不下单或撤单, 它的下单和撤单动作, 是在之后的每次 wait_update 时执行的. 因此, 需保证 set_target_volume 后还会继续调用wait_update()

为每个合约只创建一个 TargetPosTask 实例. 一旦创建好后, 可以调用任意多次 set_target_volume 函数, 它总是以最后一次 set_target_volume 设定的手数为工作目标。 如:

TargetPosTask 在工作时, 会负责下单和追单, 直至持仓手数达到目标为止.

在将净持仓调整到目标值后, 可能只持有其中一个方向的手数, 也可能同时有多/空头两个方向的持仓(原因有两个: 初始就持有多/空两个方向, 调整持仓时未平完某一方向; 或在调整目标持仓时禁止"平今"或"平昨",然后以开仓操作来调整净持仓).

如果 offset_priority 为默认值"今昨,开", 则: 先平多头今仓, (若平完今仓后未达到目标手数)再平多头昨仓, (若平完昨仓后未达到目标手数)再在空头方向开仓.

如果 offset_priority 为"今开"(即禁止平昨仓), 则: 先平多头今仓, (若平完今仓后未达到目标手数)再在空头方向开仓. (禁止平今仓的"昨开"与此类似)

如果 offset_priority 为"开"(即禁止平仓), 则: 直接在空头方向开仓以达到目标净持仓.

对于上期所和上海能源交易中心合约, 平仓时则直接根据今/昨的手数进行下单. 对于非上期所和能源交易中心: "今仓"和"昨仓" 是服务器按照今/昨仓的定义(本交易日开始时的持仓手数为昨仓, 之后下单的都为今仓)来计算的, 在平仓时, 则根据计算的今昨仓手数进行下单.

如持有大商所某合约并且 offset_priority 为"今开", 而本交易日未下单(在今昨仓的概念上这是"昨仓", 则不进行平仓, 直接用开仓操作来调整净持仓以达到目标.

如需要取消当前 TargetPosTask 任务，请参考 TargetPosTask 高级功能 。

请勿在使用 TargetPosTask 的同时使用 insert_order() 函数, 否则将导致 TargetPosTask 报错或错误下单。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
target_pos = TargetPosTask(api, "SHFE.rb1901")      #创建一个自动调仓工具, 负责调整SHFE.rb1901的持仓
target_pos.set_target_volume(5)                     #要求自动调仓工具将持仓调整到5手
do_something_else()                                 #现在你可以做别的事了, 自动调仓工具将会在后台自动下单/撤单/跟单, 直到持仓手数达到5手为止
```

Example 2 (python):
```python
from tqsdk import TqApi, TqAuth, TargetPosTask

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
# 创建 rb1810 的目标持仓 task，该 task 负责调整 rb1810 的仓位到指定的目标仓位
target_pos_near = TargetPosTask(api, "SHFE.rb1810")
# 创建 rb1901 的目标持仓 task，该 task 负责调整 rb1901 的仓位到指定的目标仓位
target_pos_deferred = TargetPosTask(api, "SHFE.rb1901")

while True:
    api.wait_update()
    if api.is_changing(quote_near) or api.is_changing(quote_deferred):
        spread = quote_near.last_price - quote_deferred.last_price
        print("当前价差:", spread)
        if spread > 200:
            print("目标持仓: 空近月，多远月")
            # 设置目标持仓为正数表示多头，负数表示空头，0表示空仓
            target_pos_near.set_target_volume(-1)
            target_pos_deferred.set_target_volume(1)
        elif spread < 150:
            print("目标持仓: 空仓")
            target_pos_near.set_target_volume(0)
            target_pos_deferred.set_target_volume(0)
```

Example 3 (python):
```python
from tqsdk import TqApi, TqAuth, TargetPosTask

api = TqApi(auth=TqAuth("快期账户", "账户密码"))
target_pos = TargetPosTask(api, "SHFE.rb2001")
# 设定目标净持仓为空头1手
target_pos.set_target_volume(-1)
# 目标净持仓由空头1手改为多头1手
target_pos.set_target_volume(1)

while True:
    # 需在 set_target_volume 后调用 wait_update() 以发出指令
    # 当调整到目标净持仓后, 账户中此合约的净持仓为多头1手
    api.wait_update()
```

---

