# Vanilla JavaScript

## Purpose

Workflow for initializing a plain JavaScript project — no framework, no bundler, no build step. This is the baseline that every other JavaScript toolchain (Node, React, Vue, Angular) is built on top of.

## Prerequisites

- **Node.js** to run scripts and use npm. For browser-only code, a browser alone is enough.

## Workflow

### 1. Initialize the package (optional)

```bash
npm init -y
```

If you are writing a single browser script with no dependencies and no npm scripts, skip this and just create the `.js` file.

### 2. Create the entry file

```plaintext
project/
└── index.js        # or src/index.js
```

### 3. Choose the runtime

**Browser** — link the script from HTML:

```html
<script src="index.js"></script>
<!-- or, for ES modules: -->
<script type="module" src="index.js"></script>
```

**Node** — run directly:

```bash
node index.js
```

### 4. Use modules

- **ES modules** (`import` / `export`) work in browsers with `type="module"`, and in Node when `package.json` has `"type": "module"` (or the file is `.mjs`).
- **CommonJS** (`require` / `module.exports`) is Node's legacy default (`.cjs`, or no `"type": "module"`).

## Notes

- "Vanilla" means no framework, no TypeScript, no bundler — one source of truth for behavior.
- When you need a build step or type safety, graduate to [TypeScript](typescript.md) or a framework such as [React](../frameworks/react.md).

## See Also

- [Node.js](node.md) — the JavaScript runtime for server/CLI projects.
- [TypeScript](typescript.md)
- [React](../frameworks/react.md)
