# CLAUDE.md

Guidance for Claude (and other AI assistants) working in this repository.

## Project

TypeScript/Node.js project. Node runs the compiled output: `src/` → `tsc` → `dist/`.
Node version is pinned in `.nvmrc` (24 LTS) and enforced via `engines` + `engine-strict`.

## Key commands

```sh
npm run dev         # run with tsx in watch mode
npm start           # run the compiled app from dist/ (after npm run build)
npm run check       # everything CI runs — must pass before you finish a task
npm run lint:fix    # auto-fix lint issues
npm run format      # auto-format
```

## Conventions

- ESM only. Relative imports **must** end in `.js` (`./foo.js`), even though the file is `foo.ts`.
- Import from `src/` via the `#/` alias (`#/config.js`, defined in `package.json` `"imports"`) rather than long `../../` paths.
- Use `import type` for type-only imports (`verbatimModuleSyntax` is on).
- Read env vars only through `config` from `src/config.ts`; never use `process.env` elsewhere.
  New variables go in the zod schema **and** `.env.example`, each with a comment.
- Import order is enforced by `import-x/order`: builtins, external, `#/` internal, relative.
- Commits and PR titles follow Conventional Commits (`feat: ...`, `fix(scope): ...`).
- Tooling goes in `devDependencies`. Keep runtime dependencies minimal.
- Use `npm` (lockfile is `package-lock.json`). No `npx` inside npm scripts.

## Don't change without asking

- `tsconfig.json` strictness flags, `module`/`target`, or the `#/*` alias (`package.json` `"imports"` + `customConditions`).
- ESLint rules or presets — fix the code instead of disabling rules. No `eslint-disable`
  comments without a reason in the same comment.
- Git hooks in `.husky/`, `commitlint.config.ts`, `.lintstagedrc.json`.
- `.github/workflows/*`: action SHA pins, permissions, and the CI job name
  `Format and Type Check` (the `main` ruleset requires a check with that exact name).
- `.nvmrc` / `engines` / the `@types/node` major (keep all three on the same Node major), or adding new dependencies (especially runtime ones, or `dotenv`).
- `LICENSE`.

## Decisions

Architecture decisions are recorded in `docs/adr/`. Add a new ADR for significant changes.
