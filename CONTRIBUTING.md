# Contributing

## Package manager

This repo uses **Yarn 1** (`yarn.lock` is canonical). Enable Corepack or install
Yarn 1.22.x, then:

```bash
yarn install
yarn lint
yarn typecheck
```

Do not commit `package-lock.json` or `npm-shrinkwrap.json`.

## Hook imports (auth + wallet)

Use the `@/contexts` barrel as the single public surface for auth and wallet hooks/providers.

Allowed:
- `import { useAuth, useWallet, useWalletConfig, AuthProvider, WalletProvider } from "@/contexts";`

Disallowed:
- `@/context/AuthContext`
- `@/contexts/AuthContext`
- `@/contexts/WalletProvider`

This is enforced in ESLint via `no-restricted-imports`.

## Optional: pre-commit lint with lint-staged + Husky

`yarn lint` is the source of truth and runs in CI. The pre-commit hook below is
**optional** — it gives faster local feedback but is never required.

### Setup (one-time, per contributor)

```bash
# 1. Install dev dependencies (not committed to the repo)
yarn add --dev husky lint-staged

# 2. Initialise Husky
npx husky init

# 3. Add the pre-commit hook
echo "npx lint-staged" > .husky/pre-commit
```

The staged-file rules live in `.lintstagedrc` at the repo root.
TypeScript checking uses `--skipLibCheck` to keep the hook fast.

### Skipping the hook

```bash
git commit --no-verify -m "wip: skip pre-commit"
```

### Why not mandatory?

Mandatory hooks slow down contributors on large changesets and can block
emergency commits. CI (`yarn lint && yarn typecheck`) is the enforced gate.
