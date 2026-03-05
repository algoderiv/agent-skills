# Usage - Part 5

**Topics:** 在 Cursor 中高效学习和使用 TqSdk, 快期账户, TqSdk 多策略使用手册, 期权交易 & 交易所官方组合, 在 Jupyter Notebook 中使用 TqSdk

---

## 在 Cursor 中高效学习和使用 TqSdk

**URL:** https://doc.shinnytech.com/tqsdk/latest/ai_editor/tqsdk_cursor.html

**Contents:**
- 在 Cursor 中高效学习和使用 TqSdk
- 概述
- Cursor：TqSdk 开发与学习的 AI 助手
  - Cursor 简介
  - 在 TqSdk 开发中使用 Cursor 的好处
- 开始使用 Cursor
  - 下载和安装 Cursor
  - 初次启动与配置
- 在 Cursor 中配置 TqSdk 开发环境
  - 创建或打开您的 TqSdk 项目

本指南旨在帮助 TqSdk 用户了解如何在 AI 代码编辑器 Cursor 中高效学习和使用 TqSdk 库。Cursor 通过集成强大的 AI 能力，可以显著提升您在 TqSdk 上的开发效率和学习体验。我们将引导您完成 Cursor 的下载、基本配置，并重点介绍如何结合 TqSdk 进行代码编写、问题解答和学习探索。

Cursor 是一款以 AI 为核心的代码编辑器，基于 VS Code 构建。它集成了先进的语言模型（如 GPT-4，claude-3.7 等），允许开发者通过自然语言与 AI 助手交互，实现代码生成、修改、解释、调试等功能，极大地提升编程效率。

对于 TqSdk 用户而言，在 Cursor 中进行开发和学习具有以下显著优势：

对 TqSdk 中的特定类（如 TqApi, TqAccount）、函数或交易概念（如 KLine, Backtest, target_pos）有疑问时，可直接向 AI 提问获得清晰解释。

快速查询 TqSdk 函数的参数、返回值及用法示例。

智能代码生成：通过自然语言描述需求，如"使用 TqSdk 写一个订阅多个期货合约 tick 数据的脚本"、"生成一个基于布林带指标的简单交易策略框架"。

代码自动补全与优化：AI 根据上下文提供更精准的代码补全，并能辅助优化现有的 TqSdk 代码逻辑，提升代码质量和性能。

错误分析与定位：当 TqSdk 代码运行出错时，可将错误信息和相关代码提供给 AI，它能帮助分析错误原因并给出修复建议。

通过将 TqSdk 源码加入工作区，您可以让 AI 解释特定模块或函数的内部实现逻辑，加深对 TqSdk 底层的理解。

贴合项目的上下文感知：Cursor 能够理解您当前项目中的代码结构和内容，使得 AI 的回答和代码建议更加贴合您的实际开发场景。

根据您的操作系统（Windows, macOS, Linux）下载对应的安装包。

登录/账户：首次启动 Cursor，它可能会提示您登录。您可以使用 GitHub 账户登录，或根据提示配置注册账户。Cursor 也提供免费的使用额度，为了您更好的使用我们推荐购买他们的会员，支持支付宝付费。

界面熟悉：如果您熟悉 VS Code，Cursor 的界面将非常易于上手。主要交互区域包括编辑器、侧边栏（文件浏览器等）、底部状态栏和 AI 聊天窗口（默认快捷键 Ctrl + K 或 Cmd + K）。

在 Cursor 中，通过 File > Open Folder... 打开您现有的 TqSdk 项目文件夹，或者创建一个新的空文件夹作为项目根目录。

确保 Cursor 使用了您期望的 Python 环境来运行 TqSdk 代码。

