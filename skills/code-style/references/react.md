# React Conventions

## Naming and Files

- Use `index.tsx` or `index.jsx` for component entries, matching the project's language.
- Place shared hooks in the shared `hooks/` directory and module-private hooks in the module's own `hooks/` directory.
- Prefix hooks with `use`, match each filename to its hook name, and keep one hook per file.
- In TypeScript, name props and ref-handle types `XProps` and `XRef`. Name hook configuration types `UseXxxOptions`; local private configuration types may use `Options`.
- Use double quotes for JSX string attributes. For multiline, non-self-closing tags, place `>` on the last attribute's line. Let the formatter handle `/>` for self-closing tags.

## Hooks

- Hooks encapsulate state and side effects; components handle composition and rendering.
- Prefer existing project hooks and utilities. If no suitable implementation exists, use built-in hooks for the current need; do not introduce dependencies merely to satisfy style requirements.
- Use an options object for multiple configuration fields; positional parameters are acceptable for a small, fixed set of inputs. Return an object, or a readonly tuple for paired values and operations.
- Keep inputs that drive recomputation or requests in dependency lists. Do not freeze them just to shorten the lists.
- Initialization-only configuration may be stored in a ref. When callbacks need the latest values, prefer the project's existing latest-value ref utility.
- When mount-time or deep-comparison logic is needed, prefer existing project implementations. Do not hide update relationships by stringifying dependencies or using empty dependency lists.
- Prefer the project's existing no-op function for default callbacks. Use a no-op error handler only when ignoring errors is explicitly allowed.

## Components

- Declare components as arrow functions assigned to named variables.
- Receive props as an object, then destructure props and set defaults in the function body.
- Set `displayName` on every component, matching its component name.
- Expose refs for imperative interfaces according to the project's convention.
- Use default exports for page entry components and business-module entry components, and named exports for shared components. Preserve the export interfaces used by existing consumers.
- Place pure functions and constants related to the current component above it. Extract reusable code into files with the corresponding responsibilities.
- Keep the page component's JSX structure intact. Avoid breaking it into overly granular child components; extract a child component only when it is sufficiently complex to justify being separated. Prefer hooks to separate logic by responsibility, while keeping the page component as the entry point so the page structure and the relationships among its child components and hooks remain clear.
- Use early returns for guards. For single-branch conditional rendering, use an explicit boolean condition with `&&`.

## Memoization

Do not use `React.memo` by default. Use `useMemo` and `useCallback` based on actual computation costs and dependency-stability needs.
