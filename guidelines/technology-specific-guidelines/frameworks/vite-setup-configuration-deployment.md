# Vite + TypeScript: Setup, Configuration, and Deployment

## Purpose

Companion to [Vite + TypeScript initialization](vite.md): the steps that come *after* scaffolding. It covers the development environment setup, `vite.config.ts` and `tsconfig.json` configuration, environment variables, and deploying the built `dist/` to static hosts and CI. Initialization — scaffolding with `create-vite`, installing, and the first `npm run dev` — lives in [vite.md](vite.md).

## Prerequisites

- A Vite + TypeScript project scaffolded per [vite.md](vite.md).
- **Node.js** — Vite 5/6 require Node 18+; Vite 7 requires Node 20.19+ or 22.12+.

## Setup

### Pin the Node version

Record the Node version so every machine and CI uses the same one. Add an `.nvmrc` and/or an `engines` field:

```text
# .nvmrc
20
```

```json
// package.json
{ "engines": { "node": ">=20.19.0" } }
```

### Editor and formatting

- **VS Code** plus the ESLint and Prettier extensions; enable format-on-save via `.vscode/settings.json`.
- An `.editorconfig` for consistent indentation and line endings.

### Lint and format (optional)

`create-vite` does not scaffold ESLint or Prettier. Add them explicitly if the project needs them:

```bash
npm install -D eslint @eslint/js typescript-eslint prettier eslint-config-prettier
```

## Configuration

### vite.config.ts

All Vite options live in one file. The common ones:

```ts
import { defineConfig } from 'vite'
import { fileURLToPath, URL } from 'node:url'

export default defineConfig({
  base: '/<repo-name>/',             // asset prefix — see vite.md
  plugins: [],                        // framework, WASM, or PostCSS plugins
  resolve: {
    alias: { '@': fileURLToPath(new URL('./src', import.meta.url)) },
  },
  server: {
    port: 5173,
    host: true,                       // expose on the LAN
    open: true,                       // open the browser on start
    proxy: {                          // forward /api to a backend; avoids CORS in dev
      '/api': 'http://localhost:3000',
    },
  },
  build: {
    outDir: 'dist',
    sourcemap: true,                  // source maps for production error tracking
    target: 'es2020',
  },
})
```

### Path aliases: keep tsconfig and Vite in sync

Aliases must be declared in *both* the TypeScript config (for the editor/type-checker) and Vite (for the bundler), or the two will disagree. In the `vanilla-ts` scaffold the alias belongs in `tsconfig.app.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["src/*"] }
  }
}
```

Alternatively use the [`vite-tsconfig-paths`](https://github.com/aleclarson/vite-tsconfig-paths) plugin to read aliases from `tsconfig.json` automatically.

### TypeScript type-checking

Vite transpiles with esbuild and **does not type-check** on its own. The scaffold's `build` script runs `tsc` first; keep it, and run a fast check in CI:

```json
{
  "scripts": {
    "build": "tsc && vite build",
    "typecheck": "tsc --noEmit"
  }
}
```

`vite build` alone never catches a type error — always keep a `tsc` step.

### Environment variables and modes

Vite loads `.env` files in this order: `.env`, `.env.local`, `.env.[mode]`, `.env.[mode].local`. Only variables prefixed with `VITE_` are exposed to client code:

```text
# .env
VITE_API_URL=https://api.example.com
```

```ts
const api = import.meta.env.VITE_API_URL
import.meta.env.MODE        // 'development' | 'production' | custom mode
import.meta.env.DEV         // boolean
import.meta.env.PROD        // boolean
import.meta.env.BASE_URL    // the `base` value
```

- Custom modes: `vite build --mode staging` loads `.env.staging`.
- Values are inlined at build time — they are not runtime reads of `process.env`.
- Type the variables in `src/vite-env.d.ts`:

```ts
/// <reference types="vite/client" />
interface ImportMetaEnv {
  readonly VITE_API_URL: string
}
interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

## Deployment

### Build and preview

```bash
npm run build      # emits dist/
npm run preview    # serve dist/ locally to verify the production output
```

The deployable artifact is the `dist/` folder, not the source tree.

### GitHub Pages

Covered in [vite.md](vite.md): set `base` to the repo path and use the GitHub Actions workflow there.

### Netlify

Build command `npm run build`, publish directory `dist`. For client-side routing, add a `_redirects` file in `public/` (copied into `dist/`):

```text
/*  /index.html  200
```

### Vercel

Framework preset **Vite**, build command `npm run build`, output directory `dist`. Add SPA rewrites in `vercel.json`:

```json
{ "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }] }
```

### Cloudflare Pages

Build command `npm run build`, output directory `dist`, and enable the **SPA fallback** (or use a `_redirects` file with `/* /index.html 200`).

### SPA routing fallback

A static host has no server-side routing, so a client-side router needs unknown paths to fall back to `index.html`. GitHub Pages: copy `index.html` to `404.html` (or use hash routing). Netlify/Vercel/Cloudflare: the redirect rules above.

### Self-hosting (nginx + Docker)

```nginx
server {
  listen 80;
  root /usr/share/nginx/html;
  index index.html;
  location / {
    try_files $uri $uri/ /index.html;
  }
  location /assets/ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }
}
```

Multi-stage Dockerfile:

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

### CI

Type-check and build as separate steps; inject `VITE_` secrets as environment variables on the build step (they are baked into the bundle):

```yaml
- uses: actions/setup-node@v4
  with: { node-version: 20, cache: npm }
- run: npm ci
- run: npm run typecheck
- run: npm run build
  env:
    VITE_API_URL: ${{ secrets.VITE_API_URL }}
```

## Notes

- `vite build` does not type-check; keep the `tsc` step in the build script and CI.
- Hashed asset filenames let you cache `/assets/` aggressively (`Cache-Control: immutable`).
- Only `VITE_`-prefixed env vars reach the browser; everything else stays server-side.

## See Also

- [Vite + TypeScript initialization](vite.md)
- [Vite + Rust (WASM): setup, configuration, and deployment](vite-rust-setup-configuration-deployment.md)
- [TypeScript](../languages/typescript.md)
- [Tailwind CSS](../styling/tailwind.md)
