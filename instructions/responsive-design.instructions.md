---
description: "Responsive design patterns for mobile-first development, breakpoints, fluid typography, and adaptive layouts"
applyTo: "**/*.{css,scss,sass,tsx,jsx,vue,svelte,html}"
---

# Responsive Design

Guidelines for creating responsive, mobile-first user interfaces that adapt seamlessly across devices and screen sizes.

## General Instructions

- Use mobile-first approach (start with mobile, enhance for larger screens)
- Define consistent breakpoints across the application
- Implement fluid typography that scales with viewport
- Use responsive images with appropriate formats and sizes
- Design touch-friendly interfaces (44x44px minimum touch targets)
- Test on real devices, not just browser DevTools
- Consider performance implications of responsive strategies
- Ensure accessibility at all screen sizes
- Use CSS Grid and Flexbox for flexible layouts
- Avoid fixed widths; prefer max-width, min-width, and percentages

## Best Practices

### Mobile-First Approach

**Why Mobile-First:**

- Forces prioritization of content
- Better performance (smaller base CSS bundle)
- Progressive enhancement mindset
- Easier to scale up than scale down
- Matches most user behavior (mobile-first browsing)

```css
/* ✅ Mobile-first: Base styles for mobile, add complexity for larger screens */
.container {
  /* Mobile styles (default) */
  padding: 1rem;
  width: 100%;
}

@media (min-width: 768px) {
  /* Tablet: Add more complexity */
  .container {
    padding: 2rem;
    max-width: 720px;
    margin: 0 auto;
  }
}

@media (min-width: 1024px) {
  /* Desktop: Further enhancements */
  .container {
    max-width: 960px;
    padding: 3rem;
  }
}

/* ❌ Desktop-first: Requires overriding styles */
.container {
  max-width: 960px;
  padding: 3rem;
}

@media (max-width: 1023px) {
  .container {
    max-width: 720px;
    padding: 2rem;
  }
}

@media (max-width: 767px) {
  .container {
    padding: 1rem;
    max-width: none;
  }
}
```

### Breakpoint Strategy

**Standard Breakpoints:**

```css
:root {
  --breakpoint-sm: 640px; /* Mobile landscape */
  --breakpoint-md: 768px; /* Tablet portrait */
  --breakpoint-lg: 1024px; /* Tablet landscape / small laptop */
  --breakpoint-xl: 1280px; /* Desktop */
  --breakpoint-2xl: 1536px; /* Large desktop */
}

/* Usage with media queries */
@media (min-width: 640px) {
  /* sm */
}
@media (min-width: 768px) {
  /* md */
}
@media (min-width: 1024px) {
  /* lg */
}
@media (min-width: 1280px) {
  /* xl */
}
@media (min-width: 1536px) {
  /* 2xl */
}
```

**Content-Based Breakpoints:**

```css
/* Add breakpoints where content naturally breaks */
.article {
  font-size: 1rem;
  line-height: 1.6;
}

/* When line length exceeds readable width (~70 characters) */
@media (min-width: 600px) {
  .article {
    max-width: 65ch; /* Character-based width */
  }
}
```

**Component-Specific Breakpoints:**

```css
/* Navigation: Mobile menu -> Desktop nav at 768px */
.nav-menu {
  display: none; /* Hidden on mobile */
}

.nav-toggle {
  display: block; /* Show hamburger */
}

@media (min-width: 768px) {
  .nav-menu {
    display: flex; /* Show full menu */
  }

  .nav-toggle {
    display: none; /* Hide hamburger */
  }
}
```

### Fluid Typography

**CSS Clamp for Responsive Text:**

```css
/* Syntax: clamp(min, preferred, max) */
h1 {
  font-size: clamp(1.75rem, 4vw + 1rem, 3rem);
  /* Minimum: 28px, grows with viewport, maximum: 48px */
}

h2 {
  font-size: clamp(1.5rem, 3vw + 0.5rem, 2.25rem);
}

body {
  font-size: clamp(1rem, 0.5vw + 0.875rem, 1.125rem);
}
```

**Type Scale with CSS Custom Properties:**

```css
:root {
  /* Base font size scales with viewport */
  --font-size-base: clamp(1rem, 0.5vw + 0.875rem, 1.125rem);

  /* Scale multiplier */
  --type-scale: 1.25; /* Major third */

  /* Calculated sizes */
  --font-size-sm: calc(var(--font-size-base) / var(--type-scale));
  --font-size-lg: calc(var(--font-size-base) * var(--type-scale));
  --font-size-xl: calc(var(--font-size-lg) * var(--type-scale));
  --font-size-2xl: calc(var(--font-size-xl) * var(--type-scale));
}

h1 {
  font-size: var(--font-size-2xl);
}
h2 {
  font-size: var(--font-size-xl);
}
h3 {
  font-size: var(--font-size-lg);
}
p {
  font-size: var(--font-size-base);
}
small {
  font-size: var(--font-size-sm);
}
```

