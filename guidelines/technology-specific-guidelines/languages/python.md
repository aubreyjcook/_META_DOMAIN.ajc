# Python

## Purpose

Workflow for initializing a Python project in a blank repository.

## Prerequisites

- **Python 3** and **pip**. Optionally `uv` or **Poetry** as a faster/modern alternative.

## Workflow

### 1. Create and activate a virtual environment

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
```

### 2. Declare the project

Create a `pyproject.toml` (the modern standard) or a `requirements.txt`.

```toml
[project]
name = "app"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = []
```

### 3. Create the package

```plaintext
project/
├── src/
│   └── app/
│       ├── __init__.py
│       └── main.py
├── pyproject.toml
└── .gitignore
```

### 4. Install dependencies

```bash
pip install <package>
pip freeze > requirements.txt   # if using requirements.txt instead of pyproject.toml
```

### 5. Run

```bash
python -m app.main        # run as a module
# or
python src/app/main.py
```

## Notes

- `.venv/` belongs in `.gitignore`.
- Modern alternatives: `uv init` + `uv add <package>`, or `poetry new app` + `poetry add <package>`.

## See Also

- [TypeScript](typescript.md) — the same "blank repo → running project" pattern in another language.
