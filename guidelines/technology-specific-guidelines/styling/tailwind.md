# Tailwind CSS

## Purpose

Workflow for initializing Tailwind CSS, a utility-first CSS framework, in a blank repository.

## Prerequisites

- **Node.js**.

## Workflow (Tailwind v4)

### 1. Install

```bash
npm install tailwindcss @tailwindcss/cli
```

### 2. Create the input stylesheet

```css
@import "tailwindcss";
```

### 3. Build

```bash
npx @tailwindcss/cli -i input.css -o output.css --watch
```

### 4. Link the output

```html
<link rel="stylesheet" href="output.css">
```

## Tailwind v3 alternative

```bash
npm install -D tailwindcss
npx tailwindcss init
```

Use the `@tailwind base/components/utilities` directives and configure `tailwind.config.js`; wire it through PostCSS.

## With a bundler

With Vite or Next.js, integrate Tailwind through PostCSS rather than the standalone CLI. `create-next-app` offers Tailwind directly during scaffolding.

## Notes

- v4 is configuration-less by default; v3 uses a `tailwind.config.js`.
- Tailwind generates utility classes at build time; commit the source CSS, not the generated output.

## See Also

- [Plain CSS](css.md)
- [Next.js](../frameworks/nextjs.md)
