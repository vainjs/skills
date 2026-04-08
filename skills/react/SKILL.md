---
name: react
description: TypeScript/React code standards for writing and reviewing React components, hooks, and TypeScript code. Make sure to use this skill whenever the user mentions React, React component, functional component, hook (useState, useEffect, useCallback, useMemo, custom hook), TypeScript type/interface, FC, Props, state management, or asks to write, review, or refactor any React or TypeScript code — even if they don't explicitly say "skill" or "standards".
---

## Standards Detection

Search for ESLint config (`.eslintrc.*`, `eslint.config.*`, `package.json`). If found, merge with baseline (ESLint takes precedence). Otherwise use baseline only.

## Baseline Standards

### Types & Imports

- Use `type` (not `interface`)
- Use `import type` for type-only imports
- Naming: `ComponentNameProps`, `UseHookNameOptions`
- Order: types → libraries → components → utilities → styles

### Naming

| Element             | Convention       | Example           |
| ------------------- | ---------------- | ----------------- |
| Components          | PascalCase       | `UserProfile`     |
| Variables/functions | camelCase        | `getUserData`     |
| Constants           | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Event handlers      | on\*             | `onClick`         |
| Hooks               | use\*            | `useAuth`         |

### Components

- Functional components with arrow functions
- Destructure props with defaults
- Named exports preferred
- Set `displayName`
- Memoize: `useCallback` for callbacks, `useMemo` for computations
- Early returns for guards
- Conditional rendering with `&&`

### Custom Components

```typescript
type ComponentNameProps = {}

export const ComponentName: FC<ComponentNameProps> = props => {
  const {} = props
}

ComponentName.displayName = 'ComponentName'
```

### Custom Hooks

```typescript
export function useHookName(options: UseHookNameOptions) {
  const { a, b } = options
  // a, b
  return {}
}
```

### Code Style

- TypeScript strict mode
- Avoid `any` → use `unknown` or specific types
- Prefer `const` over `let`
- Single responsibility functions
