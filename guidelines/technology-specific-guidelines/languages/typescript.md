# TypeScript Project Initialization

## Purpose

A clear workflow for initializing and configuring a TypeScript project inside a blank repository. It assumes the repo already exists (or that you run `git init` first) and walks from an empty directory to a compiling, runnable project.

## Prerequisites

- **Node.js** and a package manager — `npm` (default below), `pnpm`, or `yarn`. TypeScript's tooling is distributed through the npm registry, so Node is required to *develop* even when the compiled code targets something else.

## Workflow

### 1. Initialize the package

```bash
npm init -y
```

This creates a `package.json` with defaults. You will edit it in a later step.

### 2. Install TypeScript

```bash
npm install --save-dev typescript
```

TypeScript is a build-time tool, so it belongs in `devDependencies`.

### 3. Generate a tsconfig.json

```bash
npx tsc --init
```

This writes a `tsconfig.json` with every option listed (most commented out). For a cleaner starting point you can replace it with a minimal strict config and add options as needed:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "dist",
    "rootDir": "src"
  },
  "include": ["src"]
}
```

The `strict: true` flag is the single most important setting — it enables the full set of strict checks that give TypeScript its safety value.

### 4. Set up the directory structure

```plaintext
project/
├── src/
│   └── index.ts
├── dist/            # compiled output (add to .gitignore)
├── tsconfig.json
├── package.json
└── .gitignore
```

### 5. Configure package.json scripts

```json
{
  "scripts": {
    "build": "tsc",
    "watch": "tsc --watch",
    "typecheck": "tsc --noEmit"
  }
}
```

- `build` compiles to `dist/`.
- `watch` recompiles on change.
- `typecheck` checks types without emitting files — useful in CI.

### 6. Compile and run

```bash
npm run build
```

The compiled JavaScript is emitted to `dist/`. How you then *run* it depends on the runtime target — see the caveat below.

## Caveat: When Node.js Will Be Utilized

> This applies when the TypeScript code runs **on Node.js** — a server, a CLI tool, a build script, or any backend/scripting context — rather than in a browser, in Deno/Bun, or as a type-check-only step.

When Node is the runtime, three extra things matter beyond the base workflow:

### 1. Install Node's type definitions

```bash
npm install --save-dev @types/node
```

Without this, TypeScript cannot type-check Node's built-in modules (`fs`, `path`, `process`, `http`, …). The `@types/node` package is the Node caveat's most common omission.

### 2. Match tsconfig to Node's module system

Node interprets a file as ESM or CommonJS based on `package.json`. Keep the two consistent:

```json
// package.json — ESM
{ "type": "module" }
```

```json
// tsconfig.json — ESM for Node
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "outDir": "dist",
    "rootDir": "src"
  },
  "include": ["src"]
}
```

`module: "NodeNext"` makes TypeScript follow Node's native ESM/CommonJS rules. For a CommonJS project, omit `"type": "module"` and use `"module": "CommonJS"`.

### 3. Choose a run strategy

- **Compile then run** (always works):
  ```bash
  npm run build && node dist/index.js
  ```
- **Run directly with `tsx`** (modern, recommended for dev):
  ```bash
  npm install --save-dev tsx
  npx tsx src/index.ts
  ```
- **`ts-node`** (classic alternative): `npm install --save-dev ts-node` and `npx ts-node src/index.ts`.

### Node-only vs browser: the lib split

For a **Node-only** project, the standard library defaults are usually fine, but you can be explicit and exclude DOM types:

```json
"lib": ["ES2022"]
```

For a **browser** project, include the DOM instead and skip `@types/node`:

```json
"lib": ["ES2022", "DOM", "DOM.Iterable"]
```

## See Also

- [Rust project initialization](rust.md)
- [Project initialization overview](../../project-management/project-preparation/project-initializations-overview.md)
- [Next.js](../frameworks/nextjs.md) — a framework-level instantiation workflow built on top of a TypeScript base.
- [Vite + TypeScript (static frontend)](../frameworks/vite.md) — the browser/static-deployment variant.
