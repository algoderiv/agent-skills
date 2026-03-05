# Nt_Dev - Other

**Pages:** 7

---

## 打包数据

**URL:** https://nautilus.aloen.to/developer_guide/packaged_data

**Contents:**
- 打包数据
- Libor 利率​
- 短期利率（Short term interest rates）​
- 经济事件（Economic events）​

仓库内部的一些示例数据存放在 tests/test_kit/data 目录下。

1 个月期美元（USD）Libor 利率可以通过下载 CSV 格式的数据来更新，数据来源为 FRED - USD1MTD156N。

请确保在时间范围（time window）选项中选择 Max，以获取完整的历史序列。

同业（interbank）短期利率可通过 OECD 提供的 CSV 数据进行更新，下载页面： OECD Short-term interest rates。

经济事件（economic events）的数据可以从 FXStreet 下载 CSV： FXStreet Economic Calendar。

请确保所选时区为 GMT（Greenwich Mean Time）。

FXStreet 最多只允许筛选 3 个月的时间范围，因此按年度季度下载并手动合并为单个 CSV 文件是常见的做法。 使用日历图标按下面的区间分别筛选并下载：

分别下载每个季度的 CSV 并将它们合并。

---

## Cython

**URL:** https://nautilus.aloen.to/developer_guide/cython

**Contents:**
- Cython
- 什么是 Cython？​
- 函数与方法签名​
- 调试​
  - PyCharm​
  - Cython 官方文档​
  - 调试小贴士​

本文档为在 NautilusTrader 中使用 Cython 的开发者提供建议与实用技巧。 关于 Cython 的语法和约定，详情请参阅官方文档： Cython docs。

Cython 是 Python 的一个超集，可编译为 C 扩展模块，支持可选的静态类型并带来性能优化。NautilusTrader 在其 Python 绑定以及性能关键的组件中广泛使用 Cython。

请确保所有返回 void 或原生 C 类型（例如 bint、int、double）的函数与方法签名中包含 except * 关键字。

这样可以保证 Python 异常不会被静默忽略，而会按预期向上“冒泡”到调用方。

多年来，增强对 Cython 调试支持一直是 PyCharm 用户频繁提出的需求。但就目前情况来看，PyCharm 很可能不会将 Cython 调试作为一等公民来全面支持（参见 JetBrains YouTrack 上的相关 issue）。

更多信息请参见该 issue： YouTrack: PY-9476。

Cython 官方文档中也给出了一些调试建议： Cython — Debugging guide

文档总结的做法通常需要在命令行手动运行一个特殊定制的 gdb，该流程较为繁琐，我们并不推荐将其作为常规调试流程。

在调试像 NautilusTrader 这样复杂的系统时，通过调试器逐步跟踪代码通常非常有帮助。但如果无法直接对 Cython 代码进行交互式调试，下面这些方法可以提高可观察性：

---

## 适配器

**URL:** https://nautilus.aloen.to/developer_guide/adapters

**Contents:**
- 适配器
- 介绍​
- 适配器的结构​
  - Rust 核心（crates/adapters/your_adapter/）​
  - Python 层（nautilus_trader/adapters/your_adapter）​
- 适配器实现步骤（概览）​
- Rust 端的设计模式建议​
- HTTP 客户端设计要点​
  - 解析（Parser）函数​
  - 方法命名与组织​

本开发者指南说明如何为 NautilusTrader 平台开发集成适配器（adapter）。适配器负责连接交易所或数据提供方，将各类原始场馆 API 翻译为 Nautilus 的统一接口与归一化的领域模型。

NautilusTrader 的适配器遵循分层架构，通常包含：

目前几个实现模式一致、可作为参考的适配器有：

Python 层向平台暴露集成接口，通常包含：

为了方便 Python 绑定的高效克隆，HTTP 客户端采用内层/外层（inner/outer）结构，并用 Arc 包裹：

