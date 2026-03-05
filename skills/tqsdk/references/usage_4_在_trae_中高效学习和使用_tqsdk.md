# Usage - Part 4

**Topics:** 在 Trae 中高效学习和使用 TqSdk, 账户与交易, 期权基本使用

---

## 在 Trae 中高效学习和使用 TqSdk

**URL:** https://doc.shinnytech.com/tqsdk/latest/ai_editor/tqsdk_trae.html

**Contents:**
- 在 Trae 中高效学习和使用 TqSdk
- Trae：TqSdk 开发与学习的 AI 助手
  - Trae 简介
  - 在 TqSdk 开发中使用 Trae 的好处
- 开始使用 Trae
  - 下载和安装 Trae
  - 初次启动与界面要点
- 在 Trae 中配置 TqSdk 开发环境
  - 创建或打开您的 TqSdk 项目
  - 配置 Python 解释器

Trae 是一款 AI 原生的智能开发环境，旨在将大模型的理解、生成与协作能力深度融入日常编码流程。它支持对话式编程（Chat/Agent）、代码补全与多处位点修改、项目级生成（Builder）、实时预览与调试等能力，帮助开发者在同一环境内完成从构思到实现与迭代的闭环。

对于 TqSdk 用户而言，使用 Trae 进行开发与学习具有以下优势：

面向 TqSdk 的类（如 TqApi, TqAccount）、函数或交易概念（如 KLine, Backtest, target_pos）提出问题，获得结构化解释与示例。

在编辑器内就地查询函数参数、返回值及常见用法。

对话生成：用自然语言描述策略需求，如“订阅多个合约的 tick 并统计成交量阈值报警”，“基于布林带的简易开平仓框架”。

智能补全与批量修改：基于上下文提供更契合的补全，并可对多处代码进行一致性修改与重构建议。

错误分析与定位：粘贴错误堆栈和相关代码，获得原因分析与修复建议。

将 TqSdk 源码加入工作区后，可让 AI 针对具体模块/函数解释实现思路和设计取舍。

项目级上下文感知：结合项目代码结构与依赖，回答更贴合当前工程语境。

访问 Trae 官方下载页面（请根据您组织/渠道提供的地址获取安装包）。

根据您的操作系统（Windows, macOS, Linux）下载对应安装包。

界面区域：文件资源管理器、编辑器区、终端/调试区与 AI 对话/智能体入口。

AI 入口：可通过侧边栏或工具栏打开 Chat/Agent 面板，进行上下文对话与代码指令。

在 Trae 中创建新工程或通过“打开文件夹”导入现有的 TqSdk 项目根目录。

确保 Trae 使用您期望的 Python 解释器/环境来运行 TqSdk 代码。常用做法：

在 Trae 的设置或状态栏中选择目标 Python 解释器（如系统 Python、venv、Conda 环境等）。

或在 Trae 集成终端中激活虚拟环境（例如 venv/Scripts/activate 或 conda activate <env>）。

在 Trae 的集成终端中（确保已选定正确解释器/已激活环境），执行：:

可用以下命令验证安装： python -c "import tqsdk; print(tqsdk.__version__)" 。若提示 pip 未找到，请使用 python -m pip install tqsdk 或检查环境变量设置。

将 TqSdk 源码加入工作区，可显著提升 AI 对实现细节与 API 的理解质量。

方式一（深入研究推荐）：从官方仓库克隆：:

方式二（快速查阅已安装版本）：定位到当前 Python 环境的 site-packages 目录中的 tqsdk 包路径（如 Windows 的 .../Lib/site-packages/tqsdk）。

在已打开的项目中，将上述 tqsdk 源码文件夹添加到工作区（使用“添加文件夹到工作区”或等效入口）。

之后可直接在工作区浏览 TqSdk 源码，AI 对应答与代码生成将更贴近真实实现。

打开 Trae 的 AI Chat/Agent 面板后，您可以：

选中代码后提问：选中一段 TqSdk 代码并发起对话，让 AI 以上下文模式进行解释、优化或缺陷分析。

