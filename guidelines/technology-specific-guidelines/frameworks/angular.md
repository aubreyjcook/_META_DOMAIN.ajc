# Angular

## Purpose

Workflow for initializing an Angular project in a blank repository. Angular is a TypeScript-first, batteries-included framework with its own CLI.

## Prerequisites

- **Node.js**.

## Workflow

### 1. Install the Angular CLI

```bash
npm install -g @angular/cli
```

### 2. Scaffold the project

```bash
ng new my-app
```

The command prompts for routing and a stylesheet format (CSS, SCSS, etc.).

### 3. Serve

```bash
cd my-app
ng serve            # dev server at http://localhost:4200
```

### 4. Structure

```plaintext
my-app/
├── angular.json     # CLI/workspace config
├── src/
│   ├── main.ts
│   ├── index.html
│   └── app/
│       ├── app.component.ts
│       └── app.module.ts   # or standalone components
```

### 5. Generate and build

```bash
ng generate component <name>
ng build            # output to dist/
```

## Notes

- Angular is **TypeScript by default**; standalone components are the current recommended style.
- Configuration lives in `angular.json`, not per-file configs.

## See Also

- [Nest](nest.md) — a backend framework inspired by Angular's structure.
