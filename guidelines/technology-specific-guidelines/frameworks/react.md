# React

## Purpose

Workflow for initializing a React project in a blank repository. Recommended tool: **Vite** (fast, modern). `create-react-app` is deprecated and no longer recommended.

## Prerequisites

- **Node.js**.

## Workflow (Vite)

### 1. Scaffold the project

```bash
npm create vite@latest my-app -- --template react
# TypeScript variant:
# npm create vite@latest my-app -- --template react-ts
```

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
│   ├── main.jsx        # entry point
│   ├── App.jsx
│   └── components/
├── package.json
└── vite.config.js
```

### 4. Build

```bash
npm run build      # output to dist/
npm run preview    # preview the production build locally
```

## Notes

- React uses **JSX** and functional components with hooks.
- For a full-stack React framework with routing and server rendering, see [Next.js](nextjs.md).

## See Also

- [Next.js](nextjs.md)
- [TypeScript](../languages/typescript.md)
