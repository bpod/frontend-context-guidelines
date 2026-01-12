---
description: "CSS best practices for organization, naming conventions, preprocessors, scoping, and maintainable stylesheets"
applyTo: "**/*.{css,scss,sass,less,styled.ts,styled.js}"
---

# CSS Best Practices

Guidelines for writing maintainable, scalable, and performant CSS across modern frontend applications.

## General Instructions

- Use consistent naming conventions (BEM, SMACSS, or utility-first)
- Organize styles logically by component or feature
- Minimize specificity conflicts and cascading issues
- Use CSS custom properties for theming and design tokens
- Implement mobile-first responsive design
- Ensure styles are scoped to prevent leakage
- Optimize for performance (minimize reflows, repaints)
- Maintain accessibility in all styling decisions

## Best Practices

### CSS Organization

- Group related styles together (layout, typography, colors, components)
- Use a consistent file structure (components, utilities, base, themes)
- Keep global styles minimal; prefer component-scoped styles
- Separate concerns: structure (layout), skin (colors/fonts), behavior (states)
- Use CSS layers (@layer) for managing cascade order
- Create utility classes for common patterns (spacing, text alignment)
- Document complex or non-obvious CSS patterns

### Naming Conventions

**BEM (Block Element Modifier):**

```css
/* Block: standalone component */
.card {
}

/* Element: part of a block */
.card__header {
}
.card__body {
}
.card__footer {
}

/* Modifier: variation or state */
.card--featured {
}
.card--compact {
}
.card__header--large {
}
```

**Benefits:**

- Clear component structure
- Prevents naming conflicts
- Self-documenting class names
- Easier to search and refactor

**Utility-First (Tailwind-style):**

```css
/* Functional, single-purpose classes */
.flex {
  display: flex;
}
.items-center {
  align-items: center;
}
.p-4 {
  padding: 1rem;
}
.text-lg {
  font-size: 1.125rem;
}
```

**Avoid:**

- Generic names (`.wrapper`, `.container`, `.content`)
- Overly specific names (`.blue-button-in-header`)
- Non-semantic names (`.mb-20`, `.pd-l-15`)
- Ambiguous abbreviations (`.btn-prm`, `.usr-crd`)

### Scoping Strategies

**CSS Modules:**

```css
/* Button.module.css */
.button {
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
}

.primary {
  background: var(--color-primary);
  color: white;
}
```

**CSS-in-JS (Styled Components):**

```typescript
import styled from "styled-components";

const Button = styled.button<{ $variant?: "primary" | "secondary" }>`
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  background: ${(props) =>
    props.$variant === "primary" ? "var(--color-primary)" : "transparent"};
`;
```

**Framework-Specific Style Scoping:**

Most modern frameworks provide built-in style scoping mechanisms:

- Vue: `<style scoped>` directive for component-scoped styles
- Angular: `styleUrls` property for component-specific stylesheets
- Svelte: Built-in scoped styling without special syntax

### CSS Custom Properties (Variables)

**Design Tokens:**

```css
:root {
  /* Colors */
  --color-primary: #0066cc;
  --color-secondary: #6c757d;
  --color-success: #28a745;
  --color-danger: #dc3545;

  /* Spacing */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  --space-xl: 2rem;

  /* Typography */
  --font-family-base: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.25rem;
  --font-weight-normal: 400;
  --font-weight-bold: 700;

  /* Borders */
  --border-radius-sm: 0.25rem;
  --border-radius-md: 0.5rem;
  --border-radius-lg: 1rem;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
}

/* Dark theme override */
[data-theme="dark"] {
  --color-primary: #4da6ff;
  --color-background: #1a1a1a;
  --color-text: #ffffff;
}
```

**Benefits:**

- Centralized theming
- Runtime theme switching
- Easier maintenance
- Better than SCSS variables for dynamic changes

### Responsive Design

**Mobile-First Approach:**

```css
/* Base styles (mobile) */
.container {
  padding: 1rem;
  width: 100%;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    padding: 2rem;
    max-width: 720px;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container {
    max-width: 960px;
  }
}

/* Large desktop */
@media (min-width: 1280px) {
  .container {
    max-width: 1200px;
  }
}
```

**Container Queries (Modern):**

```css
.card-container {
  container-type: inline-size;
}

@container (min-width: 400px) {
  .card {
    display: flex;
  }
}
```

### Flexbox and Grid

**Flexbox for One-Dimensional Layouts:**

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
```

**Grid for Two-Dimensional Layouts:**

```css
.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}

.layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
```

### Performance Optimization

**Minimize Reflows and Repaints:**

```css
/* Prefer transform and opacity (GPU-accelerated) */
.animate-enter {
  transform: translateY(20px);
  opacity: 0;
  transition: transform 0.3s, opacity 0.3s;
}

.animate-enter-active {
  transform: translateY(0);
  opacity: 1;
}

/* Avoid animating layout properties */
/* ❌ Bad */
.bad-animation {
  transition: height 0.3s, margin 0.3s;
}

/* ✅ Good */
.good-animation {
  transition: transform 0.3s;
}
```

**Use will-change Sparingly:**

```css
/* Only for elements that will definitely animate */
.dropdown-menu {
  will-change: opacity, transform;
}

/* Remove after animation */
.dropdown-menu.open {
  will-change: auto;
}
```

**Contain Layout Calculations:**

```css
.card {
  contain: layout style;
}

.isolated-component {
  contain: layout paint;
}
```

### Accessibility in CSS

**Focus Indicators:**

```css
/* Never remove focus outlines */
/* ❌ Bad */
button:focus {
  outline: none;
}

