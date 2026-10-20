# Contributing

## Setup

```sh
nvm use          # Node version from .nvmrc
npm ci           # also installs the Git hooks
cp .env.example .env
```

## Branch naming

Branch from `main` using `<type>/<short-description>`, with the same types as commits:

- `feat/add-health-endpoint`
- `fix/config-port-parsing`
- `docs/update-readme`
- `chore/bump-deps`

## Commit messages

Commits follow [Conventional Commits](https://www.conventionalcommits.org/), enforced by
commitlint in the `commit-msg` hook:

```text
<type>(<optional scope>): <subject>

feat(config): add PORT variable
fix: handle missing .env file
docs: explain path alias
```

Common types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `build`, `perf`.
Add `!` after the type (`feat!:`) or a `BREAKING CHANGE:` footer for breaking changes.

PRs are **squash-merged**, so the **PR title** becomes the commit on `main` and is validated
the same way.

## Before opening a PR

```sh
npm run check
```

This runs exactly what CI runs: `format:check`, `lint`, `typecheck`, `build` and
`npm audit --audit-level=high`. Use `npm run format` and `npm run lint:fix` to fix most issues
automatically.

The Git hooks also run checks for you:

| Hook         | Runs                              |
| ------------ | --------------------------------- |
| `pre-commit` | ESLint + Prettier on staged files |
| `commit-msg` | commitlint                        |
| `pre-push`   | `npm run typecheck`               |

## Pull requests

Fill in the PR template, keep PRs focused on one change, and make sure CI is green.
For significant architectural changes, add an ADR in `docs/adr/`.
