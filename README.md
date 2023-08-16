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

- Node.js Gallium LTS (`^16.20.2`)
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

## Cleaning

```sh
yarn run clean
```

## Migrate to NPM

### 1. Remove the following files

- `.yarn/`
- `.yarnrc.yml`
- `yarn.lock`

### 2. Apply the following patches

```diff
--- a/.github/workflows/push.yml
+++ b/.github/workflows/push.yml
@@ -20,16 +20,18 @@ jobs:
       - name: Prepare the Node.js version ${{ matrix.node-version }} environment
         uses: actions/setup-node@v2
         with:
-          cache: ${{ !env.ACT && 'yarn' || '' }}
+          cache: ${{ !env.ACT && 'npm' || '' }}
           node-version: ${{ matrix.node-version }}
       - name: Enable the corepack because of the Yarn 3
         run: corepack enable
+      - name: set npm config
+        run: npm config set unsafe-perm true
       - env:
           HUSKY: 0
         name: Install the dependencies
-        run: yarn install --inline-builds
+        run: npm ci
       - name: Run the tests
-        run: yarn run test
+        run: npm test
     strategy:
       matrix:
         node-version:
```

```diff
--- a/.husky/commit-msg
+++ b/.husky/commit-msg
@@ -4,4 +4,4 @@

 . "$(dirname "$0")/_/husky.sh"

-yarn exec commitlint --edit "${1}"
+npm x --no -- commitlint --edit "${1}"
```

```diff
--- a/.husky/pre-commit
+++ b/.husky/pre-commit
@@ -4,4 +4,4 @@

 . "$(dirname "$0")/_/husky.sh"

-yarn exec lint-staged
+npm x --no -- lint-staged
```

```diff
--- a/.vim/coc-settings.json
+++ b/.vim/coc-settings.json
@@ -1,6 +1,4 @@
 {
-  "eslint.nodePath": ".yarn/sdks",
-  "eslint.packageManager": "yarn",
-  "tsserver.tsdk": ".yarn/sdks/typescript/lib",
+  "tsserver.tsdk": "node_modules/typescript/lib",
   "workspace.workspaceFolderCheckCwd": false
 }
```

```diff
--- a/.vscode/settings.json
+++ b/.vscode/settings.json
@@ -1,5 +1,4 @@
 {
-  "eslint.nodePath": ".yarn/sdks",
   "files.readonlyInclude": {
     "**/.pnp.*": true,
     "**/.yarn/**/*": true,
@@ -13,13 +12,11 @@
    "**/node_modules/**/*": true
  },
  "git.branchProtection": ["main"],
-  "prettier.prettierPath": ".yarn/sdks/prettier/index.js",
   "search.exclude": {
     "**/.pnp.*": true,
     "**/.yarn": true,
     "**/node_modules/**/*": true
   },
   "typescript.enablePromptUseWorkspaceTsdk": true,
-  "typescript.tsdk": ".yarn/sdks/typescript/lib",
   "yaml.schemas": {
     "https://raw.githubusercontent.com/reviewpad/schemas/main/latest/schema.json": [
       "reviewpad.yml"
```

```diff
--- a/package.json
+++ b/package.json
@@ -16,16 +16,16 @@
   "files": [],
   "scripts": {
     "clean": "rimraf -g \".eslintcache\" \"*.tgz\" \"*.tsbuildinfo\"",
-    "postinstall": "husky install",
-    "lint": "conc -m 1 \"yarn:lint:*:check\"",
+    "lint": "conc -m 1 \"npm:lint:*:check\"",
     "lint:cspell:check": "cspell lint --no-progress --show-suggestions -u \"./**/*\"",
     "lint:eslint:check": "eslint --cache --cache-strategy=content -f codeframe \"./**/*\"",
-    "lint:eslint:fix": "yarn run lint:eslint:check --fix",
-    "lint:fix": "conc -m 1 \"yarn:lint:*:fix\"",
-    "lint:prettier:check": "yarn run prettier -cu",
-    "lint:prettier:fix": "yarn run prettier -uw",
-    "prettier": "prettier --cache --loglevel=warn \"$@\" \"./**/*\"",
-    "test": "yarn run lint"
+    "lint:eslint:fix": "npm run lint:eslint:check -- --fix",
+    "lint:fix": "conc -m 1 \"npm:lint:*:fix\"",
+    "lint:prettier:check": "npm run prettier -- -cu",
+    "lint:prettier:fix": "npm run prettier -- -uw",
+    "prepare": "husky install",
+    "prettier": "prettier --cache --loglevel=warn \"./**/*\"",
+    "test": "npm run lint"
   },
   "prettier": "@kurone-kito/prettier-config",
   "devDependencies": {
@@ -39,7 +39,6 @@
     "@kurone-kito/typescript-config": "^0.7.3",
     "@typescript-eslint/eslint-plugin": "^6.4.0",
     "@typescript-eslint/parser": "^6.4.0",
-    "@yarnpkg/sdks": "^3.0.0-rc.48",
     "concurrently": "^8.2.0",
     "cspell": "^6.31.2",
     "eslint": "^8.45.0",
@@ -64,7 +63,6 @@
     "typescript": "~5.1.6",
     "typescript-eslint-language-service": "^5.0.5"
   },
-  "packageManager": "yarn@3.6.1",
   "engines": {
     "node": ">=16.20"
   },
```

### 3. Run the following command

```sh
npm install
git add -A
git commit -m "feat: migrate to NPM from Yarn"
```

## Rules for Development

Introduce commit message validation at commit time.
The “**[Conventional Commits](https://www.conventionalcommits.org/ja/)**”
rule is applied to discourage committing messages that violate conventions.

## LICENSE

MIT