解析函数把场馆特定的数据结构转为 Nautilus 的领域对象。公共转换放在 common/parse.rs，REST 特定的放在 http/parse.rs。每个解析函数接收场馆数据与上下文（账号 ID、时间戳、instrument 引用等），并返回包裹在 Result 中的 Nautilus 类型。

把复用的解析辅助（如 parse_price_with_precision, parse_timestamp）放在相同模块中作为私有函数。

把 HTTP 方法分为两类：低级直接 API 调用和高级领域方法。

在内层 client 中，高级方法通常遵循三步：从 Nautilus 类型构建场馆参数 -> 调用对应的 http_* 低级方法 -> 解析或抽取响应。对于返回领域对象的端点（positions、orders、trades），使用 common/parse 中的解析函数；对于返回原始场馆数据的端点（费率、余额），直接从响应包中抽取结果。以 request_* 前缀的方法表示返回领域数据，而 submit_*、cancel_*、modify_* 等方法用于执行动作并返回确认信息。

外层 client 仅把方法委托给内层，目的仅为通过 Arc 支持 Python 端的廉价克隆。

使用 derive_builder crate 并提供合理的默认值与友好的 Option 处理：

使用 nautilus_network 提供的 RetryManager 实现一致的重试策略。

通过 HttpClient 配置速率限制。

WebSocket 客户端用于实时流数据，需仔细管理连接状态、认证、订阅与重连逻辑。

WebSocket 客户端通常不需要内/外层模式，因为它们不常被克隆。使用单个结构体并清晰地管理状态。

一个订阅（subscription）通常处于两种状态之一：

适配器使用场馆特定的分隔符来构造订阅主题：

用 split_once() 按相应分隔符拆分以提取 channel 与 symbol。

重连时恢复认证与公共订阅，但跳过那些在断开前显式取消的私有通道。

既要支持控制帧（control-frame）级别的 ping，也要支持应用层的 ping。

在将上游架构映射到 Rust 时建议遵循以下约定。

适配器应提供两层测试覆盖：Rust 端与暴露给平台的 Python glue。保证测试套件确定性并与被保护的生产代码紧密放置。

使用 Axum mock server 验证对外公共 API。至少参考 BitMEX 的测试覆盖（见 crates/adapters/bitmex/tests/），确保每个适配器验证相同的行为集。

Login handshake — 断言成功登录会切换内部认证状态，并测试服务器返回非零 code 的失败场景；客户端应抛出错误并避免将自己标记为已认证。

Ping/Pong — 验证文本型与控制帧 ping 都能触发即时 pong。

Subscription lifecycle — 断言 public/private 通道的订阅请求/ack 被正确发送与接收，且取消订阅会从缓存集合中移除条目。

Reconnect behaviour — 模拟断连并验证客户端能重新认证、恢复公共通道，并跳过在断开前已显式取消的私有通道。

Message routing — 向 socket 注入代表性的数据/ack/error 负载，并断言其在 public stream 上以正确的 NautilusWsMessage 变体出现。

Quota tagging — （可选）验证订单/撤单/改单等操作带上合适的配额标签，以便在订阅流量之外对速率限制进行独立管控。

优先使用事件驱动的断言与共享状态（例如收集 subscription_events、跟踪 pending/confirmed 话题、等待 connection_count 的状态变化），而不是任意的 sleep 调用。

使用适配器特有的辅助工具来等待显式信号（如 "auth confirmed" 或 "reconnection finished"），以保持测试在高并发下的确定性。

下面给出使用模板为新数据提供方构建适配器的分步指南。

InstrumentProvider 负责提供场馆可用的 instrument 定义，包括加载全部 instrument、按 ID 加载特定 instrument，以及对 instrument 列表应用过滤器。

LiveDataClient 处理与市场数据非直接相关的数据订阅与管理，例如新闻流、定制数据流或其它能增强策略但不直接代表市场活动的数据源。

MarketDataClient 处理市场相关的数据，如 order book、top-of-book 报价与成交、以及 instrument 的状态更新，关注历史与实时数据供交易所需。

