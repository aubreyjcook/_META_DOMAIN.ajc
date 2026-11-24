# SQLite

## Purpose

Workflow for initializing a SQLite database for a project. SQLite is an **embedded, file-based** relational database — there is no server to install or run; the whole database is a single file.

## Prerequisites

- The `sqlite3` CLI, or a language driver (SQLite is bundled with Python's standard library; Node uses the `sqlite3` package or the built-in `node:sqlite`).

## Workflow

### 1. Create the database file

```bash
sqlite3 app.db
```

Or simply let the application create the file on first connection.

### 2. Create schema

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL
);
```

### 3. Wire the application

- **Python**: `import sqlite3; sqlite3.connect("app.db")`
- **Node**: `npm install sqlite3`, then open `app.db`

### 4. Version the schema

Commit migration/SQL files; the `.db` file itself is usually gitignored.

## Notes

- Zero configuration, single writer — ideal for local development, embedded apps, and prototyping.
- Not suited to high-concurrency, multi-writer server workloads; graduate to [PostgreSQL](postgresql.md) for those.

## See Also

- [PostgreSQL](postgresql.md)
