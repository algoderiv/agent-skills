# Agent Skills

A collection of skills for AI coding agents. Skills are packaged instructions and reference documentation that extend agent capabilities for Chinese quantitative trading development.

Skills follow the [Agent Skills](https://agentskills.io/) format.

## Available Skills

### ctp-api

CTP (Comprehensive Transaction Platform) 6.7.8 API documentation for Chinese futures and options trading. Contains 53 reference files extracted from the official 2466-page PDF.

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

### rice-quant-dev-guide

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

### nautilus-trader-dev-guide

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

## Skill Structure

Each skill contains:
- `SKILL.md` - Instructions for the agent
- `references/` - Supporting documentation (loaded on demand)
- `scripts/` - Helper scripts for automation (optional)

## Sources

- **CTP API**: Official CTP 6.7.8 PDF documentation (2466 pages, Shanghai Futures Information Technology)
- **RQData**: [RiceQuant RQData](https://www.ricequant.com/doc/rqdata) official API documentation
- **NautilusTrader**: [NautilusTrader](https://github.com/nautechsystems/nautilus_trader) official developer documentation

## License

MIT
