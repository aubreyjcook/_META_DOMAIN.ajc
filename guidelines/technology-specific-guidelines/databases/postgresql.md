# PostgreSQL

## Purpose

Workflow for initializing a PostgreSQL database for a project. PostgreSQL is a client-server relational database — it runs as a separate service your application connects to.

## Prerequisites

- A running **PostgreSQL** server.
- The **psql** client.

## Workflow

### 1. Connect as the superuser

```bash
psql -U postgres
```

### 2. Create the database and a dedicated user

```sql
CREATE DATABASE app;
CREATE USER app WITH PASSWORD 'secret';
GRANT ALL PRIVILEGES ON DATABASE app TO app;
```

### 3. Create schema and tables

Connect to the new database and create objects with SQL, or use a migration tool (see below).

### 4. Wire the application

Use a connection string (store it in an environment variable, never in code):

```
postgresql://app:secret@localhost:5432/app
```

### 5. Manage migrations

Choose a migration tool rather than hand-editing schema:

- **Node/TypeScript** — Prisma, Knex, Drizzle
- **Python** — Alembic
- **Java** — Flyway, Liquibase

## Notes

- Never commit credentials; read them from environment variables or a secrets file in `.gitignore`.
- Keep schema changes in versioned migrations.

## See Also

- [SQLite](sqlite.md) — the embedded, file-based alternative.
- [GraphQL](graphql.md) — a query layer that can sit in front of PostgreSQL.
