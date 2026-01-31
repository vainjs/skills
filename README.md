# Code Skills

A collection of code standards skills for AI coding assistants.

## Quick Start

```bash
pnpx skills add vainjs/skills
```

Or add marketplace to Claude Code:

```bash
/plugin marketplace add vainjs/skills
```

## Available Skills

| Skill                            | Description                        |
| -------------------------------- | ---------------------------------- |
| [react](skills/react/SKILL.md)   | TypeScript/React code standards    |
| [commit](skills/commit/SKILL.md) | Conventional Commits (commitlint)  |

## Project Structure

```
skills/
├── .claude-plugin/          # Plugin registry
└── skills/                  # Skill collection
    ├── react/
    │   └── SKILL.md
    └── commit/
        └── SKILL.md
```

## Contributing

1. Fork the repository
2. Add your skill under `skills/`
3. Update `.claude-plugin/marketplace.json`
4. Submit a pull request

## License

MIT
