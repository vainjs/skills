---
name: commit
description: Generate git commits following Conventional Commits (commitlint) format. Make sure to use this skill whenever the user mentions git commit, commit changes, make a commit, conventional commits, commitlint, or asks to commit any changes — even if they do not explicitly say "skill" or "Conventional Commits". This skill is especially useful when the user says things like "commit this", "commit my changes", "git add and commit", or wants a properly formatted commit message.
---

## Enforcement

**This skill MUST be followed exactly. Steps MUST execute in order. Do not skip, merge, or reorder any step.**

## Format

```
<type>(<scope>): <description>
[optional body]
[optional footer(s)]
```

## Types

`feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

## Rules

- **type**: Required, lowercase, from the list above
- **scope**: Optional. Only include when it clearly adds context (e.g. `feat(ui):` vs `feat:`). Lowercase, kebab-case if multi-word.
- **description**: Required, lowercase start, no period at end, imperative mood (what the change does, not what it did), max 72 chars
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

## Workflow — Execute in Exact Order

1. **MANDATORY** — Run `git config --list | grep -E "^user\."`. Verify user.name and user.email are present and correct. If missing or wrong, **abort and warn the user**. Do not proceed. **FORBIDDEN: Do NOT add `Co-Authored-By` or any AI credentials.**
2. **MANDATORY** — Stage all changes with `git add -A`. Do not skip.
3. **MANDATORY** — Generate the commit message in Conventional Commits format.
4. **MANDATORY** — Output the commit message **on its own line at the END of your response**. Nothing — no explanation, no commentary, no emoji — should follow it:

   feat(auth): add OAuth2 login

5. **MANDATORY** — Wait for user confirmation. Do NOT run `git commit` yourself. If the user confirms, run `git commit` with the staged changes. If the user declines, stop.
