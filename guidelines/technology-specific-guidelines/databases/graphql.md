# GraphQL

## Purpose

Workflow for initializing a GraphQL data layer. Note: **GraphQL is a query language for APIs, not a database** — it defines a schema and resolvers that fetch from any backend (PostgreSQL, SQLite, REST, etc.). It is grouped under databases because it is a common data-access layer.

## Workflow (Apollo Server, Node)

### 1. Install the server

```bash
npm init -y
npm install @apollo/server graphql
```

### 2. Define the schema (SDL)

```graphql
type Query {
  hello: String
}
```

### 3. Write resolvers

```js
const resolvers = {
  Query: {
    hello: () => "Hello world",
  },
};
```

### 4. Start the server

Create the Apollo Server, wire it to an HTTP server, and query `POST /graphql`.

## Notes

- The schema (type definitions) and resolvers together define the API; the resolvers are where the actual database access happens.
- Alternative servers: **GraphQL Yoga**, **Nest** (GraphQL module), or a **Next.js** route handler.

## See Also

- [PostgreSQL](postgresql.md)
- [SQLite](sqlite.md)
- [Nest](../frameworks/nest.md)
