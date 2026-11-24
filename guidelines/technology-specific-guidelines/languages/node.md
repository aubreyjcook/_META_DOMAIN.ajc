# Node.js

## Purpose

Workflow for initializing a Node.js (server or CLI) project in a blank repository. Node is the JavaScript runtime; this is the backend counterpart to the browser-focused [vanilla JavaScript](javascript.md) notes.

## Prerequisites

- **Node.js** and **npm**.

## Workflow

### 1. Initialize the package

```bash
npm init -y
```

### 2. Choose the module system

- **ESM** — add `"type": "module"` to `package.json`.
- **CommonJS** — the default; leave it unset.

### 3. Create the entry file

```plaintext
project/
├── src/
│   └── index.js      # index.mjs or index.cjs to force a module system
├── package.json
└── .gitignore
```

### 4. Configure scripts

```json
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js"
  }
}
```

`node --watch` (built into Node 18+) replaces `nodemon` for restart-on-change during development.

### 5. Install dependencies

```bash
npm install <package>          # runtime dependency
npm install -D <package>       # development-only dependency
```

### 6. Run

```bash
npm start       # or: npm run dev
```

## Notes

- `node_modules/` belongs in `.gitignore`; **commit** `package-lock.json`.
- For TypeScript on Node, see [TypeScript](typescript.md) — it adds a build step and `@types/node`.

## See Also

- [Vanilla JavaScript](javascript.md)
- [TypeScript](typescript.md)
