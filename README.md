# Vainjs Skills

A collection of [Agent Skills](https://agentskills.io/what-are-skills) based on personal best practices from real-world projects. Reflects my own coding style — use it if you find it helpful.

## Quick Start

```bash
pnpx skills add vainjs/skills
```

Or add marketplace to Claude Code:

```bash
/plugin marketplace add vainjs/skills

/plugin install code-skills@vainjs-skills
```

## Available Skills

| Skill                            | Description                                 |
| -------------------------------- | ------------------------------------------- |
| [react](skills/react/SKILL.md)   | TypeScript/React code standards             |
| [commit](skills/commit/SKILL.md) | Conventional Commits (commitlint)           |
| [vainjs](skills/vainjs/SKILL.md) | Scaffold npm packages and Chrome extensions |

## Contributing

1. Fork the repository
2. Add your skill under `skills/`
3. Update `.claude-plugin/marketplace.json`
4. Submit a pull request

## License

MIT
