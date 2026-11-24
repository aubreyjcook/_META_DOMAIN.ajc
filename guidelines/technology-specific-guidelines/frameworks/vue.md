# Vue

## Purpose

Workflow for initializing a Vue project in a blank repository. Recommended tool: the official **create-vue** scaffold (built on Vite).

## Prerequisites

- **Node.js**.

## Workflow

### 1. Scaffold the project

```bash
npm create vue@latest
# or the plain Vite template:
# npm create vite@latest my-app -- --template vue
```

The official scaffold prompts for optional features (TypeScript, Vue Router, Pinia, etc.).

### 2. Install and run

```bash
cd my-app
npm install
npm run dev
```

### 3. Structure

```plaintext
my-app/
├── index.html
├── src/
│   ├── main.js
│   ├── App.vue
│   └── components/
├── package.json
└── vite.config.js
```

### 4. Build

```bash
npm run build      # output to dist/
```

## Notes

- The **Composition API** with `<script setup>` is the current default style.
- **Vite** is the standard bundler; Vue Router and Pinia are the standard routing/state libraries.

## See Also

- [React](react.md)
- [Next.js](nextjs.md)
