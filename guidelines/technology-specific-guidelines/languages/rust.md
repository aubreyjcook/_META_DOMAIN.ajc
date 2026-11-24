# Rust Project Initialization

## Purpose

A clear workflow for initializing and configuring a Rust project inside a blank repository. Unlike TypeScript, Rust bundles its package manager, build system, and dependency resolution into one tool: **Cargo**.

## Prerequisites

- The Rust toolchain installed via **rustup**, which provides `rustc` (compiler), `cargo` (build system/package manager), and optionally `rustfmt`, `clippy`, and `rust-analyzer`.

## Workflow

### 1. Initialize the crate

Run from inside the blank repo:

```bash
cargo init            # binary (application) crate — the default
cargo init --lib      # library crate
```

`cargo init` scaffolds into the *current* directory. (Use `cargo new <name>` instead to create a new directory.) Use `--name <name>` if the crate name should differ from the directory name.

### 2. Structure created

```plaintext
project/
├── Cargo.toml
├── .gitignore          # cargo init creates this, ignoring /target
└── src/
    └── main.rs         # or lib.rs for --lib
```

`cargo init` already writes a `.gitignore` that ignores the `target/` build directory.

### 3. Configure Cargo.toml

```toml
[package]
name = "project"
version = "0.1.0"
edition = "2021"

[dependencies]
# add crates here
```

- `edition` sets the language edition (`2021` is the stable default; `2024` is the newest). Set it explicitly so builds are reproducible across toolchains.
- Add dependencies with `cargo add <crate>` (e.g. `cargo add serde`) rather than editing `Cargo.toml` by hand.

### 4. Build and run

```bash
cargo check             # fast type-check, no binary output — great in an editor loop
cargo build             # debug build -> target/debug/
cargo run               # build + run the binary
cargo test              # run the test suite
```

### 5. Release build

```bash
cargo build --release   # optimized build -> target/release/
```

Debug builds are fast to compile but unoptimized; release builds apply optimizations and are what you ship or benchmark.

### 6. Tooling

Rust's linter and formatter are separate rustup components:

```bash
rustup component add rustfmt clippy   # if not already installed
cargo fmt                             # format the code
cargo clippy                          # lint the code
```

The IDE/editor integration is **rust-analyzer** (the language server), which powers completion, type info, and diagnostics in VS Code and others.

### 7. Gitignore

`cargo init` handles it, but the essential line is:

```gitignore
/target
```

`target/` is fully derived from source and `Cargo.lock`, so it should never be committed. Note: for *binary* crates, **commit `Cargo.lock`** (it pins exact dependency versions); for *library* crates, it is conventional to leave it out.

## See Also

- [TypeScript project initialization](typescript.md)
- [Project initialization overview](../../project-management/project-preparation/project-initializations-overview.md)
