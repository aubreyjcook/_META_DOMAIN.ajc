# Plain CSS

## Purpose

Workflow for using plain CSS in a project. CSS is a stylesheet language, not a programming language — it needs **no build step**, compiler, or package manager.

## Workflow

### 1. Create the stylesheet

```plaintext
project/
├── index.html
└── styles.css
```

### 2. Link it from HTML

```html
<link rel="stylesheet" href="styles.css">
```

### 3. Write rules

```css
:root {
  --color-brand: #0ea5e9;
}

.card {
  background: var(--color-brand);
  padding: 1rem;
}
```

### 4. Open in the browser

No server or build tooling is required; reload to see changes.

## Notes

- Understand the **cascade** and **specificity** — they govern which rules win.
- Modern CSS includes Grid, Flexbox, custom properties (variables), `:has()`, and container queries — most layout needs no framework.

## See Also

- [Tailwind CSS](tailwind.md) — a utility-first framework built on top of CSS.
