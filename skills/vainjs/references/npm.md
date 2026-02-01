## package.json

```json
{
  "name": "",
  "version": "0.0.1",
  "description": "",
  "keywords": [],
  "license": "MIT",
  "author": "",
  "sideEffects": false,
  "type": "module",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": ["package.json", "README.md", "dist"],
  "scripts": {
    "build": "rolldown -c"
  },
  "engines": {
    "node": ">=20"
  }
}
```

## tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## eslint.config.mjs

```javascript
import config from '@vainjs/eslint-config'

/** @type {import("eslint").Linter.Config} */
export default config
```

## Rolldown

**File**: `rolldown.config.ts`

```typescript
import { defineConfig } from 'rolldown'

export default defineConfig({
  input: 'src/index.ts',
  output: {
    format: 'es',
    file: 'dist/index.js',
    minify: true,
  },
  platform: 'node',
})
```

## Entry File

**File**: `src/index.ts`

```typescript
export {}
```

## NPM Specific Dependencies

**devDependencies** (append to common):

```json
{
  "@types/node": "latest",
  "rolldown": "latest",
  "typescript": "latest"
}
```

## GitHub Actions

**File**: `.github/workflows/publish.yml`

```yaml
name: Publish to NPM

on:
  workflow_dispatch:
    inputs:
      version_type:
        description: 'Version bump type'
        required: true
        default: 'patch'
        type: choice
        options:
          - patch
          - minor
          - major

jobs:
  build-and-publish:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    permissions:
      contents: write
      id-token: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'
          registry-url: 'https://registry.npmjs.org'

      - name: Install pnpm
        uses: pnpm/action-setup@v4

      - name: Install dependencies
        run: pnpm install

      - name: Build project
        run: pnpm build

      - name: Configure Git
        run: |
          git config --local user.email "${{ secrets.EMAIL }}"
          git config --local user.name "GitHub Action"

      - name: Bump version and create tag
        run: |
          pnpm version ${{ github.event.inputs.version_type }} --no-git-tag-version
          NEW_VERSION=$(node -p "require('./package.json').version")
          echo "NEW_VERSION=$NEW_VERSION" >> $GITHUB_ENV

          pnpm changelog
          git add package.json CHANGELOG.md

          git commit -m "chore(release): v$NEW_VERSION"
          git tag "v$NEW_VERSION"

      - name: Publish to NPM
        run: pnpm publish --no-git-checks --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}

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
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Required Secrets**:

- `NPM_TOKEN`: npm publish token
- `EMAIL`: Git commit email

## Directory Structure

```
project/
├── .github/workflows/publish.yml
├── .husky/
│   ├── commit-msg
│   └── pre-commit
├── .vscode/
│   ├── extensions.json
│   └── settings.json
├── src/
│   └── index.ts
├── .gitignore
├── .prettierrc
├── eslint.config.mjs
├── package.json
├── rolldown.config.ts
└── tsconfig.json
```
