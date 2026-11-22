# Agent Guidelines

## Environment setup

### Node.js version

Before running project commands, change to the repository directory that contains `.nvmrc` and run `nvm use` there.
Do not run `nvm use` from a parent directory or a different working copy; use the `.nvmrc` that belongs to the
worktree where the project command will run.

If `nvm use` reports that the version from `.nvmrc` is not installed, remain in the directory containing `.nvmrc`
and run `nvm install` without a version argument so NVM installs the repository-pinned version. Then run `nvm use`
again. If the `nvm` command itself is unavailable, stop and report that NVM is not loaded or installed instead of
silently using a different Node.js version.

### Package manager

- This project uses [pnpm](https://pnpm.io/) to manage dependencies; `pnpm-lock.yaml` is the lockfile.
- Use `pnpm install`, `pnpm add` and `pnpm remove` to manage dependencies, and `pnpm run <script>` to run `package.json` scripts.

## Testing

### No tautological tests

Tests must verify an independent behavioral contract and fail when that contract is broken. Do not compare a value with itself, derive the expected result from the implementation under test, or mock the behavior under test and only assert its configured return value. Use explicit expected results or an independent oracle, and limit mocks to collaborators.

### Test constants

When a test asserts a production contract, keep the expected value explicit in the test or in a test-local constant. Do not import production constants only to mirror the value being asserted, because the test would update with the implementation and stop catching unintended contract changes.

Production imports are fine for types, schemas, factories, enum values used to create valid setup data, or helpers that are not the expected value under assertion. If a production constant is part of the behavior being verified, repeat the expected literal in the test and let a production change break the test intentionally.

### E2E selectors

Use stable `data-testid` selectors for E2E assertions whenever possible. Avoid `.locator()` and `.getByText()` in E2E tests, and use exact string values for role `name` options instead of regular expressions.
