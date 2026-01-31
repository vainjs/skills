# CLAUDE.md

Code standards skills for AI coding assistants. Skills are distributed via `.claude-plugin/marketplace.json`.

## Structure

```
.claude-plugin/marketplace.json   # Plugin registry
skills/<name>/SKILL.md            # Skill file (YAML frontmatter + Markdown body)
```

## Adding a New Skill

1. Create `skills/<name>/SKILL.md` with `name` and `description` in frontmatter
2. Update `.claude-plugin/marketplace.json` — add skill path to `skills` array
3. Update `README.md` — add skill to Available Skills table
