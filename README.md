# 📄 Yarn project boilerplate

## Features

- Yarn with PnP
- TypeScript
- ESLint
- Prettier
- Visual Studio Code / Vim ready
- CI / CD configurations
  - Dependabot
  - GitHub Actions

## System Requirements

- Node.js Fermium LTS (`^14.19.2`)
- Yarn (`>=2.4.3`)

## Install the dependencies

```sh
yarn install
```

## Linting

```sh
yarn run lint
yarn run lint:fix # Lint and auto-fix
```

## Testing

```sh
yarn run test
```

Currently, the command works as an alias for the `yarn run lint` command.

## Migrate to NPM

### 1. Remove following files

- `.yarn/`
- `.yarnrc.yml`
- `yarn.lock`

### 2. Apply the following patch to `package.json`

```diff
--- a/package.json
+++ b/package.json
@@ -12,14 +12,14 @@
   "author": "kurone-kito <krone@kit.black> (https://kit.black/)",
   "files": [],
   "scripts": {
-    "postinstall": "husky install",
-    "lint": "concurrently -m 1 \"yarn:lint:*:check\"",
+    "prepare": "husky install",
+    "lint": "concurrently -m 1 \"npm:lint:*:check\"",
     "lint:eslint:check": "eslint --cache --format codeframe \"$@\" \"./**/*\"",
-    "lint:eslint:fix": "yarn run lint:eslint:check --fix",
-    "lint:fix": "concurrently -m 1 \"yarn:lint:*:fix\"",
+    "lint:eslint:fix": "npm run lint:eslint:check --fix",
+    "lint:fix": "concurrently -m 1 \"npm:lint:*:fix\"",
     "lint:prettier:check": "prettier --check \"./**/*\"",
     "lint:prettier:fix": "prettier --write \"./**/*\"",
-    "test": "yarn run lint"
+    "test": "npm run lint"
   },
   "devDependencies": {
     "@commitlint/cli": "^17.0.0",
@@ -27,7 +27,6 @@
     "@types/eslint": "^8.4.2",
     "@typescript-eslint/eslint-plugin": "^5.24.0",
     "@typescript-eslint/parser": "^5.24.0",
-    "@yarnpkg/sdks": "^3.0.0-rc.6",
     "concurrently": "^7.2.0",
     "eslint": "^8.15.0",
     "eslint-config-airbnb-typescript": "^17.0.0",
@@ -51,10 +50,8 @@
     "typescript": "^4.6.4",
     "typescript-eslint-language-service": "^5.0.0"
   },
-  "packageManager": "yarn@3.2.1",
   "engines": {
-    "node": ">=14.19.2",
-    "yarn": ">=2.4.3"
+    "node": ">=14.19.2"
   },
   "publishConfig": {
     "access": "public"
```

### 3. Run following command

```sh
npm install
git add -A
git commit -m "feat: migrate to NPM from Yarn"
```

## Rules for Development

Introduce commit message validation at commit time.
“**[Conventional Commits](https://www.conventionalcommits.org/ja/)**”
rule is applied to discourage committing messages that violate conventions.

## LICENSE

MIT
