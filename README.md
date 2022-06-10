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

- Node.js Fermium LTS (`^14.19.3`)
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

### 1. Remove following files

- `.yarn/`
- `.yarnrc.yml`
- `yarn.lock`

### 2. Apply the following patches

```diff
--- a/.github/workflows/push.yml
+++ b/.github/workflows/push.yml
@@ -13,16 +13,14 @@ jobs:
       - name: Prepare the Node.js version ${{ matrix.node-version }} environment
         uses: actions/setup-node@v2
         with:
-          cache: ${{ !env.ACT && 'yarn' || '' }}
+          cache: ${{ !env.ACT && 'npm' || '' }}
           node-version: ${{ matrix.node-version }}
-      - name: Install the Yarn
-        run: npm install --global yarn@berry
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
+npx --no-install commitlint --edit "${1}"
```

```diff
--- a/.husky/pre-commit
+++ b/.husky/pre-commit
@@ -4,4 +4,4 @@

 . "$(dirname "$0")/_/husky.sh"

-yarn exec pretty-quick --staged
+npx --no-install pretty-quick --staged
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
@@ -5,24 +5,13 @@
     ".github/CODE_OF_CONDUCT.*",
     ".vscode",
     ".vscode-insiders",
-    ".yarn",
     "node_modules",
-    "vscode-extension",
-    "yarn.lock"
+    "vscode-extension"
   ],
   "cSpell.words": ["kito", "kurone", "kuroné", "tsbuildinfo"],
-  "eslint.nodePath": ".yarn/sdks",
   "files.watcherExclude": {
-    "**/.eslintcache": true,
-    "**/.pnp.*": true,
-    "**/.yarn/cache/**": true,
-    "**/.yarn/unplugged/**": true
-  },
-  "prettier.prettierPath": ".yarn/sdks/prettier/index.js",
-  "search.exclude": {
-    "**/.pnp.*": true,
-    "**/.yarn": true
+    "**/.eslintcache": true
   },
   "typescript.enablePromptUseWorkspaceTsdk": true,
-  "typescript.tsdk": ".yarn/sdks/typescript/lib"
+  "typescript.tsdk": "node_modules/typescript/lib"
 }
```

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
     "lint:eslint:check": "eslint --cache --format codeframe \"./**/*\"",
-    "lint:eslint:fix": "yarn run lint:eslint:check --fix",
-    "lint:fix": "concurrently -m 1 \"yarn:lint:*:fix\"",
+    "lint:eslint:fix": "npm run lint:eslint:check --fix",
+    "lint:fix": "concurrently -m 1 \"npm:lint:*:fix\"",
     "lint:prettier:check": "prettier --loglevel=warn --check \"./**/*\"",
     "lint:prettier:fix": "prettier --loglevel=warn --write \"./**/*\"",
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
     "node": ">=14.19",
-    "yarn": ">=2.4.3"
+    "node": ">=14.19"
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
