# Vite + TypeScript (Static Frontend)

## Purpose

Workflow for initializing an **entirely frontend** TypeScript application built with **Vite**, designed to be deployed to a fully static host such as **GitHub Pages**. There is no server runtime: the deployable artifact is a folder of static HTML, CSS, JavaScript, and assets.

Vite is a build tool and dev server (not a framework). This page covers the framework-free ("vanilla") TypeScript setup; the framework variants are documented in [React](react.md), [Vue](vue.md), and [Angular](angular.md).

## Prerequisites

- **Node.js** and **npm**.

## Workflow

### 1. Scaffold with create-vite

```bash
npm create vite@latest my-app -- --template vanilla-ts
```

`vanilla-ts` is the no-framework TypeScript template. To scaffold into the current directory instead, use `.` in place of `my-app`.

### 2. Install and run the dev server

```bash
cd my-app
npm install
npm run dev
```

### 3. Structure created

```plaintext
my-app/
├── index.html              # entry HTML; Vite injects the built scripts
├── src/
│   ├── main.ts             # TypeScript entry point
│   ├── style.css
│   └── vite-env.d.ts
├── public/                 # files copied verbatim to the build output
├── tsconfig.json
├── package.json
└── .gitignore              # ignores node_modules/ and dist/
```

### 4. Configure the base path for static hosting

This is the single most common gotcha. GitHub Pages serves **project sites** from `https://<user>.github.io/<repo>/`, so Vite must prefix asset URLs with the repo name. Create or edit `vite.config.ts` at the project root:

```ts
import { defineConfig } from 'vite'

export default defineConfig({
  base: '/<repo-name>/',
})
```

- **Project site** (`user.github.io/repo-name/`) → `base: '/repo-name/'`
- **User/organization site** (`user.github.io/`) → `base: '/'`
- **Custom domain** → `base: '/'`

### 5. Build

```bash
npm run build
```

The production build is emitted to `dist/`. That folder — not the source tree — is what gets deployed.

### 6. Test the production build locally

```bash
npm run preview
```

This serves `dist/` so you can verify the built output (including the base path) before deploying.

### 7. Deploy to GitHub Pages

Enable Pages on the repo and add a GitHub Actions workflow that builds and publishes `dist/`. Example `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run build
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

Set the Pages source to **GitHub Actions** (Settings → Pages), not the `gh-pages` branch.

## Notes

- `dist/` and `node_modules/` are gitignored by the scaffold; **commit** `package-lock.json`.
- The `base` path must match the deployed URL path, or the built assets 404.
- Static hosts do no server-side routing: for client-side routing (if you later add a router), use hash routing or a `404.html` redirect trick.
- Environment variables prefixed with `VITE_` are exposed to client code at build time (e.g. `import.meta.env.VITE_API_URL`).

## See Also

- [Vite + TypeScript: setup, configuration, and deployment](vite-setup-configuration-deployment.md) — the companion covering what comes after initialization.
- [TypeScript](../languages/typescript.md) — the language setup, including the Node.js runtime caveat.
- [React](react.md) and [Vue](vue.md) — the Vite-based framework variants.
- [Vite + Rust (WebAssembly)](vite-rust.md) — the same static-deployment workflow with the logic written in Rust.
- [Tailwind CSS](../styling/tailwind.md) — add a utility-first styling layer via PostCSS.