ExecutionClient 负责订单管理，包括提交、修改与撤单。它与场馆交易系统交互以管理并执行交易。

配置文件定义适配器相关设置，例如 API key 与连接信息。此类设置对初始化与管理适配器连接至数据提供方至关重要。

针对适配器说明书中声称支持的每一种场馆行为都应该有测试用例。把下列场景纳入 Rust 与 Python 的测试套件。

**Examples:**

Example 1 (unknown):
```unknown
crates/adapters/your_adapter/├── src/│   ├── common/           # 共享类型与工具函数│   │   ├── consts.rs     # 场馆常量 / broker ID│   │   ├── credential.rs # API key 存储与签名辅助│   │   ├── enums.rs      # 与 REST/WS 负载对应的枚举│   │   ├── urls.rs       # 与环境/产品相关的 base-url 解析器│   │   ├── parse.rs      # 共享解析辅助函数│   │   └── testing.rs    # 单元测试复用的 fixtures│   ├── http/             # HTTP 客户端实现│   │   ├── client.rs     # 带认证的 HTTP 客户端│   │   ├── models.rs     # REST 负载对应的结构体│   │   ├── query.rs      # 请求 / 查询构造器│   │   └── parse.rs      # 响应解析函数│   ├── websocket/        # WebSocket 实现│   │   ├── client.rs     # WebSocket 客户端│   │   ├── messages.rs   # 流式消息的结构体│   │   └── parse.rs      # 消息解析函数│   ├── python/           # PyO3 对 Python 的导出│   ├── config.rs         # 配置结构体│   └── lib.rs            # 库入口└── tests/                # 使用 mock server 的集成测试
```

Example 2 (unknown):
```unknown
nautilus_trader/adapters/your_adapter/├── config.py     # 配置类├── constants.py  # 适配器常量├── data.py       # LiveDataClient / LiveMarketDataClient├── execution.py  # LiveExecutionClient├── factories.py  # instrument factory├── providers.py  # InstrumentProvider└── __init__.py   # 包初始化
```

Example 3 (rust):
```rust
use std::sync::Arc;use nautilus_network::http::HttpClient;// Inner client - contains actual HTTP logicpub struct MyHttpInnerClient {    base_url: String,    client: HttpClient,  // Use nautilus_network::http::HttpClient, not reqwest directly    credential: Option<Credential>,    retry_manager: RetryManager<MyHttpError>,    cancellation_token: CancellationToken,}// Outer client - wraps inner with Arc for cheap cloning (needed for Python)pub struct MyHttpClient {    pub(crate) inner: Arc<MyHttpInnerClient>,}
```

Example 4 (rust):
```rust
use derive_builder::Builder;#[derive(Clone, Debug, Deserialize, Serialize, Builder)]#[serde(rename_all = "camelCase")]#[builder(setter(into, strip_option), default)]pub struct InstrumentsInfoParams {    pub category: ProductType,    #[serde(skip_serializing_if = "Option::is_none")]    pub symbol: Option<String>,    #[serde(skip_serializing_if = "Option::is_none")]    pub limit: Option<u32>,}impl Default for InstrumentsInfoParams {    fn default() -> Self {        Self {            category: ProductType::Linear,            symbol: None,            limit: None,        }    }}
```

---

## 开发者指南

**URL:** https://nautilus.aloen.to/developer_guide

**Contents:**
- 开发者指南
- 目录​

欢迎阅读 NautilusTrader 的开发者指南！

这里汇集了如何开发和扩展 NautilusTrader 以满足你交易需求的说明，以及如何将改进贡献回项目的指引。

本指南的结构同时兼顾了机器化工具和人类读者的阅读需求，便于自动化处理与人工查阅并存。

我们信奉“用对工具做对事”的理念。整体设计思想是尽可能发挥 Python 在高级抽象和丰富生态上的优势，同时通过额外手段弥补其在性能和类型安全（作为解释型动态语言时固有的短板）方面的不足。

