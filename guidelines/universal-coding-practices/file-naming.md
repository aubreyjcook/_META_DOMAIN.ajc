# File Naming Guidelines

## Purpose

How files are named in this repository, and a lighter convention for other repos. Names should be readable, consistent, and safe on case-sensitive filesystems.

## This repository

1. **Lowercase** for files and directories.
2. **Hyphens** between words (`file-naming.md`, not `file_naming.md` or `FileNaming.md`).
3. **No spaces** in paths (`web-server`, not `web server`).
4. **Overview files** for a directory: `{topic}-overview.md` sitting in that directory.
5. **Note files**: `{topic}.md` without a `guideline-` or `note-` prefix. The folder already says what kind of page it is.
6. **README.md** only at a repo or example-project root.
7. Dates, when needed, use `YYYY-MM-DD`.

Examples in this repo:

- `guidelines/universal-coding-practices/file-naming.md`
- `notes/personal-observations/hyper-minimalization.md`
- `templates/projects/project-000/README.md`

## Other repositories

Keep the same lowercase-hyphen rule. Prefixes are optional when a mixed folder needs them:

- Source: `{module}-{functionality}.ext` — `user-authentication.js`
- Config: `config-{purpose}.ext` — `config-database.yml`
- Scripts: `script-{purpose}.ext` — `script-deploy.sh`
- Tests: `test-{module}-{functionality}.ext`
- Images: `img-{description}.{ext}`

Repo-level names (prefixes such as `dev_`, `sandbox_`, `_META_`) are documented in [tagging notation](../source-control/git/tagging-notation.md), not here.