/* ✅ Good - Custom but visible */
button:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

**Color Contrast:**

```css
/* WCAG AA requires 4.5:1 for normal text, 3:1 for large text */
.text {
  color: #333333; /* Good contrast on white */
  background: #ffffff;
}

.large-text {
  font-size: 1.25rem;
  font-weight: 700;
  color: #666666; /* Acceptable for large/bold */
}
```

**Screen Reader Only Content:**

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.sr-only-focusable:focus,
.sr-only-focusable:active {
  position: static;
  width: auto;
  height: auto;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

### CSS Preprocessors (SCSS)

**Nesting (Use Sparingly):**

```scss
// ✅ Good - Max 3 levels deep
.card {
  padding: 1rem;

  &__header {
    font-weight: bold;
  }

  &--featured {
    border: 2px solid var(--color-primary);
  }
}

// ❌ Bad - Too deeply nested
.sidebar {
  .navigation {
    .menu {
      .item {
        .link {
        } // 5 levels - hard to override
      }
    }
  }
}
```

**Mixins for Reusable Patterns:**

```scss
@mixin button-base {
  padding: 0.5rem 1rem;
  border-radius: var(--border-radius-md);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

@mixin focus-ring($color: var(--color-primary)) {
  &:focus-visible {
    outline: 2px solid $color;
    outline-offset: 2px;
  }
}

.button {
  @include button-base;
  @include focus-ring;
}
```

### Utility Classes

**Spacing System:**

```css
/* Margin utilities */
.m-0 {
  margin: 0;
}
.m-1 {
  margin: 0.25rem;
}
.m-2 {
  margin: 0.5rem;
}
.m-4 {
  margin: 1rem;
}

.mt-4 {
  margin-top: 1rem;
}
.mr-4 {
  margin-right: 1rem;
}
.mb-4 {
  margin-bottom: 1rem;
}
.ml-4 {
  margin-left: 1rem;
}

/* Padding utilities */
.p-4 {
  padding: 1rem;
}
.px-4 {
  padding-left: 1rem;
  padding-right: 1rem;
}
.py-4 {
  padding-top: 1rem;
  padding-bottom: 1rem;
}
```

**Typography Utilities:**

```css
.text-sm {
  font-size: 0.875rem;
}
.text-base {
  font-size: 1rem;
}
.text-lg {
  font-size: 1.125rem;
}
.text-xl {
  font-size: 1.25rem;
}

.font-normal {
  font-weight: 400;
}
.font-bold {
  font-weight: 700;
}

.text-left {
  text-align: left;
}
.text-center {
  text-align: center;
}
.text-right {
  text-align: right;
}
```

## Code Standards

### Specificity Management

**Keep specificity low:**

```css
/* ✅ Good - Low specificity */
.button {
}
.button--primary {
}

/* ❌ Bad - High specificity */
#header .navigation ul li a.active {
}
```

**Avoid !important:**

```css
/* ❌ Bad */
.text-red {
  color: red !important;
}

/* ✅ Good - Increase specificity if needed */
.card .text-red {
  color: red;
}
```

### CSS Order

Organize properties in consistent order:

```css
.element {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  z-index: 10;

  /* Display & Box Model */
  display: flex;
  width: 100%;
  height: auto;
  padding: 1rem;
  margin: 0;

  /* Typography */
  font-size: 1rem;
  font-weight: 600;
  line-height: 1.5;
  color: #333;

  /* Visual */
  background: white;
  border: 1px solid #ddd;
  border-radius: 0.5rem;
  box-shadow: var(--shadow-md);

  /* Misc */
  cursor: pointer;
  transition: all 0.2s;
}
```

### Modern CSS Features

**Logical Properties:**

```css
/* Use logical properties for better RTL support */
.card {
  margin-inline-start: 1rem; /* Instead of margin-left */
  padding-block: 1rem; /* Instead of padding-top + padding-bottom */
  border-inline: 1px solid; /* Instead of border-left + border-right */
}
```

**Cascade Layers:**

```css
@layer base, components, utilities;

@layer base {
  * {
    box-sizing: border-box;
  }
}

@layer components {
  .button {
    /* component styles */
  }
}

@layer utilities {
  .mt-4 {
    margin-top: 1rem;
  }
}
```

**Nesting (Native CSS):**

```css
/* Modern browsers support native nesting */
.card {
  padding: 1rem;

  & .title {
    font-weight: bold;
  }

  &:hover {
    box-shadow: var(--shadow-lg);
  }
}
```

## Common Patterns

### Card Component

```css
.card {
  background: white;
  border-radius: var(--border-radius-md);
  box-shadow: var(--shadow-md);
  padding: var(--space-lg);
  transition: box-shadow 0.2s;
}

.card:hover {
  box-shadow: var(--shadow-lg);
}

.card__header {
  margin-bottom: var(--space-md);
  font-size: var(--font-size-lg);
  font-weight: var(--font-weight-bold);
}

.card__body {
  color: var(--color-text-secondary);
  line-height: 1.6;
}
```

### Responsive Navigation

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.nav-menu {
  display: none;
}

@media (min-width: 768px) {
  .nav-menu {
    display: flex;
    gap: 1rem;
  }

  .nav-toggle {
    display: none;
  }
}
```

## Validation and Verification

- Validate CSS with linters (stylelint)
- Check browser compatibility (Can I Use, Autoprefixer)
- Test responsive breakpoints on real devices
- Verify accessibility with contrast checkers
- Profile rendering performance with DevTools
- Test with different font sizes and zoom levels
- Verify RTL support if internationalized