Cython 的一个重要优势是：内存的分配与释放可以在构建的“cythonization”步骤中由 C 代码生成器处理（除非你在实现中显式使用了它的低级特性）。

这种做法把 Python 的简洁性与通过编译扩展获得的近原生 C 性能结合了起来。

我们主要的开发与运行环境是 Python。由于生产代码库中大量采用了以 .pyx 与 .pxd 为后缀的 Cython 文件，因此理解 CPython 实现如何与底层 CPython API 交互，以及 Cython 生成的 NautilusTrader C 扩展模块的工作方式，会非常有帮助。

建议仔细阅读 Cython 文档 来熟悉其核心概念，以及在哪些地方使用了 C 类型（C typing）。

你不需要成为 C 语言专家，但理解 Cython 中的 C 语法如何用于函数与方法定义、局部代码块，以及常见原始 C 类型如何映射到对应的 PyObject 类型，会对开发与调试极为有利。

---

## 编码规范

**URL:** https://nautilus.aloen.to/developer_guide/coding_standards

**Contents:**
- 编码规范
- 代码风格​
  - 通用格式规则​
  - 注释约定​
  - 文档注释 / docstring 的语气​
  - 术语与措辞​
  - 代码格式化细则​
  - PEP-8​
- Python 风格指南​
  - 类型注解（Type hints）​

当前代码库本身可作为格式约定的参考。下面补充了一些额外的指南。

下列规范适用于所有源文件（包括 Rust、Python、Cython、shell 等）：

这些约定与各语言生态的主流风格保持一致，能让生成的文档对最终用户更自然友好。

错误信息：避免在错误信息中使用 ", got"。可根据上下文使用更清晰的替代词，如 ", was"、", received" 或 ", found"。

拼写：使用单词 "hardcoded"（连写），而不是 "hard-coded" 或 "hard coded" —— 这是更现代且被更广泛接受的写法。

当代码行较长或传入参数较多时，应换行并对齐到下一个逻辑缩进层级（而非尝试将参数按开括号进行“视觉对齐”）。这种做法能节省右侧空间，使重要代码更居中显示，并且对函数/方法名变更更具鲁棒性。

确保多行挂起的参数或参数列表以尾随逗号结束：

代码库总体遵循 PEP-8 风格指南。即便在 Cython 部分利用了 C 类型，我们仍尽量保持 Python 的惯用写法。 一个显著的例外是：在除集合类型之外的场景中，我们并不总是使用 Python 的 truthiness（真值）来判断参数是否为 None。

Cython 在遇到 is None 和 is not None 时，可以生成更高效的 C 代码，而无需进入 Python 运行时去检查 PyObject 的真值性。

根据 Google Python Style Guide 的建议——当函数或方法可能接收意外类型而导致布尔求值出现异常时，不建议用 truthiness 来判断变量是否为 None，因为这可能引入逻辑错误。

始终使用 if foo is None:（或 is not None）来检查 None 值。例如，当判断一个默认值为 None 的变量或参数是否被设置为其他值时，其他值可能在布尔上下文中被判定为 False。

在非性能关键的代码路径中，出于可读性考虑，if foo is None: 与 if not foo: 的使用仍然是可以接受的。

对于空集合（例如 if not my_list:），应使用 truthiness 检查，而不是显式与 None 或空值比较。

我们欢迎对任何在无明显理由下偏离 PEP-8 的地方提供反馈。

所有函数和方法签名必须包含完整的类型注解：

泛型类型（Generic Types）：对可重用组件使用 TypeVar

代码库统一采用 NumPy docstring 规范。 必须一致遵守该规范以确保文档能正确构建。

测试方法命名：使用描述性名称说明测试场景：

ruff 用于对代码库进行静态检查（lint）。Ruff 的规则位于顶层的 pyproject.toml 中，针对被忽略的规则通常会在注释中给出理由。

提交主题（subject）应限制在 60 字符以内，首字母大写且不以句号结尾。

使用祈使句（imperative voice），即描述应用该提交后将会做什么。