**Fluid Line Height:**

```css
body {
  /* Tighter line-height on mobile, more spacious on desktop */
  line-height: clamp(1.5, 0.1vw + 1.4, 1.75);
}
```

### Responsive Images

**Responsive Image Techniques:**

```html
<!-- 1. Srcset for different resolutions -->
<img
  src="image-800w.jpg"
  srcset="
    image-400w.jpg   400w,
    image-800w.jpg   800w,
    image-1200w.jpg 1200w,
    image-1600w.jpg 1600w
  "
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  alt="Responsive image"
  loading="lazy"
/>

<!-- 2. Picture element for art direction -->
<picture>
  <source media="(min-width: 1024px)" srcset="hero-desktop.jpg" />
  <source media="(min-width: 768px)" srcset="hero-tablet.jpg" />
  <img src="hero-mobile.jpg" alt="Hero image" />
</picture>

<!-- 3. Modern image formats with fallback -->
<picture>
  <source srcset="image.avif" type="image/avif" />
  <source srcset="image.webp" type="image/webp" />
  <img src="image.jpg" alt="Optimized image" />
</picture>
```

**CSS for Responsive Images:**

```css
/* Fluid images */
img {
  max-width: 100%;
  height: auto;
  display: block;
}

/* Aspect ratio container */
.image-container {
  position: relative;
  aspect-ratio: 16 / 9; /* Native CSS aspect ratio */
  overflow: hidden;
}

.image-container img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Object-fit for controlled image display */
.avatar {
  width: 100px;
  height: 100px;
  object-fit: cover; /* Crop to fit */
}

.logo {
  max-width: 200px;
  object-fit: contain; /* Scale to fit, maintain aspect ratio */
}
```

### Responsive Layouts

**CSS Grid for Responsive Layouts:**

```css
/* Auto-fit: Creates as many columns as fit */
.grid-auto-fit {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}

/* Auto-fill: Creates columns even if empty */
.grid-auto-fill {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

/* Named grid areas for complex layouts */
.layout {
  display: grid;
  grid-template-areas:
    "header"
    "main"
    "sidebar"
    "footer";
  gap: 1rem;
}

@media (min-width: 768px) {
  .layout {
    grid-template-areas:
      "header header"
      "main sidebar"
      "footer footer";
    grid-template-columns: 1fr 300px;
  }
}

@media (min-width: 1024px) {
  .layout {
    grid-template-areas:
      "header header header"
      "sidebar main aside"
      "footer footer footer";
    grid-template-columns: 250px 1fr 250px;
  }
}
```

**Flexbox for Responsive Components:**

```css
/* Responsive navigation */
.nav {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

@media (min-width: 768px) {
  .nav {
    flex-direction: row;
    gap: 2rem;
  }
}

/* Flexible card layout */
.card {
  display: flex;
  flex-direction: column;
}

@media (min-width: 640px) {
  .card {
    flex-direction: row;
    align-items: center;
  }
}

/* Responsive spacing with gap */
.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: clamp(0.5rem, 2vw, 1rem);
}
```

### Container Queries

**Component-based responsive design:**

```css
/* Container setup */
.card-container {
  container-type: inline-size;
  container-name: card;
}

/* Query the container, not the viewport */
@container card (min-width: 400px) {
  .card {
    display: flex;
    flex-direction: row;
  }

  .card__image {
    width: 40%;
  }

  .card__content {
    width: 60%;
  }
}

@container card (min-width: 600px) {
  .card {
    padding: 2rem;
  }

  .card__title {
    font-size: 1.5rem;
  }
}
```

### Touch-Friendly Design

**Minimum Touch Target Sizes:**

```css
/* WCAG 2.2 recommends 44x44px minimum */
button,
a,
input[type="checkbox"],
input[type="radio"] {
  min-height: 44px;
  min-width: 44px;
}

/* Increase tap area without changing visual size */
.icon-button {
  /* Visual size */
  width: 24px;
  height: 24px;

  /* Tap area (using padding or pseudo-element) */
  padding: 10px; /* Total: 44x44px */
}

/* Alternative: pseudo-element for larger hit area */
.small-button {
  position: relative;
  width: 24px;
  height: 24px;
}

.small-button::before {
  content: "";
  position: absolute;
  top: -10px;
  right: -10px;
  bottom: -10px;
  left: -10px;
}
```

**Touch Gestures and Hover States:**

```css
/* Prevent hover states on touch devices */
@media (hover: hover) and (pointer: fine) {
  /* Only apply hover effects on devices with precise pointers */
  .button:hover {
    background-color: var(--color-primary-dark);
  }
}

/* Touch-specific feedback */
.button:active {
  transform: scale(0.98);
}

/* Disable iOS tap highlight */
button,
a {
  -webkit-tap-highlight-color: transparent;
}
```

### Responsive Spacing