“TqSdk 中 TqApi 与 TqAccount 的关系与区别是什么？”

“如何获取 SHFE.rb2410 的 1 分钟 K 线？请给完整示例。”

“回测 TqBacktest 的 start_dt 和 end_dt 该如何设置？”

“insert_order 的 limit_price 与 offset 参数如何使用？”

“@tqsdk/trade.py 中 TdApi 的 _on_rsp_order_insert 做了什么？”

“我在看 @tqsdk/tools/downloader.py，该下载器支持哪些数据类型？”

“运行以下代码时报错 ...（附完整堆栈），可能原因是什么？如何修复？”

“写个函数，输入合约列表，批量订阅这些合约的盘口行情 quote。”

“用 TqSim 做模拟交易，当资金变化超过 5% 时发送通知的框架。”

“将这段 TqSdk 代码的 datetime 格式化为 YYYY-MM-DD HH:MM:SS。”

“为下单逻辑增加条件：只有当最新价大于过去 20 周期均线时才开多。”

“重构策略：将行情处理与交易决策拆成独立函数。”

Trae 提供集成调试能力（具体入口与配置以实际版本为准）。

启动调试：在运行/调试面板选择相应 Python 配置（如“Python File”或配置的调试任务）。

AI 辅助调试：调试中若遇到异常或变量状态不明，可将相关片段与变量值粘贴到对话中，请求解释或给出下一步排查建议。

若已加入 TqSdk 源码，指明相关模块或符号位置（例如 @tqsdk/... 风格的文件/符号提示）。

说明版本：如 Python 版本、TqSdk 版本、依赖（pandas/NumPy 等）版本。

共享尝试：说明已尝试方案与结果，便于更精准的建议。

在 AI 对话中说明您的需求与版本信息（如 Python 3.11、pandas 2.2、NumPy 2.x）。

在合适的平台（若支持）启用 Context7 后，配合工作区源码一起提问，获得“规范 + 实现”的双重校对。

在问题末尾添加“use context7”的提示仅在支持的平台/配置生效；在不支持的平台不会生效。

“环境已选但运行用错解释器？”——在设置与集成终端中同时确认：状态栏解释器与终端激活环境需一致。

“AI 回答不贴合代码？”——将 TqSdk 源码加入工作区；提问时引用具体模块或函数；粘贴最小可复现片段。

“网络/镜像问题导致安装失败？”——优先使用内网镜像或 python -m pip 方式；必要时手动下载离线包安装。

Trae 将 AI 深度融入开发流程，对 TqSdk 用户而言，既能加速理解与编写策略，也能在调试与源码学习上提供持续助力。我们建议您将 TqSdk 源码加入工作区，并充分利用对话生成、智能补全与调试能力，配合明确的问题与上下文描述，以获得更高质量、更高效率的开发体验。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
pip install tqsdk
```

Example 2 (unknown):
```unknown
git clone https://github.com/shinnytech/tqsdk-python.git

