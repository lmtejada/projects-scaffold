# projects-scaffold

A GitHub **template repository** for TypeScript/Node.js projects: strict TypeScript, ESLint,
Prettier, Git hooks, validated env config, and hardened GitHub Actions — with as few
dependencies as possible. Use it as the starting point for any app, CLI or library.

By default it's set up as an **app that Node runs from compiled output** (`tsc` → `dist/`).

## Prerequisites

- [Node.js](https://nodejs.org/) 24 LTS (see [`.nvmrc`](.nvmrc); `nvm use` picks it up)
- npm 11+ (ships with Node 24)

## Setup

```sh
nvm use
npm ci
cp .env.example .env
npm run dev
```

## Scripts

| Script                 | What it does                                                   |
| ---------------------- | -------------------------------------------------------------- |
| `npm run dev`          | Runs `src/index.ts` with tsx and restarts on changes           |
| `npm run build`        | Compiles `src/` to `dist/` with `tsc`                          |
| `npm start`            | Runs the compiled app from `dist/` (run `build` first)         |
| `npm run typecheck`    | Type-checks `src/` and the config files without emitting       |
| `npm run lint`         | Runs ESLint (type-aware)                                       |
| `npm run lint:fix`     | Runs ESLint and applies automatic fixes                        |
| `npm run format`       | Formats all files with Prettier                                |
| `npm run format:check` | Checks formatting without writing                              |
| `npm run check`        | Runs everything CI runs: format, lint, typecheck, build, audit |

## Folder structure

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/      # Bug report and feature request forms
│   ├── workflows/           # ci, pr-title, codeql
│   ├── CODEOWNERS
│   ├── dependabot.yml
│   ├── pull_request_template.md
│   └── SECURITY.md
├── .husky/                  # pre-commit, commit-msg, pre-push hooks
├── docs/adr/                # Architecture decision records
├── src/
│   ├── config.ts            # Loads and validates env vars (zod), exports `config`
│   └── index.ts             # Entry point
├── .env.example             # Every env variable, documented
├── eslint.config.ts
├── tsconfig.json            # Type-checking (editor, ESLint, `typecheck`)
└── tsconfig.build.json      # Emit to dist/ (`build`)
```

## Conventions

- **ES modules** throughout (`"type": "module"`). Relative imports use `.js` extensions
  (`import { x } from "./x.js"`), as Node requires for compiled ESM.
- **Path alias:** `#/*` maps to `src/*` (e.g. `import { config } from "#/config.js"`), using
  Node's native [subpath imports](https://nodejs.org/api/packages.html#subpath-imports) in
  `package.json` — no build-time rewriting. A custom `source` condition points TypeScript,
  ESLint and tsx at `src/`; plain `node` resolves the same import to `dist/`. (`@/` isn't
  possible natively: Node requires subpath imports to start with `#`.)
- **Config:** read env vars only through `config` from `src/config.ts`. Add new variables to
  the zod schema **and** `.env.example`. `.env` is loaded with Node's built-in
  `process.loadEnvFile()` — no dotenv.

## After creating a repo from this template

GitHub copies files, but **not** repository settings. In the new repo:

1. **Update project identity:** `name`, `description` and `author` in `package.json`, the
   title of this README, `LICENSE`, `CODEOWNERS`, and the URLs in `.github/SECURITY.md` and
   `.github/ISSUE_TEMPLATE/config.yml`.
2. **Merge settings** (Settings → General → Pull Requests): allow merge commits only,
   enable "Automatically delete head branches".
3. **Ruleset for `main`** (Settings → Rules → Rulesets): require a pull request (allowed
   merge method: merge), require the `Format and Type Check` status check, block force pushes and deletion.
4. **Security** (Settings → Advanced Security): enable Dependabot alerts and security
   updates, secret scanning with push protection, and private vulnerability reporting.
5. **Dependabot** starts using `.github/dependabot.yml` automatically, but check that its
   PRs can run the workflows.

Or do it with the `gh` CLI:

```sh
gh repo edit --enable-merge-commit --enable-squash-merge=false --enable-rebase-merge=false \
  --delete-branch-on-merge --enable-secret-scanning --enable-secret-scanning-push-protection
gh api -X PUT repos/{owner}/{repo}/vulnerability-alerts
gh api -X PUT repos/{owner}/{repo}/automated-security-fixes
```

## License

[ISC](LICENSE)
