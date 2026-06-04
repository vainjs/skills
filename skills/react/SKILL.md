---
name: react
description: TypeScript/React code standards for writing, modifying, generating, reviewing, and refactoring React code. Make sure to use this skill for any task that creates or changes React code, TSX/JSX files, React components, hooks, props, state, React TypeScript types, or React project file organization — even if the user does not explicitly mention "React", "skill", or "standards".
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

## Directory Organization

When this skill creates or reorganizes React files, it must follow this directory organization. First identify the existing app/module root (`src/`, `src/apps/<app>/`, `packages/<name>/src/`, framework route directory, etc.), then apply these rules inside that root.

- Put reusable code in shared `hooks/` and `components/`.
- Put page-only code inside the page module's `hooks/`, `components/`, and `api.ts`.
- Do not create ad hoc sibling folders for hooks, components, page APIs, or page-private code.
- Preserve clearly established equivalent project naming only when the shared vs page-private boundaries stay the same.

### Shared Hooks

```
hooks/
├── index.ts              # re-exports all shared hooks (single import entry)
└── useXxx.ts             # one hook per file, named useXxx
```

### Shared Components

```
components/
├── index.ts              # re-exports all shared components
├── Button/
│   ├── index.tsx         # exports Button component
│   └── index.module.less
└── XxxCard/
    ├── index.tsx         # exports XxxCard component
    └── index.module.less
```

### Page Modules

```
<Name>/                   # <Name> = main component name (e.g., UserProfile)
├── index.tsx             # exports the page's main component
├── index.module.less
├── api.ts                # page API requests only; no business logic
├── components/           # page-private components (optional)
│   ├── index.ts          # re-exports this page's components
│   └── ...
└── hooks/                # page-private hooks (optional)
    ├── index.ts          # re-exports this page's hooks
    └── ...
```

**Conventions:**

1. **`index.ts` for unified exports** — keeps imports clean: `import { Button } from '@/components'`
2. **Named exports only** — avoid default exports from shared components
3. **Directory name = Component name** — `UserProfile/index.tsx` exports `UserProfile`
4. **Page `api.ts` is request-only** — keep page API calls there; do not put data transformation, state updates, or business branching there

**Import examples:**

```typescript
// Good
import { Button } from '@/components'
import { useAuth } from '@/hooks'
import { UserProfile } from '@/pages/UserProfile' // or the project's existing page/module path

// Avoid
import Button from '@/components/Button' // inconsistent with index.ts pattern
```
