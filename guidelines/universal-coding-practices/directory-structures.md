# Directory Structures

A consistent layout makes files easier to find. This page records the pattern used here and a small example for a typical project. Language-specific layouts can be added as separate files when they exist.

## This repository

```plaintext
_META_DOMAIN.ajc/
├── README.md
├── guidelines/
│   ├── plain-text-notation.md
│   ├── open-source-licensing/
│   ├── project-management/
│   ├── source-control/git/
│   ├── technology-specific-guidelines/
│   └── universal-coding-practices/
├── notes/
├── templates/
├── llm/
└── project-planning/
```

Each subject directory has an overview markdown file that lists its children.

## Typical code project (example)

```plaintext
project-name/
├── README.md
├── .gitignore
├── src/
├── tests/
└── docs/
```

Python-oriented example used in the [project template](../../templates/projects/project-000/README.md).

[Back to universal coding practices](universal-coding-practices-overview.md)
