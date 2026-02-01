## .prettierrc

```json
{
  "singleQuote": true,
  "tabWidth": 2,
  "bracketSpacing": true,
  "bracketSameLine": true,
  "trailingComma": "es5",
  "arrowParens": "avoid",
  "semi": false
}
```

## .husky/pre-commit

```bash
npx lint-staged
```

## .husky/commit-msg

```bash
npx commitlint --edit $1
```

## .vscode/settings.json

```json
{
  "eslint.enable": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "eslint.workingDirectories": ["."]
}
```

## .vscode/extensions.json

```json
{
  "recommendations": ["dbaeumer.vscode-eslint", "esbenp.prettier-vscode"]
}
```

## .gitignore

```
.DS_Store
*.log
node_modules/
dist/
```

## package.json (partial)

Merge into `package.json`:

```json
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -s",
    "preinstall": "npx only-allow pnpm",
    "prepare": "husky"
  },
  "commitlint": {
    "extends": ["@commitlint/config-conventional"]
  },
  "lint-staged": {
    "*.{ts,js}": ["eslint --fix", "prettier --write"]
  },
  "devDependencies": {
    "@commitlint/cli": "latest",
    "@commitlint/config-conventional": "latest",
    "@vainjs/eslint-config": "latest",
    "conventional-changelog-cli": "latest",
    "eslint": "latest",
    "husky": "latest",
    "lint-staged": "latest",
    "prettier": "latest"
  },
  "packageManager": "pnpm@<current-version>"
}
```

> **Note:** Replace `<current-version>` with the currently installed pnpm version (via `pnpm --version`) when creating a project.
