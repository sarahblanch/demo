# CLAUDE.md

Guidance for Claude Code (and other AI assistants) working in this repository.

## Project overview

This is a minimal TypeScript template repository (`newrepo-ts`). It currently
contains a single example module (`add`) demonstrating the project's
conventions — file layout, build setup, and test style. Treat it as a
starting point: new source code should follow the same patterns already
established here.

## Codebase structure

```
.
├── src/
│   ├── add.ts          # example module (source)
│   └── add.test.ts      # colocated unit test for add.ts
├── tsconfig.json        # TypeScript compiler configuration
├── package.json          # scripts and dependencies
└── package-lock.json
```

- Source files live under `src/` and compile to `dist/` (both `dist/` and
  `node_modules/` are gitignored — never commit build output).
- Tests are colocated with the source file they test, using the
  `*.test.ts` naming convention (e.g. `add.ts` → `add.test.ts`).
- The package uses native ESM (`"type": "module"` in `package.json`).
  Relative imports between source files must include the `.js` extension
  (e.g. `import { add } from "./add.js";`), even though the source file is
  `.ts` — this is required by `moduleResolution: "NodeNext"`.

## Development workflow

Install dependencies:
```bash
npm install
```

Run the test suite (vitest):
```bash
npm test
```

Build (type-check and emit JS to `dist/`):
```bash
npm run build
```

There is no separate lint script or CI configuration in this repository at
present. The TypeScript compiler runs in `strict` mode, so type-checking via
`npm run build` is the primary correctness gate beyond tests.

## Key conventions

- **Module system**: ESM only. Use `import`/`export`, not `require`.
- **Import extensions**: always use `.js` in relative import specifiers,
  never `.ts`.
- **Strict TypeScript**: `strict: true` is enabled — avoid `any`, prefer
  explicit types on exported function signatures.
- **Testing**: use `vitest` (`describe`/`it`/`expect`), colocated
  `*.test.ts` files next to the code they test. Test files are excluded
  from the build (see `tsconfig.json` `exclude`).
- **No external runtime dependencies** currently exist — `typescript` and
  `vitest` are the only (dev) dependencies. Think carefully before adding
  new dependencies to keep the template lightweight.

## Notes for AI assistants

- When adding a new module, mirror the existing pattern: one focused
  `.ts` file in `src/`, plus a colocated `.test.ts` file with vitest tests
  covering normal, edge, and boundary cases (see `add.test.ts` for the
  style to follow).
- Keep changes minimal and consistent with the established structure
  rather than introducing new tooling (linters, frameworks, alternate test
  runners) unless explicitly requested.
- Run `npm test` (and `npm run build` for type-checking) after making
  changes to verify correctness.
