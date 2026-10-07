# Frontend Conventions

## Module Organization

- Identify the actual application or package source root first, then organize pages and shared code within it.
- Use PascalCase for component names and directories, including page components, shared components, and module-private components. Each standalone component's directory name must match its component name.
- Preserve the route directory and entry file names required by file-based routing frameworks.
- Place shared components in the shared `components/` directory and page-private components in the page module's own `components/` directory. Keep page-private code inside the page module; do not create detached sibling directories for it.
- Aggregate shared modules' named exports through `index`, and import them through that public entry point. Source file extensions follow the project's language.
- Keep component styles in the same directory as the component.

## Directory Structure Example

The filenames below illustrate a TypeScript React Web project; the same directory organization applies to Vue and other frontend projects. Use the project's existing source root and create directories and files only as needed.

```text
src/
├── components/
│   ├── index.ts              # Public shared-component exports
│   └── UserCard/
│       ├── index.tsx
│       └── index.module.less
├── hooks/
│   ├── index.ts              # Public shared-hook exports
│   └── usePagination.ts      # Shared reusable logic
└── pages/
    └── UserList/
        ├── index.tsx         # Page component entry
        ├── api.ts            # Page requests
        ├── components/
        │   └── UserFilter/
        │       └── index.tsx
        └── hooks/
            └── useUserList.ts
```

Adapt component file extensions to the framework, such as `.vue` for Vue components. Match style extensions to the project's preprocessor, and use the corresponding framework's `hooks/` or `composables/` directory for reusable logic.

This example uses page-level request organization. When requests are managed centrally, place them in `services/` instead of duplicating them in a page-level `api.ts`.

## Request Wrappers

- Frontend request wrappers send requests and pass parameters. The caller handles business data transformations, page state updates, and rendering logic.
- Follow the project's existing convention for request file locations. For centralized organization, use `services/` and split files by business domain. For page-level organization, place the request entry at the page module root, alongside the page component entry.

## Application Module Files

These file responsibilities apply to page and business modules. The table uses TypeScript filenames; use the corresponding project extensions for other languages.

| File | Responsibility |
| --- | --- |
| `index.ts` | Aggregate the module's public exports |
| `api.ts` | Page-level request entry; prefer this filename in TypeScript projects |
| `type.ts` | Module types |
| `enum.ts` | Enums and their associated `*_MAP` display mappings |
| `text.ts` | Optional module-level copy entry for internationalized projects, referenced as `TEXT` |
| `utils.ts` | Pure functions and data transformations |

## Import Order

In frontend application modules, order the groups that are actually present.

Types only → framework → utility libraries → platform/UI libraries → request modules → local utilities and components → enums → internationalized copy, if any → styles.

## Web Page Layout

Prefer HTML elements that match the content's semantics. Use `div` for layout-only containers.

## Web Styles

- When using CSS Modules, import them as `styles` and use `index.module.less` as the filename. Change only the extension when using another preprocessor or plain CSS.
- Use kebab-case for selectors. Use `&-` to append suffixes when the preprocessor supports nesting.
- Prefer existing design tokens for colors, spacing, and font sizes, with type-compatible fallback values such as `var(--color-text, #333)` or `var(--spacing, 16px)`.
- When overriding third-party component classes, place `:global(...)` under a local selector in the current CSS Module. Avoid unscoped global overrides.
- Inline styles are acceptable for one-off dynamic dimensions. Extract repeated styles into style files or shared constants.
- Keep conditional class-name expressions consistent within a module.
