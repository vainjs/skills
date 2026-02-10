---
name: commit
description: Generate git commits following Conventional Commits (commitlint). Use when user wants to commit changes.
---

## Commit Message Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

## Types

| Type       | Description                                      |
| ---------- | ------------------------------------------------ |
| `feat`     | New feature                                      |
| `fix`      | Bug fix                                          |
| `docs`     | Documentation only                               |
| `style`    | Formatting, missing semicolons (no code change)  |
| `refactor` | Code change that neither fixes nor adds features |
| `perf`     | Performance improvement                          |
| `test`     | Adding or correcting tests                       |
| `build`    | Build system or external dependencies            |
| `ci`       | CI configuration files and scripts               |
| `chore`    | Other changes that don't modify src or test      |
| `revert`   | Reverts a previous commit                        |

## Rules

- **type**: Required, lowercase, from list above
- **scope**: Optional, lowercase, describes affected area (e.g., `api`, `auth`, `ui`)
- **description**: Required, lowercase start, no period at end, imperative mood, max 72 chars
- **body**: Optional, wrap at 100 chars, explain "what" and "why" (not "how")
- **footer**: Optional, for breaking changes (`BREAKING CHANGE:`) or issue refs (`Closes #123`)

## Breaking Changes

Add `!` after type/scope or `BREAKING CHANGE:` in footer:

```
feat(api)!: change authentication endpoint response format

BREAKING CHANGE: The /auth endpoint now returns a JWT token instead of session ID.
```

## Examples

```
feat(auth): add OAuth2 login support
fix(ui): resolve button alignment on mobile
docs: update API documentation
refactor(core): simplify error handling logic
chore(deps): upgrade React to v19
```

## Workflow

1. Run `git status` to see all changes; if unrelated changes exist, suggest splitting into separate commits
2. Stage relevant files with `git add <files>` (avoid `git add -A` to prevent accidentally staging secrets or large files)
3. Run `git diff --staged` to review what will be committed
4. Generate commit message following the format above
5. End your response with only the commit message — do NOT execute `git commit`. Example:

   `feat(auth): add OAuth2 login`

   This lets prompt suggestion prefill it for the user to review and edit.
6. On the next turn, execute `git commit -m "<confirmed message>"`