记下其中的 ``tqsdk`` 源码目录。
```

---


## 账户与交易

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/trade.html

**Contents:**
- 账户与交易
- 快期账户和实盘账户
- 设定实盘交易账户
- 设定快期模拟交易账户
- 设定模拟交易账户
- 获取账户情况
- 交易指令
- TqSdk支持的期货公司列表

在使用 TqSdk 之前，用户需要先注册自己的 快期账户 ，传入快期账户是使用任何 TqSdk 程序的前提

每个快期账户支持最多绑定3个实盘账户，并且快期账户会在用户第一次使用实盘账户时自动进行绑定(自动绑定功能需要 TqSdk 版本> 1.8.3):

如果需要注册快期账户或者修改您的快期账户绑定的实盘账户，请点击 登录用户管理中心 ，登录成功后显示如下

在下方红框处,用户可以自行解绑/绑定实盘账户，其中解绑操作每天限定一次

如果需要让您的快期账户支持更多的实盘账户，可以在购买我们的 天勤量化专业版 后联系工作人员进行额外账户数的购买

TqSdk 要求在创建 TqApi 时指定交易账户。一旦TqApi创建成功，后续所有通过TqApi发出的交易指令均在此账户中进行.

要使用实盘交易账户, 请使用 TqAccount (注：使用前请先 import TqAccount):

TqAccount 的三个参数分别为 <期货公司名>, <用户名> 和 <密码> (期货公司名前需加大写首字母). 目前TqSdk支持的期货公司列表请参见: TqSdk支持的期货公司列表

TqApi 创建成功即代表相应账户已登录成功. 如果在60秒内无法完成登录, 会抛出超时异常, 用户代码可以此判定登录失败:

CTP:不合法登录，这个报错是指期货账户，密码或者席位错误, 天勤默认情况下连接的是CTP主席席位, 需要切换到CTP席位才能使用

CTP:客户端认证失败，这个报错是指需要用户联系期货公司, 将自己的期货账户与天勤中继对应的APPID进行绑定, 具体流程以期货公司要求为准

如果您需要使用快期模拟账户进行测试，只需在创建TqApi时传入一个 TqKq 的实例，同时需要传入快期账户 模拟交易和论坛。

此账户类型与快期 APP 、天勤官网论坛、快期专业版使用相同的模拟账户系统:

如果您需要使用模拟账户进行测试，只需在创建TqApi时传入一个 TqSim 的实例（不填写参数则默认为 TqSim() 模拟账号）:

如果需要使用能保存账户资金及持仓信息的模拟账户，请使用 "快期模拟" 账号, 账户申请及使用方法请参考 模拟交易和论坛 部分内容。

TqApi 提供以下函数来获取交易账户相关信息:

get_account() - 获取账户资金情况

get_position() - 获取持仓情况

以上函数返回的都是dict, 并会在 wait_update 时更新

要在交易账户中发出一个委托单, 使用 insert_order() 函数:

这个函数调用后会立即返回一个指向此委托单的对象引用, 使用方法与dict一致, 内容如下:

与其它所有数据一样, 委托单的信息也会在 api.wait_update() 时被自动更新:

要撤销一个委托单, 使用 cancel_order() 函数:

除 insert_order 和 cancel_order 外, TqSdk 提供了一些更强的交易辅助工具比如 TargetPosTask. 使用这些工具, 可以简化交易逻辑的编码工作.

请点击查看: TqSdk支持的期货公司列表

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
from tqsdk import TqApi, TqAuth
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
```

Example 3 (python):
```python
from tqsdk import TqAccount, TqApi, TqAuth

api = TqApi(TqAccount("H宏源期货", "320102", "123456"), auth=TqAuth("快期账户", "账户密码"))
```

Example 4 (typescript):
```typescript
try:
    api = TqApi(TqAccount("H宏源期货", "320102", "123456"), auth=TqAuth("快期账户", "账户密码"))
except Exception as e:
    print("行情服务连不上, 或者期货公司服务器关了, 或者账号密码错了, 总之就是有问题")
```

---


## 期权基本使用

**URL:** https://doc.shinnytech.com/tqsdk/latest/demo/option_base.html

**Contents:**
- 期权基本使用
- o10 - 获取期权实时行情
- o20 - 查询符合要求的期权
- o30 - 查询平值/虚值/实值期权
- o40 - 计算期权的希腊字母
- o41 - 计算期权隐含波动率和历史波动率
- o60 - 获取期权波动率曲面
- o70 - 期权套利策略
- o71 - 获取一组期权和其对应行权价
- o72 - 查询标的对应期权按虚值平值实值分类方法一

o41 - 计算期权隐含波动率和历史波动率

o72 - 查询标的对应期权按虚值平值实值分类方法一

o73 - 查询标的对应期权按虚值平值实值分类方法二

o74 - 本地计算ETF期权卖方开仓保证金

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'ringo'

from tqsdk import TqApi, TqAuth

# 创建API实例,传入自己的快期账户
api = TqApi(auth=TqAuth("快期账户", "账户密码"))

# 获取大商所豆粕期权行情
quote_m = api.get_quote("DCE.m1807-C-2450")

