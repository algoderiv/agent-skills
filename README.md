# Agent Skills

A collection of skills for AI coding agents. Skills are packaged instructions and reference documentation that extend agent capabilities for Chinese quantitative trading development.

Skills follow the [Agent Skills](https://agentskills.io/) format.

## Available Skills

| Skill | Description | Refs | Source |
|-------|-------------|------|--------|
| [ctp-api](skills/ctp-api/) | CTP 6.7.8 综合交易平台 API — 期货期权交易接口、行情订阅、委托执行 | 52 | Official PDF (2466p) |
| [rice-quant](skills/rice-quant/) | 米筐 RQData Python API — A股、港股、期货、期权、基金、债券数据 | 9 | RQData Docs |
| [nautilus-trader](skills/nautilus-trader/) | NautilusTrader 开发者指南 — 源码构建、Rust/Python 集成、测试 | 6 | GitHub Docs |
| [wtpy](skills/wtpy/) | WonderTrader/wtpy 量化交易综合指南 — CTA/HFT/UFT 引擎、策略回测、实盘运维 | 13 | Official + Community |
| [tqsdk](skills/tqsdk/) | 天勤 TqSdk Python 量化交易框架 — 期货/期权/股票策略开发、回测与实盘 | 30 | Official Docs |

### ctp-api

CTP (Comprehensive Transaction Platform) 6.7.8 API documentation for Chinese futures and options trading. Contains 52 reference files extracted from the official 2466-page PDF.

**Use when:**
- Developing with CTP API for futures/options trading
- Implementing market data subscription and processing
- Handling order submission, cancellation, and queries
- Understanding CTP callback mechanisms and event handling
- Debugging CTP connection, authentication, and trading issues

**Categories covered:**
- Trading interface (`CThostFtdcTraderApi`) (Critical)
- Market data interface (`CThostFtdcMdApi`) (Critical)
- Callback handlers (`TraderSpi`, `MdSpi`) (High)
- Data structures and field definitions (High)
- Error codes and troubleshooting (Medium)
- Regulatory monitoring (Medium)

### rice-quant

RiceQuant RQData Python API for accessing Chinese financial market data — A-shares, Hong Kong stocks, futures, options, funds, bonds, and macro data.

**Use when:**
- Fetching Chinese market data with `rqdatac` Python package
- Building quantitative analysis pipelines for Chinese markets
- Accessing derivatives, fixed income, or alternative data
- Working with RiceQuant data feeds in backtesting or live trading

**Categories covered:**
- Getting started and authentication (Critical)
- Stock and equity data (High)
- Derivatives (futures, options) data (High)
- Fixed income and bond data (Medium)
- Fund and index data (Medium)
- Alternative and macro data (Low-Medium)

### nautilus-trader

Developer guide for NautilusTrader, a high-performance algorithmic trading platform with Python and Rust.

**Use when:**
- Building NautilusTrader from source
- Setting up Rust/Python development environment
- Writing and running tests for NautilusTrader
- Contributing to the NautilusTrader project
- Understanding Rust/Python integration via PyO3

**Categories covered:**
- Development environment setup (Critical)
- Building from source (Critical)
- Testing practices and CI (High)
- Rust crate structure and conventions (High)
- Contribution guidelines (Medium)

### wtpy

WonderTrader/wtpy 量化交易开发综合指南。整合官方文档（Read the Docs）、社区学习笔记和非官方整理文档三大来源，覆盖 155 页文档。

**Use when:**
- 使用 wtpy 开发 CTA/SEL/HFT/UFT 策略
- 进行策略回测、仿真交易或实盘交易
- 配置 WonderTrader 引擎和交易/行情接口（CTP/openctp/XTP）
- 处理行情数据（DSB/CSV 转换、数据录制、WtDataHelper）
- 开发自定义执行器（ExtExecuter）或行情解析器（ExtParser）
- 使用 WtMonSvr 监控服务或 WtStudio 工作台
- WonderTrader C++ 核心开发和编译

**Categories covered:**
- Getting started and architecture (Critical)
- Strategy development and API reference (Critical)
- Configuration files (High)
- Data tools and management (High)
- Advanced development — ExtParser, ExtExecuter, C++ (Medium)
- Web console, WtStudio, and operations (Medium)
- FAQ and troubleshooting (Medium)
- Source code analysis (Low)

### tqsdk

天勤量化 TqSdk — 信易科技开发的开源 Python 量化交易框架，基于快期交易及行情服务器，支持期货、期权、股票的行情获取、策略开发、回测与实盘交易。

**Use when:**
- 使用 tqsdk 编写量化交易策略
- 获取期货/期权/股票实时行情、K线、Tick 数据
- 进行策略回测（Tick 级和 K 线级）
- 实盘下单交易（直连 CTP 或快期通道）
- 使用 tqsdk 算法模块、技术指标计算
- 策略图形化界面开发

**Categories covered:**
- Getting started and quick start (Critical)
- TqApi core framework and business objects (Critical)
- Algorithm module and trading strategies (High)
- Technical indicators (tafunc / ta) (High)
- Backtesting and parameter optimization (High)
- CTP direct connection (tqctp) (Medium)
- Risk control module (Medium)
- GUI and IDE integration (Cursor/Trae) (Low)

## Installation

```bash
# Install a single skill
git clone https://github.com/algoderiv/agent-skills.git /tmp/agent-skills
cp -r /tmp/agent-skills/skills/ctp-api ~/.claude/skills/

# Install all skills
cp -r /tmp/agent-skills/skills/*/ ~/.claude/skills/
```

Or upload the `.zip` file from `skills/` to claude.ai project knowledge.

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected.

**Examples:**
```
Help me implement CTP market data subscription with OnRtnDepthMarketData callback
```
```
Fetch CZCE futures daily data using rqdatac
```
```
How do I build NautilusTrader from source with Rust?
```
```
用 wtpy 写一个 DualThrust CTA 策略并回测
```
```
用 tqsdk 订阅螺纹钢主力合约实时行情并下单
```

## Skill Structure

Each skill contains:
- `SKILL.md` - Instructions for the agent
- `references/` - Supporting documentation (loaded on demand)
- `scripts/` - Helper scripts for automation (optional)
- `{skill-name}.zip` - Packaged for claude.ai upload

## Sources

- **CTP API**: Official CTP 6.7.8 PDF documentation (2466 pages, Shanghai Futures Information Technology)
- **RQData**: [RiceQuant RQData](https://www.ricequant.com/doc/rqdata) official API documentation
- **NautilusTrader**: [NautilusTrader](https://github.com/nautechsystems/nautilus_trader) official developer documentation
- **WonderTrader/wtpy**: [Official docs](https://wtdocs.readthedocs.io/zh/latest/), [Learning Notes](https://zzzzhej.github.io/WonderTrader-Learning-Notes/), [Unofficial docs](https://dumengru.github.io/docs_wondertrader/)
- **TqSdk**: [TqSdk 官方文档](https://doc.shinnytech.com/tqsdk/latest/)

## License

MIT
