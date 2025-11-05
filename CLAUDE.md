# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A code standards skills collection for AI coding assistants (Claude Code, Cursor, Windsurf). Skills are distributed as a plugin via `.claude-plugin/marketplace.json`.

## Architecture

```
.claude-plugin/marketplace.json   # Plugin registry — defines plugins and their skill paths
skills/<name>/SKILL.md            # Each skill is a single SKILL.md with YAML frontmatter
```

### Skill Format

Each skill is a `SKILL.md` file with:

- **YAML frontmatter**: `name` and `description` (used for triggering)
- **Markdown body**: Baseline standards and instructions (loaded after trigger)

### Adding a New Skill

1. Create `skills/<name>/SKILL.md` with frontmatter and body
2. Add the skill path to the `skills` array in `.claude-plugin/marketplace.json`

## Key Files

- `.claude-plugin/marketplace.json` — plugin registry, must stay in sync with skill directories
- `skills/react/SKILL.md` — React/TypeScript code standards skill
