---
name: rice-quant-dev-guide
description: RiceQuant RQData Python API - 米筐金融数据Python接口，用于获取中国A股、港股、期货、期权、基金、债券、宏观经济和另类数据。当使用rqdatac编写量化交易和金融分析Python代码时使用此skill。
---

# Rq_Dev Skill

Ricequant rqdata python api - 米筐金融数据python接口，用于获取中国a股、港股、期货、期权、基金、债券、宏观经济和另类数据。当使用rqdatac编写量化交易和金融分析python代码时使用此skill。, generated from official documentation.

## When to Use This Skill

This skill should be triggered when:
- Working with rq_dev
- Asking about rq_dev features or APIs
- Implementing rq_dev solutions
- Debugging rq_dev code
- Learning rq_dev best practices

## Quick Reference

### Common Patterns

*Quick reference patterns will be added as you use the skill.*

### Example Code Patterns

**Example 1** (rust):
```rust
all_instruments(type=None, date=None, market='cn')
```

**Example 2** (rust):
```rust
rqdatac.all_instruments(type=None, market='hk', date=None)
```

**Example 3** (rust):
```rust
get_pit_financials_ex(order_book_ids, fields, start_quarter, end_quarter, date=None, statements='latest', market='cn')
```

**Example 4** (rust):
```rust
futures.get_dominant(underlying_symbol, start_date=None, end_date=None, rule=0, rank=1, market='cn')
```

**Example 5** (json):
```json
[In]
futures.get_dominant('IF', '20160801')
[Out]
date
20160801    IF1608
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **alternative_data.md** - Alternative Data documentation
- **api_reference.md** - Api Reference documentation
- **derivatives.md** - Derivatives documentation
- **fixed_income.md** - Fixed Income documentation
- **funds_indices.md** - Funds Indices documentation
- **getting_started.md** - Getting Started documentation
- **risk_analytics.md** - Risk Analytics documentation
- **stocks.md** - Stocks documentation

Use `view` to read specific reference files when detailed information is needed.

## Working with This Skill

### For Beginners
Start with the getting_started or tutorials reference files for foundational concepts.

### For Specific Features
Use the appropriate category reference file (api, guides, etc.) for detailed information.

### For Code Examples
The quick reference section above contains common patterns extracted from the official docs.

## Resources

### references/
Organized documentation extracted from official sources. These files contain:
- Detailed explanations
- Code examples with language annotations
- Links to original documentation
- Table of contents for quick navigation

### scripts/
Add helper scripts here for common automation tasks.

### assets/
Add templates, boilerplate, or example projects here.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference patterns are extracted from common usage examples in the docs

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information