**Fluid Spacing with clamp:**

```css
:root {
  /* Spacing scale that grows with viewport */
  --space-xs: clamp(0.25rem, 0.5vw, 0.5rem);
  --space-sm: clamp(0.5rem, 1vw, 1rem);
  --space-md: clamp(1rem, 2vw, 1.5rem);
  --space-lg: clamp(1.5rem, 3vw, 2.5rem);
  --space-xl: clamp(2rem, 4vw, 4rem);
}

.section {
  padding-block: var(--space-lg);
}

.container {
  padding-inline: var(--space-md);
}
```

**Responsive Margins with Logical Properties:**

```css
/* Logical properties for better i18n support */
.card {
  margin-block-start: var(--space-md); /* margin-top */
  margin-block-end: var(--space-lg); /* margin-bottom */
  margin-inline: var(--space-sm); /* margin-left + margin-right */
  padding-block: var(--space-md); /* padding-top + padding-bottom */
  padding-inline: var(--space-lg); /* padding-left + padding-right */
}
```

## Framework-Specific Patterns

Framework-specific responsive implementations live in their respective guides:

- React: responsive hooks and breakpoint context patterns in [instructions/frameworks/react/react.instructions.md](instructions/frameworks/react/react.instructions.md)
- Vue: responsive breakpoint example in [instructions/frameworks/vue/vue.instructions.md](instructions/frameworks/vue/vue.instructions.md)

## Common Patterns

### Responsive Navigation

```css
/* Mobile: Hamburger menu */
.nav {
  position: fixed;
  top: 0;
  right: -100%;
  width: 80%;
  height: 100vh;
  background: white;
  transition: right 0.3s ease;
  z-index: 1000;
}

.nav.open {
  right: 0;
}

.nav-toggle {
  display: block;
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 1001;
}

/* Desktop: Horizontal menu */
@media (min-width: 768px) {
  .nav {
    position: static;
    width: auto;
    height: auto;
    display: flex;
    gap: 2rem;
  }

  .nav-toggle {
    display: none;
  }
}
```

### Responsive Typography

```css
/* Scale heading sizes responsively */
h1 {
  font-size: clamp(2rem, 5vw, 3.5rem);
  line-height: 1.2;
}

h2 {
  font-size: clamp(1.5rem, 4vw, 2.5rem);
  line-height: 1.3;
}

h3 {
  font-size: clamp(1.25rem, 3vw, 2rem);
  line-height: 1.4;
}

/* Responsive paragraph width */
p {
  max-width: 65ch; /* Optimal reading width */
  font-size: clamp(1rem, 0.5vw + 0.875rem, 1.125rem);
  line-height: clamp(1.5, 0.1vw + 1.4, 1.75);
}
```

### Responsive Cards

```css
.card-grid {
  display: grid;
  grid-template-columns: 1fr; /* Mobile: 1 column */
  gap: 1rem;
}

@media (min-width: 640px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr); /* Tablet: 2 columns */
  }
}

@media (min-width: 1024px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr); /* Desktop: 3 columns */
    gap: 1.5rem;
  }
}

@media (min-width: 1280px) {
  .card-grid {
    grid-template-columns: repeat(4, 1fr); /* Large: 4 columns */
  }
}
```

## Testing Responsive Designs

### Browser DevTools

- Test at common breakpoints: 320px, 375px, 768px, 1024px, 1440px
- Use device emulation for iOS and Android devices
- Test with DevTools Network throttling (3G, 4G)
- Verify touch target sizes with browser overlays

### Real Device Testing

- Test on actual iOS and Android devices
- Check different screen densities (1x, 2x, 3x)
- Verify scroll behavior and gestures
- Test landscape and portrait orientations

### Automated Testing

```typescript
// Playwright responsive testing
import { test, expect } from "@playwright/test";

const viewports = [
  { name: "mobile", width: 375, height: 667 },
  { name: "tablet", width: 768, height: 1024 },
  { name: "desktop", width: 1920, height: 1080 },
];

for (const viewport of viewports) {
  test(`Navigation works on ${viewport.name}`, async ({ page }) => {
    await page.setViewportSize(viewport);
    await page.goto("/");

    // Test navigation at this viewport
    if (viewport.width < 768) {
      // Mobile: hamburger menu
      await expect(page.locator(".nav-toggle")).toBeVisible();
    } else {
      // Desktop: full menu
      await expect(page.locator(".nav-menu")).toBeVisible();
    }
  });
}
```

## Validation and Verification

- Test at all defined breakpoints
- Verify no horizontal scroll at any width
- Check touch target sizes (minimum 44x44px)
- Validate images load appropriate sizes
- Test with browser zoom (100%, 150%, 200%)
- Verify text remains readable at all sizes
- Check layout doesn't break between breakpoints
- Test with slow network conditions
- Verify accessibility at all screen sizes
- Test keyboard navigation on mobile and desktop