# 获取中金所股指期权行情
quote_IO = api.get_quote("CFFEX.IO2002-C-3550")

# 输出 m1807-C-2450 的最新行情时间和最新价
print(quote_m.datetime, quote_m.last_price)

# 关闭api,释放资源
api.close()
```

Example 2 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'ringo'

from tqsdk import TqApi, TqAuth

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

ls = api.query_options("SHFE.au2012")
print(ls)  # 标的为 "SHFE.au2012" 的所有期权

ls = api.query_options("SHFE.au2012", option_class="PUT")
print(ls)  # 标的为 "SHFE.au2012" 的看跌期权

ls = api.query_options("SHFE.au2012", option_class="PUT", expired=False)
print(ls)  # 标的为 "SHFE.au2012" 的看跌期权, 未下市的

ls = api.query_options("SHFE.au2012", strike_price=340)
print(ls)  # 标的为 "SHFE.au2012" 、行权价为 340 的期权

ls = api.query_options("SSE.000300", exchange_id="CFFEX")
print(ls)  # 中金所沪深300股指期权

ls = api.query_options("SSE.510300", exchange_id="SSE")
print(ls)  # 上交所沪深300etf期权

ls = api.query_options("SSE.510300", exchange_id="SSE", exercise_year=2020, exercise_month=12)
print(ls)  # 上交所沪深300etf期权, 限制条件 2020 年 12 月份行权

api.close()
```

Example 3 (python):
```python
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'ringo'

from tqsdk import TqApi, TqAuth

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

quote = api.get_quote("SHFE.au2012")

# 预计输出的为以au2012现在最新价来比对的认购的平值期权，当没有符合的平值期权时返回为空,如果有返回则格式为 ["SHFE.au2012C30000"]
ls = api.query_atm_options("SHFE.au2012", quote.last_price, 0, "CALL")

# 预计输出的为au2012，以开盘价来比对的认购的实值3档，实值2档，实值1档期权，如果没有符合要求的期权则对应栏返回为None，如果有则返回格式例如 [None,None,"SHFE.au2012C30000"]
ls = api.query_atm_options("SHFE.au2012", quote.open, [3, 2, 1], "CALL")

# 预计输出的为au2012，以开盘价来比对的认购的实值1档，平值期权，虚值1档，如果没有符合要求的期权则对应栏返回为None，如果有则返回格式例如
ls = api.query_atm_options("SHFE.au2012", quote.open, [1, 0, -1], "CALL")

# 预计输出的为au2012，以现在最新价来比对的认购的虚值1档期权
ls = api.query_atm_options("SHFE.au2012", quote.last_price, -1, "CALL")

# 预计输出沪深300股指期权,2020年12月的虚值1档期权
ls = api.query_atm_options("SSE.000300", quote.last_price, -1, "CALL", exercise_year=2020, exercise_month=12)

# 预计输出 上交所 沪深300股指ETF期权,2020年12月的虚值1档期权
ls = api.query_atm_options("SSE.510300", quote.last_price, -1, "CALL", exercise_year=2020, exercise_month=12)

api.close()
```

Example 4 (swift):
```swift
#!/usr/bin/env python
#  -*- coding: utf-8 -*-
__author__ = 'ringo'

from tqsdk import TqApi, TqAuth
from tqsdk.ta import OPTION_GREEKS

api = TqApi(auth=TqAuth("快期账户", "账户密码"))

# 获取指定期权行情
quote = api.get_quote("SHFE.cu2006C44000")

# 获取期权和其对应标的的多合约的 kline 数据
klines = api.get_kline_serial(["SHFE.cu2006C44000", "SHFE.cu2006"], 24 * 60 * 60, 30)

# 运行 OPTION_GREEKS 希腊值计算函数
greeks = OPTION_GREEKS(klines, quote, 0.025)

# 输出希腊字母
print(list(greeks["delta"]))
print(list(greeks["theta"]))
print(list(greeks["gamma"]))
print(list(greeks["vega"]))
print(list(greeks["rho"]))

api.close()
```

---

