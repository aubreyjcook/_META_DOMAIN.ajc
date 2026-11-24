# Next.js

## Overview

Next.js is a React framework that adds routing, server-side rendering (SSR), static site generation (SSG), and API routes on top of React.

## Initialization Workflow

### Recommended: create-next-app

```bash
npx create-next-app@latest my-app
```

The scaffold prompts for TypeScript, ESLint, Tailwind CSS, and the App Router (vs the legacy Pages Router), then generates a fully configured project:

```bash
cd my-app
npm run dev
```

### Manual installation

For full control, build the project piece by piece:

1. `npm init -y`
2. `npm install next react react-dom`
3. Add scripts to `package.json`:
   ```json
   {
     "scripts": {
       "dev": "next dev",
       "build": "next build",
       "start": "next start",
       "lint": "next lint"
     }
   }
   ```
4. Create the `app/` (App Router) or `pages/` (Pages Router) directory.

### Other starters

- **create-t3-app** — type-safe full-stack starter (TypeScript + tRPC + Prisma + NextAuth).
- **shadcn/ui** — add pre-built accessible components on top of a Next + Tailwind project.
- **create-turbo** — a monorepo starter that can include a Next.js app.

## Project-specific Stacks

While Next.js itself is based on a specific stack, the nature of Next allows it to be used with various pseudo-stacks of its own. This is due to the wide compatibility of many frameworks and libraries that have been built to be used with Next.js and other site generators like it. This also has to do with the relative portability of Next instances themselves, which can be hosted or configured to work with many custom platforms.

## See Also

- [React](react.md)
- [TypeScript](../languages/typescript.md)
- [Tailwind CSS](../styling/tailwind.md)
