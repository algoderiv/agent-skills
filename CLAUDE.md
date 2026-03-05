# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Copilot, etc.) when working with code in this repository.

## Repository Overview

A collection of skills for AI coding agents focused on Chinese quantitative trading development. Skills are packaged instructions and reference documentation that extend agent capabilities for CTP futures/options trading, market data APIs, algorithmic trading platforms, and WonderTrader/wtpy quantitative trading development.

## Skill Structure

```
skills/
  {skill-name}/           # kebab-case directory name
    SKILL.md              # Required: skill definition
    references/           # Required: supporting documentation
      {topic}.md          # Reference files loaded on demand
    scripts/              # Optional: executable scripts
    assets/               # Optional: static assets
  {skill-name}.zip        # Required: packaged for distribution
```

### Naming Conventions

- **Skill directory**: `kebab-case` (e.g., `ctp-api`, `rice-quant`)
- **SKILL.md**: Always uppercase, always this exact filename
- **References**: `kebab-case.md` (e.g., `trader-api.md`, `getting-started.md`)
- **Zip file**: Must match directory name exactly: `{skill-name}.zip`

### End-User Installation

**Claude Code:**
```bash
cp -r skills/{skill-name} ~/.claude/skills/
```

**claude.ai:**
Upload the `{skill-name}.zip` file to project knowledge.
