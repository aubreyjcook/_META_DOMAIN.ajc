# Guidelines for Initializing Coding Projects

## Project Initialization Overview

Initializing a project is the process of turning an empty repository into a working, documented starting point. Done deliberately up front, it avoids the harder job of retrofitting documentation and structure later — the [documentation practices](../../universal-coding-practices/documentation-practices.md) note observes it is "generally better to do the former and avoid the latter."

These guidelines cover the whole setup, especially for projects on GitHub: naming, description, directory structure, and the initial documentation that should exist before (or alongside) the first line of code.

## Project Initialization Checklist

Run through these in order when starting a new project:

1. **Name the repository** — apply the prefix/suffix tagging convention (see [Project Naming](#project-naming)).
2. **Create the repository** — on GitHub (or wherever it will live) and clone it locally.
3. **Write the initial README** — use the [project init README template](../../../templates/project-init-readme.md).
4. **Set up the directory structure** — follow [directory structures](../../universal-coding-practices/directory-structures.md).
5. **Initialize the toolchain** — run the language-specific workflow for the stack; these live under [technology-specific guidelines](../../technology-specific-guidelines/technology-specific-guidelines-overview.md) (JavaScript, TypeScript, PHP, Python, Rust, Java, C++, React, Next.js, and others).
6. **Initialize version control** — add a `.gitignore`, make the first commit, and follow the [commit message convention](../../source-control/git/commit-messages.md).
7. **Record initial documentation** — problem/solution statement, pseudo-code, and a config-init log (see [Project Initial Documentation](#project-initial-documentation)).

## Project Naming

### Repository name

Use the prefix/suffix tagging system in [tagging notation](../../source-control/git/tagging-notation.md) — `dev_`, `sandbox_`, `app_`, `lib_`, `index_`, `_META_`, `_DOMAIN_`, and so on — with the `.ajc` suffix for personal projects. The prefix communicates the project's intent and phase at a glance.

### Files and directories

Once inside the repo, follow [file naming](../../universal-coding-practices/file-naming.md): lowercase, hyphens between words, no spaces.

## Project Description

A project needs a clear description before code exists:

1. **Problem and solution statement** — define what problem the project solves and how, using the [problem-solution template](../../../templates/problem-solution-statement.md).
2. **Purpose statement** — a one-to-two-sentence summary at the top of the README.

The description is what turns "a repository of code" into "a project with intent."

## Project Directory Structure

Start from the general layout in [directory structures](../../universal-coding-practices/directory-structures.md):

```plaintext
project-name/
├── README.md
├── .gitignore
├── src/
├── tests/
└── docs/
```

Language-specific layouts (Maven's `src/main/java`, Python's `src/<package>/`, Cargo's `src/` + `Cargo.toml`, and so on) are documented in the individual workflows under [technology-specific guidelines](../../technology-specific-guidelines/technology-specific-guidelines-overview.md).

## Project Initial Documentation

The initial documentation should exist before, or at least alongside, the first code.

### Main Readme

Use the [project init README template](../../../templates/project-init-readme.md). The minimal README contains:

- Project name
- Purpose statement
- Todos
- Pseudo-code (optional)
- Pseudo-directory-structure (optional)
- Config-init log

### Problem and Solution Statement

For projects that solve a defined problem, fill out the [problem-solution template](../../../templates/problem-solution-statement.md) and link it from the README. The template captures the problem, its impact, the proposed solution, and the solution's objectives.

### Pseudo-Code

Outline the main logic before implementation. See [pseudo-code templates](pseudo-code/pseudo-code-templates.md) for the non-language-specific format.

### Config-Init Log

Log the initial configuration steps — especially CLI commands such as package installs and toolchain setup — so the setup is reproducible. This section lives in the README template as the "Config-Init Log".

### ChatGPT / LLM Initial Project Outline Discussion

It's typical that a project starts with a discussion or outline of the project's scope, goals, and initial tasks. This discussion can be carried out with ChatGPT, other LLMs, or team members to brainstorm ideas and outline the project's initial structure. An additional initial documentation file should be included alongside the main readme that logs this information. The log should be formatted to be as clear and concise as possible, outlining the key points discussed and the initial project structure.
