# Nest

## Purpose

Workflow for initializing a Nest project in a blank repository. Nest is a TypeScript-first backend framework with a modular, Angular-inspired architecture.

## Prerequisites

- **Node.js**.

## Workflow

### 1. Install the Nest CLI

```bash
npm install -g @nestjs/cli
```

### 2. Scaffold the project

```bash
nest new my-app
```

### 3. Run

```bash
cd my-app
npm run start:dev    # dev server with hot reload
```

### 4. Structure

```plaintext
my-app/
├── src/
│   ├── main.ts
│   ├── app.module.ts
│   ├── app.controller.ts
│   └── app.service.ts
├── nest-cli.json
└── package.json
```

### 5. Generate modules and resources

```bash
nest generate module <name>
nest generate resource <name>     # controller + service + DTOs
```

## Notes

- Nest is **TypeScript**-based and uses decorators and dependency injection.
- Modules group controllers (routing) and services (business logic).

## See Also

- [Angular](angular.md) — the framework whose structure Nest mirrors.
- [TypeScript](../languages/typescript.md)
