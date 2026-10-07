# TypeScript Conventions

## Formatting

When the project has no formatting configuration, use a 100-column line width, 2-space indentation, single quotes, LF line endings, and no line-ending semicolons. Keep trailing commas wherever valid in multiline structures, including function parameters and calls. Always parenthesize arrow function parameters.

## Naming

- Use camelCase for variables and functions.
- Use PascalCase for types, and UPPER_SNAKE_CASE for enums, enum members, and constants.
- Prefix booleans with `is` / `has` / `can`, and event callbacks with `on`.

## Imports

- Use `import type` for type-only imports. When importing values and types together, combine them with inline `type` modifiers if desired.
- Use named imports for utility functions.

## Types

- Use `strict` as the baseline for new projects; follow the current TypeScript configuration in existing projects.
- Prefer `type` declarations and keep similar declarations consistent.
- Use `T[]` for arrays.
- Prefer explicit types. Use `unknown` for unknown external data and narrow it at the boundary. Limit necessary `any` usage to compatibility boundaries, and use explicit named types for public interfaces.
- Use `T | null`, `Partial<T>`, or similar types for data that has not been fetched or is incomplete. Do not use `as T` to assert such data as a complete object.

## Variables

Prefer `const`; use `let` only when reassignment is needed.

## Comments

Use JSDoc for documentation comments and `@deprecated` for deprecation markers.
