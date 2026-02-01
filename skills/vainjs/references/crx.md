## package.json

```json
{
  "name": "",
  "displayName": "",
  "version": "0.0.0",
  "private": true,
  "description": "",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build"
  },
  "dependencies": {
    "react": "latest",
    "react-dom": "latest"
  },
  "devDependencies": {
    "@crxjs/vite-plugin": "latest",
    "@types/chrome": "latest",
    "@types/react": "latest",
    "@types/react-dom": "latest",
    "@vitejs/plugin-react": "latest",
    "typescript": "latest",
    "vite": "latest",
    "vite-plugin-zip-pack": "latest"
  },
  "engines": {
    "node": ">=20"
  }
}
```

## eslint.config.mjs

```javascript
import config from '@vainjs/eslint-config/react'

/** @type {import("eslint").Linter.Config} */
export default config
```

## manifest.config.ts

```typescript
import { defineManifest } from '@crxjs/vite-plugin'
import { displayName, description, version } from './package.json'

const isDev = process.env.NODE_ENV === 'development'

export default defineManifest({
  name: `${displayName}${isDev ? ` [dev]` : ''}`,
  description: description,
  version,
  manifest_version: 3,
  icons: {
    16: 'img/logo-16.png',
    32: 'img/logo-32.png',
    48: 'img/logo-48.png',
    128: 'img/logo-128.png',
  },
  background: {
    service_worker: 'src/background/index.ts',
    type: 'module',
  },
  side_panel: {
    default_path: 'src/sidepanel/index.html',
  },
  permissions: ['sidePanel', 'storage', 'tabs'],
  host_permissions: isDev ? ['http://localhost:*/*'] : [],
})
```

## vite.config.ts

```typescript
import path from 'node:path'
import { crx } from '@crxjs/vite-plugin'
import react from '@vitejs/plugin-react'
import zip from 'vite-plugin-zip-pack'
import { defineConfig } from 'vite'
import { version, displayName } from './package.json'
import manifest from './manifest.config'

export default defineConfig({
  resolve: {
    alias: {
      '@': `${path.resolve(__dirname, 'src')}`,
    },
  },
  plugins: [
    react(),
    crx({ manifest }),
    zip({
      outDir: 'release',
      outFileName: `${displayName}-${version}-chrome.zip`,
    }),
  ],
  server: {
    port: 5173,
    strictPort: true,
    hmr: {
      clientPort: 5173,
    },
    cors: {
      origin: [/chrome-extension:\/\//],
    },
  },
})
```

## tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
    "allowJs": false,
    "skipLibCheck": true,
    "esModuleInterop": false,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

## tsconfig.node.json

```json
{
  "compilerOptions": {
    "composite": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "allowSyntheticDefaultImports": true
  },
  "include": ["manifest.config.ts", "vite.config.ts"]
}
```

## src/background/index.ts

```typescript
chrome.runtime.onInstalled.addListener(() => {
  chrome.sidePanel.setPanelBehavior({ openPanelOnActionClick: true })
})
```

## src/sidepanel/index.html

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Chrome Extension</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="./index.tsx"></script>
  </body>
</html>
```

## src/sidepanel/index.tsx

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { App } from './App'

ReactDOM.createRoot(document.getElementById('app') as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

## src/sidepanel/App.tsx

```tsx
export const App = () => {
  return <div>Hello Extension</div>
}
```

## .github/workflows/publish-extension.yml

```yaml
name: Publish Chrome Extension

on:
  workflow_dispatch:
    inputs:
      version:
        description: 'Version bump type'
        required: true
        type: choice
        options:
          - patch
          - minor
          - major
        default: minor

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'

      - name: Install pnpm
        uses: pnpm/action-setup@v4

      - name: Install dependencies
        run: pnpm install

      - name: Configure git
        run: |
          git config --local user.email "${{ secrets.EMAIL }}"
          git config --local user.name "GitHub Action"

      - name: Bump version and create tag
        run: |
          pnpm version ${{ github.event.inputs.version }} --no-git-tag-version
          NEW_VERSION=$(node -p "require('./package.json').version")
          echo "NEW_VERSION=$NEW_VERSION" >> $GITHUB_ENV
          pnpm changelog
          git add package.json CHANGELOG.md
          git commit -m "chore(release): v$NEW_VERSION"
          git tag "v$NEW_VERSION"

      - name: Build extension
        run: pnpm build

      - name: Upload to Chrome Web Store
        uses: mnao305/chrome-extension-upload@v5.0.0
        with:
          file-path: release/*-chrome.zip
          extension-id: ${{ secrets.CHROME_EXTENSION_ID }}
          client-id: ${{ secrets.CHROME_CLIENT_ID }}
          client-secret: ${{ secrets.CHROME_CLIENT_SECRET }}
          refresh-token: ${{ secrets.CHROME_REFRESH_TOKEN }}
          publish: true

      - name: Push changes and tags
        run: |
          git push origin main
          git push origin "v$NEW_VERSION"

      - name: Create GitHub Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: v${{ env.NEW_VERSION }}
          name: v${{ env.NEW_VERSION }}
          body: |
            For detailed changes, see [CHANGELOG.md](${{ github.server_url }}/${{ github.repository }}/blob/main/CHANGELOG.md).
          files: release/*-chrome.zip
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Required Secrets**:

- `CHROME_EXTENSION_ID`: Chrome Web Store extension ID
- `CHROME_CLIENT_ID`: Google OAuth client ID
- `CHROME_CLIENT_SECRET`: Google OAuth client secret
- `CHROME_REFRESH_TOKEN`: Google OAuth refresh token
- `EMAIL`: Git commit email

## Directory Structure

```
project/
├── .github/
│   └── workflows/
│       └── publish-extension.yml
├── .husky/
│   ├── commit-msg
│   └── pre-commit
├── .vscode/
│   ├── extensions.json
│   └── settings.json
├── public/
│   └── img/
│       ├── logo-16.png
│       ├── logo-32.png
│       ├── logo-48.png
│       └── logo-128.png
├── src/
│   ├── background/
│   │   └── index.ts
│   └── sidepanel/
│       ├── App.tsx
│       ├── index.html
│       └── index.tsx
├── .gitignore
├── .prettierrc
├── eslint.config.mjs
├── manifest.config.ts
├── package.json
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts
```
