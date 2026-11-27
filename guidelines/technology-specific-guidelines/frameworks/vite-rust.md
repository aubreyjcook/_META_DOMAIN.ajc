# Vite + Rust (WebAssembly, Static Frontend)

## Purpose

Workflow for initializing a frontend application whose logic is written in **Rust** and compiled to **WebAssembly (WASM)**, then served as a static site on a host like **GitHub Pages**. The shell is Vite (with a thin TypeScript entry point), while the actual "program" — computation, algorithms, data processing — lives in Rust and is exposed to JavaScript through `wasm-bindgen`.

This mirrors the [Vite + TypeScript](vite.md) workflow, with the Rust crate replacing the TypeScript `src/` logic.

## Prerequisites

- **Node.js** and **npm**.
- **Rust** toolchain (via rustup), the `wasm32-unknown-unknown` target, and **wasm-pack**.

## Workflow

### 1. Install the WASM toolchain

```bash
rustup target add wasm32-unknown-unknown
cargo install wasm-pack
```

### 2. Scaffold the Vite frontend

```bash
npm create vite@latest my-app -- --template vanilla-ts
cd my-app
npm install
```

### 3. Create the Rust library crate

```bash
cargo new --lib wasmcore
```

Configure `wasmcore/Cargo.toml` to build a WASM-compatible dynamic library:

```toml
[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2"
```

### 4. Write the Rust logic

In `wasmcore/src/lib.rs`, annotate any function you want to call from JS with `#[wasm_bindgen]`:

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

### 5. Compile Rust to WASM

```bash
wasm-pack build wasmcore --target web
```

This emits `wasmcore/pkg/` containing the `.wasm` binary, the generated JS bindings, and TypeScript `.d.ts` declarations.

### 6. Import and call it from the frontend

Install the generated package locally:

```bash
npm install ./wasmcore/pkg
```

Then, in `src/main.ts`:

```ts
import init, { add } from 'wasmcore'

await init()
console.log(add(2, 3)) // 5
```

The generated bindings load the `.wasm` asynchronously, so `init()` must be awaited before any function is called. If Vite reports errors about WASM or top-level await, add `vite-plugin-wasm` and `vite-plugin-top-level-await` to `vite.config.ts`.

### 7. Configure the base path, build, and deploy

The deployment steps are identical to the TypeScript workflow:

1. Set the GitHub Pages base path in `vite.config.ts`:
   ```ts
   import { defineConfig } from 'vite'
   export default defineConfig({ base: '/<repo-name>/' })
   ```
2. Build: `npm run build` (emits `dist/`).
3. Deploy with GitHub Actions (below), which additionally compiles the Rust crate.

### GitHub Actions

`.github/workflows/deploy.yml`:

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
      - uses: dtolnay/rust-toolchain@stable
        with:
          targets: wasm32-unknown-unknown
      - run: cargo install wasm-pack
      - run: npm ci
      - run: wasm-pack build wasmcore --target web
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

## Notes

- The `.wasm` binary is the compiled Rust; the frontend never contains the Rust source logic — it only calls into it via the generated bindings.
- `wasm-pack --target web` is the most Vite-friendly output (the bindings fetch the `.wasm` themselves). Use `--target bundler` only when a bundler manages the WASM import directly.
- For richer JavaScript/DOM interop, add the `js-sys` and `web-sys` crates alongside `wasm-bindgen`.
- Commit `package-lock.json`; ignore `node_modules/`, `dist/`, and the Rust `target/` directory (plus `wasmcore/pkg/` if you rebuild it in CI rather than committing it).

## See Also

- [Vite + Rust (WASM): setup, configuration, and deployment](vite-rust-setup-configuration-deployment.md) — the companion covering what comes after initialization.
- [Vite + TypeScript (static frontend)](vite.md) — the TypeScript-only variant of this workflow.
- [Rust](../languages/rust.md) — native (non-WASM) Rust project initialization.
- [TypeScript](../languages/typescript.md)
