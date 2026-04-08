---
name: commit
description: Generate git commits following Conventional Commits (commitlint) format. Make sure to use this skill whenever the user mentions git commit, commit changes, make a commit, conventional commits, commitlint, or asks to commit any changes — even if they don't explicitly say "skill" or "Conventional Commits". This skill is especially useful when the user says things like "commit this", "commit my changes", "git add and commit", or wants a properly formatted commit message.
---

## Format

```
<type>(<scope>): <description>
[optional body]
[optional footer(s)]
```

## Types

`feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

## Rules

- **type**: Required, lowercase, from list above
- **scope**: Optional. Only include when it clearly adds context (e.g., `feat(ui):` vs `feat:`). Use lowercase, kebab-case if multi-word.
- **description**: Required, lowercase start, no period at end, imperative mood (say what the change does, not what it did), max 72 chars
- **body**: Optional, wrap at 100 chars, explain "what" and "why" (not "how")
- **footer**: Optional, for breaking changes (`BREAKING CHANGE:`) or issue refs (`Closes #123`, `Fixes #456`)

## Breaking Changes

Append `!` to type/scope, or add `BREAKING CHANGE:` footer.

## Examples

```
feat(auth): add OAuth2 login support
fix(ui): resolve button alignment on mobile
docs: update API documentation
refactor(auth): simplify error handling logic
chore(deps): upgrade React to v19
```

## Workflow

1. Run `git status` to see all changes; if unrelated changes exist, suggest splitting into separate commits
2. Stage relevant files with `git add <files>` (avoid `git add -A` to prevent accidentally staging secrets or large files)
3. Run `git diff --staged` to review what will be committed
4. Generate commit message following the format above
5. Output the commit message on its own line at the END of your response — nothing should follow it:

   feat(auth): add OAuth2 login

6. Wait for user confirmation or prompt suggestion prefill. Do NOT run `git commit` yourself.
