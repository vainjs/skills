---
name: code-style
description: Code style and organization conventions for writing, modifying, refactoring, and reviewing code, including frontend components, Web styles, and backend APIs. Use even when style conventions are not explicitly requested. Covers language-agnostic rules and TypeScript/React-specific guidance.
---

## Usage Principles

This skill defines a personal coding style. Read the project's formatting, lint, and build configuration first; explicit project configuration takes precedence in a conflict. Apply this skill's naming, module organization, and coding conventions wherever explicit configuration does not impose a constraint. Reuse existing shared code, and preserve behavior and public interfaces during style refactoring.

## Shared Conventions

- Create files for actual responsibilities, not empty files just to complete a directory structure.
- Keep private code close to its consumers and cross-module reusable code in shared locations. Avoid depending on sibling modules' private implementations.
- Split modules by independent responsibilities and reuse needs, not mechanically by line count.
- Reuse existing definitions for the same data structure. Derive variants by extending existing definitions or selecting fields instead of redeclaring the same fields.
- For API calls, follow the project's existing convention on whether request wrappers return the full response or only `data`. Do not transform the same data in both the wrapper and the caller. This applies to frontend code and backend code that calls other APIs.
- If the project already uses internationalization, manage user-facing copy through its existing internationalization mechanism and reference it through language resources or a module-level copy entry point. Otherwise, centralized copy extraction and separate copy files are not required.
- Document public interfaces with documentation comments, and explain the reason when marking an interface as deprecated.

## References

Read the references for all scenarios involved in the task. Do not load unrelated references.

- For frontend development or Web styling, read [references/frontend.md](references/frontend.md).
- For backend development, read [references/backend.md](references/backend.md).
- For TypeScript, read [references/typescript.md](references/typescript.md); it applies to both frontend and backend code.
- For React components or hooks, read [references/react.md](references/react.md) together with the frontend conventions.