可选：在正文中说明变更，正文与主题之间空一行。正文保持在每行 100 字符以内，可使用带或不带句号的项目符号。

可选：在提交信息中提供相关 issue 或 ticket 的 # 引用。

**Examples:**

Example 1 (python):
```python
long_method_with_many_params(    some_arg1,    some_arg2,    some_arg3,  # <-- trailing comma)
```

Example 2 (python):
```python
def __init__(self, config: EMACrossConfig) -> None:def on_bar(self, bar: Bar) -> None:def on_save(self) -> dict[str, bytes]:def on_load(self, state: dict[str, bytes]) -> None:
```

Example 3 (python):
```python
T = TypeVar("T")class ThrottledEnqueuer(Generic[T]):
```

Example 4 (python):
```python
def test_currency_with_negative_precision_raises_overflow_error(self):def test_sma_with_no_inputs_returns_zero_count(self):def test_sma_with_single_input_returns_expected_value(self):
```

---

## 开发者指南

**URL:** https://nautilus.aloen.to/developer_guide/

**Contents:**
- 开发者指南
- 目录​

欢迎阅读 NautilusTrader 的开发者指南！

这里汇集了如何开发和扩展 NautilusTrader 以满足你交易需求的说明，以及如何将改进贡献回项目的指引。

本指南的结构同时兼顾了机器化工具和人类读者的阅读需求，便于自动化处理与人工查阅并存。

我们信奉“用对工具做对事”的理念。整体设计思想是尽可能发挥 Python 在高级抽象和丰富生态上的优势，同时通过额外手段弥补其在性能和类型安全（作为解释型动态语言时固有的短板）方面的不足。

Cython 的一个重要优势是：内存的分配与释放可以在构建的“cythonization”步骤中由 C 代码生成器处理（除非你在实现中显式使用了它的低级特性）。

这种做法把 Python 的简洁性与通过编译扩展获得的近原生 C 性能结合了起来。

我们主要的开发与运行环境是 Python。由于生产代码库中大量采用了以 .pyx 与 .pxd 为后缀的 Cython 文件，因此理解 CPython 实现如何与底层 CPython API 交互，以及 Cython 生成的 NautilusTrader C 扩展模块的工作方式，会非常有帮助。

建议仔细阅读 Cython 文档 来熟悉其核心概念，以及在哪些地方使用了 C 类型（C typing）。

你不需要成为 C 语言专家，但理解 Cython 中的 C 语法如何用于函数与方法定义、局部代码块，以及常见原始 C 类型如何映射到对应的 PyObject 类型，会对开发与调试极为有利。

---

## 文档风格指南

**URL:** https://nautilus.aloen.to/developer_guide/docs

**Contents:**
- 文档风格指南
- 总体原则​
- 语言与语气​
- Markdown 表格​
  - 列对齐与间距​
  - 注释与说明​
  - 示例（Markdown 表格）​
  - 支持标记​
- 代码引用​
- 标题（Headings）​

本指南列出为 NautilusTrader 撰写文档时建议遵循的样式约定与最佳实践。

我们遵循以可读性与无障碍为优先的现代文档约定：

该约定与业界主流文档风格（如 Google Developer Documentation、Microsoft Docs 等）一致，能提升可读性、降低认知负担，并对国际化用户与屏幕阅读器更友好。

**Examples:**

Example 1 (markdown):
```markdown
| Order Type          | Spot | Margin | USDT Futures | Coin Futures | Notes                   || ------------------- | ---- | ------ | ------------ | ------------ | ----------------------- || `MARKET`            | ✓    | ✓      | ✓            | ✓            |                         || `STOP_MARKET`       | -    | ✓      | ✓            | ✓            | Not supported for Spot. || `MARKET_IF_TOUCHED` | -    | -      | ✓            | ✓            | Futures only.           |
```

Example 2 (markdown):
```markdown
# NautilusTrader Developer Guide## Getting started with Python## Using the Binance adapter## REST API implementation## WebSocket data streaming## Testing with pytest
```

---
