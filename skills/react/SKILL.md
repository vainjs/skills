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

## Directory Structure

```
src/
├── hooks/                    # shared hooks
│   ├── index.ts              # re-exports all hooks (single import entry)
│   └── useXxx.ts            # one hook per file, named useXxx
├── components/               # shared components
│   ├── index.ts              # re-exports all components
│   ├── Button/
│   │   ├── index.tsx         # exports Button component
│   │   └── index.module.less
│   └── XxxCard/
│       ├── index.tsx         # exports XxxCard component
│       └── index.module.less
└── pages/                    # organized by page module
    └── <Name>/               # <Name> = main component name (e.g., UserProfile)
        ├── index.tsx         # exports the page's main component
        ├── index.module.less
        ├── components/       # page-private components (optional)
        │   ├── index.ts     # re-exports this page's components
        │   └── ...
        └── hooks/           # page-private hooks (optional)
            ├── index.ts     # re-exports this page's hooks
            └── ...
```

**Conventions:**

1. **`index.ts` for unified exports** — keeps imports clean: `import { Button } from '@/components'`
2. **Named exports only** — avoid default exports from shared components
3. **Directory name = Component name** — `pages/UserProfile/index.tsx` exports `UserProfile`
4. **Co-locate page-specific code** — complex pages may have their own `components/` and `hooks/`

**Import examples:**
```typescript
// Good
import { Button, useAuth } from '@/components'
import { UserProfile } from '@/pages/UserProfile'

// Avoid
import Button from '@/components/Button'  // inconsistent with index.ts pattern
```
