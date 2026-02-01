---
name: vainjs
description: Scaffold projects with vainjs conventions. Use when initializing npm packages or Chrome extensions.
---

## Supported Templates

| Template | Description                                      |
| -------- | ------------------------------------------------ |
| npm      | TypeScript npm package with rolldown bundler     |
| crx      | Chrome Extension (Manifest V3) with React + Vite |

## Workflow

> Run this skill in the target project directory.

### 1. Generate files

1. Read [references/common.md](references/common.md) for shared config files
2. Read template-specific reference:
   - npm → [references/npm.md](references/npm.md)
   - crx → [references/crx.md](references/crx.md)

### 2. Resolve dependency versions

For dependencies marked `"latest"`, run `npm view <pkg> version` and write as `^<version>`.

### 3. Post-setup

Check if pnpm is installed (`command -v pnpm`). If not, prompt user to install: `npm install -g pnpm`.

```bash
pnpm install
chmod +x .husky/*
```