打开终端：在 Cursor 中使用 Terminal > New Terminal (或 Ctrl + `) 打开集成终端。

选择解释器：按 Ctrl + Shift + P (或 Cmd + Shift + P) 打开命令面板，输入 Python: Select Interpreter，然后选择您系统中已安装的、希望用于 TqSdk 开发的 Python 解释器。这可能是系统全局的 Python，也可能是您通过其他方式（如 Conda）管理的特定环境。

在 Cursor 的集成终端中（确保该终端使用的是您选定的 Python 解释器环境），执行：:

您可以通过 python -c "import tqsdk; print(tqsdk.__version__)" 来验证安装。如果提示 pip 命令未找到，请确保您的 Python 解释器路径已正确添加到系统环境变量中，或者使用 python -m pip install tqsdk。

为了让 Cursor 的 AI 能够最深入、最准确地理解 TqSdk 的内部实现和 API 细节，从而提供最高质量的辅助，我们 强烈建议您将 TqSdk 的源码直接添加到 Cursor 的工作区中。

选项一 (推荐用于深入研究)：从 GitHub 克隆 TqSdk 的官方仓库：:

选项二 (适用于快速查阅已安装版本)：找到您当前 Python 环境中 TqSdk 库的安装路径。这通常位于 Python site-packages 目录下 (例如 .../Lib/site-packages/tqsdk (Windows) 或 .../lib/pythonX.Y/site-packages/tqsdk (macOS/Linux))。

首先，在 Cursor 中打开您的 TqSdk 项目文件夹。

然后，在 Cursor 菜单栏中选择 File > Add Folder to Workspace...。

浏览并选择您在上一步中获取的 TqSdk 源码文件夹 (例如，克隆的 tqsdk 目录或 site-packages 下的 tqsdk 目录)。

添加后，Cursor 的文件浏览器侧边栏将同时显示您的项目文件和 TqSdk 的完整源码。

精准的上下文理解 ：Cursor 的 AI 可以直接分析 TqSdk 的每一个函数、类和模块的实现细节。

强大的 @ 符号引用：在与 AI 聊天时（通过 Ctrl/Cmd + K），您可以使用 @ 符号来引用 TqSdk 源码中的具体文件（如 @tqsdk/api.py）或符号（如 @TqApi）。这使得 AI 能够针对特定代码块提供解释、建议或生成相关代码。

更高质量的代码生成与补全 ：AI 生成的代码会更贴合 TqSdk 的实际 API 和最佳实践。

深入学习 TqSdk 内部机制 ：您可以选中源码中的任何部分，让 AI 解释其功能和设计思路。

按下 Ctrl + K (Windows/Linux) 或 Cmd + K (macOS) 会打开 AI 聊天界面。

直接提问：输入您关于 TqSdk 的任何问题。

选中代码后提问：在编辑器中选中一段 TqSdk 代码，然后按 Ctrl/Cmd + K，AI 会以上下文模式启动，您可以针对这段代码提问，如"解释这段代码"、"优化这段代码"或"这段代码有什么潜在问题？"。

"TqSdk 中 TqApi 和 TqAccount 有什么区别和联系？"

"如何使用 TqSdk 获取 SHFE.rb2410 的1分钟 K 线数据？请给出完整代码。"

"解释 TqSdk 回测时 TqBacktest 的 start_dt 和 end_dt 参数。"

"TqSdk 中 insert_order 函数的 limit_price 和 offset 参数如何使用？"

结合源码提问 (假设 TqSdk 源码已加入工作区)：

"@tqsdk/trade.py TdApi 类中的 _on_rsp_order_insert 方法是做什么的？"

"我正在看 @tqsdk/tools/downloader.py，这个下载器支持哪些数据类型？"

"我运行这段 TqSdk 代码 [粘贴您的代码] 时，遇到了这个错误 xxx [粘贴完整错误信息]，可能是什么原因？如何修复？"

"用 TqSdk 写一个函数，输入合约列表，批量订阅这些合约的盘口行情 quote。"

"帮我生成一个使用 TqSdk TqSim 进行模拟交易，并在账户资金变化超过5%时发送通知的基本框架。"

"将这段 TqSdk 代码中的 datetime 对象格式化为 YYYY-MM-DD HH:MM:SS 字符串。"

"为这段 TqSdk 下单逻辑增加一个条件：只有当最新价大于过去20周期均线时才开多仓。"

"重构这个 TqSdk 策略，将行情处理和交易决策部分分离成独立的函数。"

Cursor 继承了 VS Code 强大的调试功能。

启动调试：打开"运行和调试"侧边栏，点击 "Run and Debug" 并选择 "Python File"。

AI 辅助调试：在调试过程中，如果遇到难以理解的行为或变量状态，可以将相关代码和变量值复制到 AI 聊天窗口，寻求解释或建议。

为了从 Cursor AI 获得最准确、最有用的回答，请尝试以下技巧：

遇到错误时，提供完整的错误信息和复现步骤。

使用 @ 符号将 AI 的注意力引导到工作区内的特定 TqSdk 源码文件或符号。

逐步深入：复杂问题可以分解成几个小问题分步提问。

说明 TqSdk 版本 (如果适用)：某些特性可能与版本相关。

分享您的尝试：如果您已尝试过某些解决方案，告知 AI，这有助于获得更精准的指导。

迭代与追问：AI 的首次回答可能并非完美，您可以基于其回答进行追问或要求澄清。

在使用cursor时，还可以结合 context7 来增强文档上下文，从而减少幻觉，让回答更贴近实际可用的 API 与最佳实践。

Context7 是一款基于 MCP（Model Context Protocol）的“文档与示例检索”工具，由 Upstash 提供。启用后，它会在回答问题前自动检索权威、最新且可指定版本的官方文档与示例代码，并把结果注入到 AI 的上下文中，从而显著减少“幻觉”，让回答更贴近实际可用的 API 与最佳实践。

Node.js >= v18.0.0（必需）

建议通过 node -v 确认本机 Node.js 版本，未满足时请先升级至 18 或更高版本。

访问 Context7 Github 地址

在下方找到 Install in Cursor，点击 Add to Cursor 按钮

此时会跳转到 Cursor 界面，点击 Install 即安装成功

打开聊天（Ctrl + K / Cmd + K）。

在说明完你的需求后，在提示末尾添加：`use context7`。

结合本仓库源码一起引用更佳：在问题中同时描述你的代码片段或通过 @tqsdk/... 指向相关模块，获得“实现 + 规范”的联合校对与建议。

“请用 TqSdk 获取 SHFE.rb 主力连续的 1 分钟 K 线，并用 pandas 2.2 计算 20/60 均线；代码需兼容 Python 3.11。use context7”

降低幻觉：基于权威、最新、可指定版本的文档进行回答，显著减少错误建议。

效率更高：无需频繁切换浏览器查文档，回答可带来源依据，验证成本更低。

与源码联动：将外部文档与本仓库源码一起纳入上下文，得到“规范 + 实现”的双重视角。

明确版本：在提示中写出你正在使用的 Python 与关键依赖版本（如 pandas 2.2、NumPy 2.x）。

问题要聚焦：一次只问一个核心问题，必要时拆分为多轮对话，逐步演进。

最小上下文：提供足够但不冗余的代码与背景，避免信息噪声干扰答案质量。

与源码同引用：当官方文档与实现存在差异时，请在问题中同时 @ 源码位置，并让 AI 标注差异与原因。

“添加后没有生效？”——请在 Cursor 的 MCP 面板确认 context7 为 Running；若未运行，检查 Node.js 版本与网络代理；重启 Cursor 再试。

“公司网络限制 npx？”——可预先外网安装依赖，或与网络/安全同事沟通放行 @upstash/context7-mcp 的获取；必要时使用私有镜像源。

“如何停用？”——移除 MCP 配置或在 MCP 面板禁用 context7，重启 Cursor。

Cursor 为 TqSdk 用户提供了一个利用 AI 提升开发和学习效率的强大工具。通过其智能的代码辅助、问答和调试功能，您可以更快地掌握 TqSdk 的使用技巧，更高效地构建和优化您的量化交易策略。

我们鼓励您将 TqSdk 源码加入 Cursor 工作区，并积极使用 AI 聊天功能进行提问和探索。这将帮助您充分发挥 Cursor 的潜力，让 TqSdk 的使用体验更上一层楼。

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
pip install tqsdk
```

Example 2 (unknown):
```unknown
git clone https://github.com/shinnytech/tqsdk-python.git

将克隆下来的 `tqsdk` 文件夹备用。
```

---


## 快期账户

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/shinny_account.html

**Contents:**
- 快期账户
- 用快期账户来模拟交易
- 用快期账户来实盘交易
- 登录用户管理中心

在使用 TqSdk 之前，用户需要先注册自己的 快期账户 ，传入快期账户是使用任何 TqSdk 程序的前提

注册完成的快期账户的【手机号】/【邮箱地址】/【用户名】和【密码】可以作为 快期模拟 账号，通过 TqKq 对 auth 传入参数进行登录，这个 快期模拟 账户在快期APP、快期专业版 和天勤量化上是互通的:

快期账户会在用户使用实盘账户时自动进行绑定，直到该快期账户没有能绑定实盘账户的名额:

如果需要注册快期账户或者修改您的快期账户绑定的实盘账户请参见 登录用户管理中心

点击 登录用户管理中心 ，可以注册快期账户或者修改您的快期账户绑定的实盘账户

登录成功后显示如下，在下方红框处,用户可以自行解绑/绑定实盘账户，其中解绑操作每天限定一次

如需一个快期账户支持更多的实盘账户，请联系工作人员进行批量购买

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (python):
```python
from tqsdk import TqApi, TqAuth
api = TqApi(auth=TqAuth("快期账户", "账户密码"))
```

Example 2 (python):
```python
from tqsdk import TqApi, TqAuth, TqKq
api = TqApi(TqKq(), auth=TqAuth("快期账户", "账户密码"))
```

Example 3 (python):
```python
from tqsdk import TqApi, TqAccount, TqAuth, TqKq
api = TqApi(TqAccount("H宏源期货", "320102", "123456"), auth=TqAuth("快期账户", "账户密码"))
```

---


## TqSdk 多策略使用手册

**URL:** https://doc.shinnytech.com/tqsdk/latest/advanced/tq_trading_unit.html

**Contents:**
- TqSdk 多策略使用手册
- 概述
- 系统配置要求
- 安装
- 使用流程
- 异常委托分配
- 快期专业版里面登录多策略（前端账户）
- 其他事项
- 版本变更

随着对收益曲线稳定的追求，较多用户需要在一个账户下去运行多个策略，当多个策略交易同一标的时，则面临着不同策略的持仓管理，绩效归因等问题

为了解决该问题, TqSdk 企业版 中提供了本地众期多策略系统，支持用户在本地将一个实盘账户拆分为多个策略（多个前端账户），每个策略交易数据相互隔离，且跨日有效

同时该方案会提供多策略的管理界面，支持可视化观察各个多策略的持仓，委托，资金和盈亏情况，也支持以策略维度（前端账户）登录 快期专业版 进行管理与交易

Windows: >= Windows 10

Linux: >= Ubuntu 22.04

使用本地众期多策略系统需要安装 tqsdk-zq 包，用来初始化本地环境:

对于 Windows 用户，还需要安装 Microsoft Visual C++ Redistributable，下载地址： Microsoft Redistributable

kq-name 是快期账户，kq-password 是快期密码，web-port 是可选项，不用指定

初始化完成后，控制台会输出多策略管理页的账户、密码以及网址，默认账户密码均为 admin,

后续跨交易日，重启电脑，不用再执行初始化步骤，直接运行 python 代码即可

如果机器重启或者网页打不开了，请执行以上命令

web 管理端在跨交易日后，需要输入 tqsdk-zq web 拉起

在策略组中，选择账户及交易模式，然后点击右侧的+号，添加后端账号（实盘账户或simnow账户）

继续点击+号添加前端账户（多策略）并设置入金金额

如果后端账户有持仓，则需要在资金监控和强平强撤页的持仓明细来分配各个前端账户的持仓

使用 TqTradingUnit 登录前端账户:

当使用众期多策略进行交易后，系统默认该后端账户下的所有交易指令，均由前端账户发出，当后端账户发出了某笔委托而未指定前端账户时，则会被归类为异常委托范围

当系统存在异常委托时，会每隔固定时间，向前端账户推送通知，提醒用户将异常委托进行分配到具体前端账户，如果本交易日未进行分配，会导致下一个交易日众期系统初始化失败

在下一个交易日如果由于该原因初始化失败后，前端账户的持仓将都会清空，用户需要重新从后端账户往前端账户分配持仓

在备注项设置站点名称，服务器地址项设置本地交易地址，本地交易地址为 ws://localhost ，端口号在用户不设置的情况下默认为 8342

当用户在众期多策略系统中添加实盘账户时，仍然占用企业版3个实盘账户的名额，如果需要额外购买实盘账户名额请联系我们的商务同事 QQ:1539404802

多策略的拆分和运行均在本地，不支持跨电脑使用多策略功能

如果需要删除前端账户，需要先清除持仓（平仓，或者通过持仓划转），然后将出入金转出，才可以删除

使用 pip install -U --upgrade-strategy eager tqsdk-zq 更新多策略功能所有依赖包

修复: 和本地其他 postgres 进程端口冲突的问题

修复: 风控规则高级模式没有正确传递 enable 参数的问题

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
pip install tqsdk-zq
```

Example 2 (unknown):
```unknown
tqsdk-zq init --kq-name=xx --kq-password=xx --web-port=xx
```

Example 3 (unknown):
```unknown
tqsdk-zq web
```

Example 4 (python):
```python
from tqsdk import TqApi, TqTradingUnit, TqAuth

# 在天勤中登录前端账户时，只用输入账户名即可，不用输入前端账户密码
account = TqTradingUnit(account_id="前端账户号")
api = TqApi(account, auth=TqAuth("快期账户", "账户密码"))
```

---


## 期权交易 & 交易所官方组合

**URL:** https://doc.shinnytech.com/tqsdk/latest/usage/option_trade.html

**Contents:**
- 期权交易 & 交易所官方组合
- 期权指标计算&序列计算函数
- 期权查询函数

TqSdk 中期权合和交易所官方组合的约代码格式参考如下:

对于交易所官方组合，目前 TqSdk 中只支持交易所官方组合进行实盘交易，不支持在模拟中进行交易所官方组合交易

TqSdk 内提供了丰富的期权指标计算&序列计算函数，参考如下：

OPTION_GREEKS() - 计算期权希腊指标

OPTION_IMPV() - 计算期权隐含波动率

BS_VALUE() - 计算期权 BS 模型理论价格

OPTION_VALUE() - 计算期权内在价值，期权时间价值

get_bs_price() - 计算期权 BS 模型理论价格

get_delta() - 计算期权希腊指标 delta 值

get_gamma() - 计算期权希腊指标 gamma 值

get_rho() - 计算期权希腊指标 rho 值

get_theta() - 计算期权希腊指标 theta 值

get_vega() - 计算期权希腊指标 vega 值

get_his_volatility() - 计算某个合约的历史波动率

get_t() - 计算 K 线序列对应的年化到期时间，主要用于计算期权相关希腊指标时，需要得到计算出序列对应的年化到期时间

TqSdk 内提供了完善的期权查询函数 query_options() 和对应平值虚值期权查询函数 query_atm_options() ，供用户搜索符合自己需求的期权:

© 版权所有 2018-2026, TianQin。

**Examples:**

Example 1 (unknown):
```unknown
DCE.m1807-C-2450 - 大商所豆粕期权
CZCE.CF003C11000 - 郑商所棉花期权
SHFE.au2004C308 - 上期所黄金期权
CFFEX.IO2002-C-3550 - 中金所沪深300股指期权
SSE.10002513 - 上交所上证50etf期权
SSE.10002504 - 上交所沪深300etf期权
SZSE.90000097 - 深交所沪深300etf期权
CZCE.SPD SR901&SR903 - 郑商所 SR901&SR903 跨期合约
DCE.SP a1709&a1801 - 大商所 a1709&a1801 跨期合约
```

