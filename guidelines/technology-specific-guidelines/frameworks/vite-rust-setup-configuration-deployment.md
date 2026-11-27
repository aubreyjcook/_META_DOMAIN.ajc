# Vite + Rust (WebAssembly): Setup, Configuration, and Deployment

## Purpose

Companion to [Vite + Rust initialization](vite-rust.md): the steps that come *after* scaffolding the Vite shell and the Rust crate. It covers the WASM toolchain setup, configuring and shrinking the compiled `.wasm`, and deploying the static site with CI that compiles the Rust crate. Shared Vite configuration (aliases, env vars, dev server) and hosting specifics are documented in the [TypeScript companion](vite-setup-configuration-deployment.md).

## Prerequisites

- A project scaffolded per [vite-rust.md](vite-rust.md).
- The Rust toolchain, the `wasm32-unknown-unknown` target, and **wasm-pack**.

## Setup

### Toolchain

```bash
rustup target add wasm32-unknown-unknown
cargo install wasm-pack
```

Keep `wasm-pack` up to date: it bundles a `wasm-bindgen-cli` that must match the `wasm-bindgen` version in `Cargo.toml` (see the note under Deployment).

### Optional size tooling: wasm-opt

`wasm-opt` (from Binaryen) applies an extra optimization/shrink pass to the compiled `.wasm`. Install Binaryen via your package manager (`apt install binaryen`, `brew install binaryen`, `choco install binaryen`) or `cargo install wasm-opt`.

### Readable panic messages

By default a Rust panic in WASM prints an opaque message. Add `console_error_panic_hook`:

```toml
[dependencies]
console_error_panic_hook = "0.1"
```

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen(start)]
pub fn start() {
    console_error_panic_hook::set_once();
}
```

(It can also be called from JS right after `await init()`.)

## Configuration

### Cargo.toml: cdylib output

```toml
[lib]
crate-type = ["cdylib", "rlib"]   # cdylib builds the .wasm; rlib keeps `cargo test` working

[dependencies]
wasm-bindgen = "0.2"
```

### Shrink the .wasm

An optimized release profile is the biggest lever on `.wasm` size:

```toml
[profile.release]
opt-level = "s"          # size-optimized; "z" is more aggressive
lto = true
codegen-units = 1
panic = "abort"          # smaller; pair with console_error_panic_hook for messages
strip = true
```

### wasm-pack targets

```bash
wasm-pack build wasmcore --target web --release
```

- `--target web` — bindings fetch the `.wasm` themselves; the most Vite-friendly.
- `--target bundler` — for a bundler that resolves the WASM import.
- `--target nodejs` / `--target no-modules` — Node or non-ESM environments.

### Wire the build into package.json

```json
{
  "scripts": {
    "build": "wasm-pack build wasmcore --target web --release && tsc && vite build",
    "build:wasm": "wasm-pack build wasmcore --target web --release"
  }
}
```

Compile Rust *before* Vite so the bundler can consume the generated `pkg/` bindings.

### Optional wasm-opt pass

```bash
wasm-opt -Oz wasmcore/pkg/wasmcore_bg.wasm -o wasmcore/pkg/wasmcore_bg.wasm
```

### Vite WASM plugins

If Vite errors on WASM imports or top-level `await`, add `vite-plugin-wasm` and `vite-plugin-top-level-await` to `vite.config.ts`.

## Deployment

### Build order

Rust first, then Vite. The generated `pkg/` must exist before `vite build` bundles it — the `build` script above encodes that order.

### GitHub Pages CI

[vite-rust.md](vite-rust.md) has the complete workflow. The additions beyond the TypeScript workflow are the Rust toolchain, target, and a Cargo cache:

```yaml
- uses: dtolnay/rust-toolchain@stable
  with:
    targets: wasm32-unknown-unknown
- uses: Swatinem/rust-cache@v2
- run: cargo install wasm-pack
- run: wasm-pack build wasmcore --target web --release
```

### Serve .wasm with the correct MIME type

The `.wasm` binary must be served as `application/wasm`. GitHub Pages, Netlify, Vercel, and Cloudflare set this automatically. Self-hosted servers may not:

```nginx
# nginx — add to the http or server context
types {
  application/wasm wasm;
}
```

```apache
# Apache .htaccess
AddType application/wasm .wasm
```

### wasm-bindgen version match

The `wasm-bindgen` version in `Cargo.toml` must match the `wasm-bindgen-cli` that `wasm-pack` bundles. A mismatch fails the build with a version error — update `wasm-pack`, or pin `wasm-bindgen` to the version your `wasm-pack` supports.

## Notes

- Commit `Cargo.lock` (binary crate) and `package-lock.json`; ignore `target/`, `node_modules/`, `dist/`, and `wasmcore/pkg/` when you rebuild it in CI.
- The `.wasm` is fetched at runtime by the generated bindings, so it must be reachable and correctly MIME-typed like any other asset.
- `console_error_panic_hook` makes runtime panics readable in the browser console instead of "unreachable".

## See Also

- [Vite + Rust initialization](vite-rust.md)
- [Vite + TypeScript: setup, configuration, and deployment](vite-setup-configuration-deployment.md)
- [Rust](../languages/rust.md)
