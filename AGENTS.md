# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Copilot, etc.) when working with code in this repository.

## Repository Overview

A collection of skills for AI coding agents focused on Chinese quantitative trading development. Skills are packaged instructions and reference documentation that extend agent capabilities for CTP futures/options trading, market data APIs, and algorithmic trading platforms.

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

- **Skill directory**: `kebab-case` (e.g., `ctp-api`, `rice-quant-dev-guide`)
- **SKILL.md**: Always uppercase, always this exact filename
- **References**: `kebab-case.md` (e.g., `trader-api.md`, `getting-started.md`)
- **Zip file**: Must match directory name exactly: `{skill-name}.zip`

### SKILL.md Format

```markdown
---
name: {skill-name}
description: {One sentence describing when to use this skill.}
---

# {Skill Title}

{Brief description of what the skill does.}

## When to Use This Skill

{Trigger conditions and use cases}

## Quick Reference

{Most important information at a glance}

## Reference Files

{Table of reference files with descriptions}

## Working with This Skill

{Workflows and usage patterns}
```

### Best Practices for Context Efficiency

- **Keep SKILL.md under 500 lines** — put detailed reference material in separate files
- **Write specific descriptions** — helps the agent know exactly when to activate the skill
- **Use progressive disclosure** — reference supporting files that get read only when needed

### End-User Installation

**Claude Code:**
```bash
cp -r skills/{skill-name} ~/.claude/skills/
```

**claude.ai:**
Upload the `{skill-name}.zip` file to project knowledge.
