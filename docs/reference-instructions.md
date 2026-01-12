# Instructions Reference

Complete catalog of all instruction files available in this repository with descriptions, use cases, and technical details.

## Overview

This reference provides a comprehensive guide to all instruction files. Each entry includes:
- **Description**: What the instruction covers
- **Best for**: Ideal use cases
- **Applies to**: File patterns where it's active
- **Key topics**: Main areas covered
- **Combines well with**: Complementary instructions

## Quick Reference Table

| Instruction | Languages/Frameworks | Focus Area | Complexity |
|-------------|---------------------|------------|------------|
| [typescript-5-es2022](#typescript-5-es2022) | TypeScript, JavaScript | Language features | ⭐⭐ |
| [frontend-performance](#frontend-performance) | All | Performance optimization | ⭐⭐⭐⭐ |
| [api-integration](#api-integration) | All | API patterns | ⭐⭐⭐ |
| [css-best-practices](#css-best-practices) | CSS, SCSS, styled | Styling patterns | ⭐⭐⭐ |
| [state-management](#state-management) | React, Vue, Svelte | State patterns | ⭐⭐⭐⭐ |
| [responsive-design](#responsive-design) | All | Responsive UI | ⭐⭐⭐ |
| [forms-and-validation](#forms-and-validation) | All | Form handling | ⭐⭐⭐⭐ |
| [error-handling](#error-handling) | All | Error patterns | ⭐⭐⭐⭐ |
| [reactjs](#reactjs) | React | Component patterns | ⭐⭐⭐ |
| [nextjs](#nextjs) | Next.js | Framework best practices | ⭐⭐⭐⭐ |
| [playwright-typescript](#playwright-typescript) | Playwright | E2E testing | ⭐⭐⭐ |
| [a11y](#a11y-accessibility) | All | Accessibility | ⭐⭐⭐⭐ |
| [security-and-owasp](#security-and-owasp) | All | Security | ⭐⭐⭐⭐⭐ |
| [self-explanatory-code-commenting](#self-explanatory-code-commenting) | All | Documentation | ⭐⭐ |
| [update-docs-on-code-change](#update-docs-on-code-change) | All | Documentation | ⭐⭐ |
| [instructions](#instructions) | All | Meta (writing instructions) | ⭐⭐⭐ |

## Detailed Reference

### Frontend Performance

**File**: [frontend-performance.instructions.md](../instructions/frontend-performance.instructions.md)

**Description**: Performance optimization patterns for bundle size, loading times, Core Web Vitals, and runtime performance.

**Best for**:
- Optimizing application performance
- Improving Core Web Vitals scores
- Reducing bundle size and load times
- Runtime performance optimization

**Applies to**: `**/*.{ts,tsx,js,jsx,vue,svelte,css,scss}`

**Key Topics**:
- Bundle size optimization and code splitting
- Core Web Vitals (LCP, FID, CLS, INP, TTFB)
- Lazy loading and dynamic imports
- Image optimization
- Font loading strategies
- Caching strategies
- Performance budgets

**Combines Well With**:
- Framework-specific instructions for optimization patterns
- `accessibility-audit.prompt.md` for performance + accessibility

---

### API Integration

**File**: [api-integration.instructions.md](../instructions/api-integration.instructions.md)

**Description**: HTTP API integration patterns with error handling, retry logic, and type-safe data fetching.

**Best for**:
- RESTful API integration
- GraphQL clients
- Error handling and retry strategies
- Type-safe API calls

**Applies to**: `**/*.{ts,tsx,js,jsx,vue,svelte}`

**Key Topics**:
- HTTP clients (Fetch API, Axios)
- Error handling and retry logic
- Request/response interceptors
- Type safety with TypeScript
- Custom hooks for data fetching
- Optimistic updates

**Combines Well With**:
- `error-handling.instructions.md` for comprehensive error strategies
- `state-management.instructions.md` for server state

---

### CSS Best Practices

**File**: [css-best-practices.instructions.md](../instructions/css-best-practices.instructions.md)

**Description**: CSS organization, naming conventions, preprocessors, and maintainable stylesheet patterns.

**Best for**:
- CSS architecture and organization
- Naming conventions (BEM, utility-first)
- Preprocessor patterns (SCSS)
- CSS-in-JS implementations

**Applies to**: `**/*.{css,scss,sass,less,styled.ts,styled.js}`

**Key Topics**:
- BEM and naming conventions
- CSS Modules and scoping
- CSS custom properties (variables)
- Flexbox and Grid layouts
- Performance optimization
- Accessibility in CSS

**Combines Well With**:
- `responsive-design.instructions.md` for mobile-first styling
- `a11y.instructions.md` for accessible styling

---

### State Management

**File**: [state-management.instructions.md](../instructions/state-management.instructions.md)

**Description**: State management patterns for client state, server state, and state management libraries.

**Best for**:
- React, Vue, Svelte applications
- Client and server state separation
- State management libraries (Redux, Zustand, Pinia)
- Form state management

**Applies to**: `**/*.{ts,tsx,js,jsx,vue,svelte}`

**Key Topics**:
- Local vs global state
- Server state with TanStack Query
- Zustand for lightweight state
- Redux Toolkit for complex state
- Vue Pinia
- Form state patterns

**Combines Well With**:
- Framework-specific instructions
- `forms-and-validation.instructions.md` for form state

---

### Responsive Design

**File**: [responsive-design.instructions.md](../instructions/responsive-design.instructions.md)

**Description**: Mobile-first responsive design patterns with breakpoints, fluid typography, and adaptive layouts.

**Best for**:
- Mobile-first development
- Responsive breakpoint strategies
- Fluid typography and spacing
- Touch-friendly interfaces

**Applies to**: `**/*.{css,scss,sass,tsx,jsx,vue,svelte}`

**Key Topics**:
- Mobile-first approach
- Breakpoint strategies
- Fluid typography with clamp()
- Responsive images
- Container queries
- Touch target sizing

**Combines Well With**:
- `css-best-practices.instructions.md` for styling patterns
- `a11y.instructions.md` for accessible responsive design

---

### Forms and Validation

**File**: [forms-and-validation.instructions.md](../instructions/forms-and-validation.instructions.md)

**Description**: Form handling and validation patterns for accessible, user-friendly forms.

**Best for**:
- Form development
- Client and server-side validation
- React Hook Form, Formik patterns
- Accessible form design

**Applies to**: `**/*.{ts,tsx,js,jsx,vue,svelte}`

**Key Topics**:
- Semantic form HTML
- Validation strategies
- React Hook Form patterns
- Accessible error messaging
- Multi-step forms
- Async validation

**Combines Well With**:
- `a11y.instructions.md` for accessible forms
- `state-management.instructions.md` for form state

---

### Error Handling

**File**: [error-handling.instructions.md](../instructions/error-handling.instructions.md)

**Description**: Error handling strategies including error boundaries, logging, user feedback, and graceful degradation.

**Best for**:
- Robust error handling
- Error boundaries (React)
- Logging and monitoring
- User-friendly error messages

**Applies to**: `**/*.{ts,tsx,js,jsx,vue,svelte}`

**Key Topics**:
- Error boundaries
- Async error handling
- Error logging and monitoring
- Retry mechanisms
- Graceful degradation
- User-friendly error messages

**Combines Well With**:
- `api-integration.instructions.md` for API error handling
- Framework-specific instructions

---

### TypeScript 5 ES2022

**File**: [typescript-5-es2022.instructions.md](../instructions/typescript-5-es2022.instructions.md)

**Description**: Modern TypeScript development standards using TypeScript 5.x features and ES2022+ syntax.

**Best for**:
- TypeScript projects requiring type safety
- Modern JavaScript codebases
- Projects using latest language features

**Applies to**: `**/*.ts, **/*.tsx, **/*.js, **/*.jsx`

**Key Topics**:
- TypeScript 5.x features (decorators, const type parameters)
- Strict type checking
- Modern ES2022+ syntax (top-level await, class fields)
- Type inference and generics
- Utility types
- Module systems (ESM)

**Guidelines Include**:
- Always use strict mode
- Prefer `const` over `let`, avoid `var`
- Use type inference when obvious
- Explicit return types for public APIs
- Avoid `any` type, use `unknown` when needed
- Use template literal types
- Leverage conditional types

**Combines Well With**:
- `frameworks/react/react.instructions.md` for React + TypeScript
- `nextjs.instructions.md` for Next.js + TypeScript
- `security-and-owasp.instructions.md` for type-safe security

**Example Pattern**:
```typescript
// ✅ Good: Modern TypeScript with proper types
interface UserData {
  id: string;
  name: string;
  email: string;
}

async function fetchUser(id: string): Promise<UserData> {
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) throw new Error('Failed to fetch');
  return response.json() as Promise<UserData>;
}
```

---

### ReactJS

**File**: [frameworks/react/react.instructions.md](../instructions/frameworks/react/react.instructions.md)

**Description**: ReactJS development standards and best practices for building modern React applications.

**Best for**:
- React 18+ applications
- Component-driven development
- Projects using hooks and functional components

**Applies to**: `**/*.jsx, **/*.tsx, **/*.js, **/*.ts, **/*.css, **/*.scss`

**Key Topics**:
- Functional components with hooks
- Component composition patterns
- State management (useState, useReducer, useContext)
- Effects and lifecycle (useEffect)
- Performance optimization (useMemo, useCallback)
- TypeScript integration
- Testing patterns

**Guidelines Include**:
- Use functional components as default
- Implement proper prop validation with TypeScript
- Single responsibility principle for components
- Proper dependency arrays in useEffect
- Cleanup functions for side effects
- Component naming conventions (PascalCase)
- Custom hooks for reusable logic

**Combines Well With**:
- `typescript-5-es2022.instructions.md` for TypeScript + React
- `nextjs.instructions.md` for Next.js projects
- `a11y.instructions.md` for accessible components
- `playwright-typescript.instructions.md` for testing

**Example Pattern**:
```typescript
// ✅ Good: Functional component with proper typing
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

export const Button: React.FC<ButtonProps> = ({ 
  label, 
  onClick, 
  variant = 'primary',
  disabled = false 
}) => {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn-${variant}`}
    >
      {label}
    </button>
  );
};
```

---

### Next.js

**File**: [nextjs.instructions.md](../instructions/nextjs.instructions.md)

**Description**: Next.js 14+ best practices using App Router, Server Components, and modern patterns.

**Best for**:
- Next.js 14+ projects
- Server-side rendering applications
- Full-stack React applications
- Projects using App Router

**Applies to**: `**/*` (all files in Next.js projects)

**Key Topics**:
- App Router architecture
- Server Components vs Client Components
- File-based routing
- Data fetching patterns
- Route handlers (API routes)
- Metadata and SEO
- Performance optimization
- Image optimization

**Guidelines Include**:
- Use `app/` directory (App Router) over `pages/`
- Default to Server Components
- Use `'use client'` only when necessary
- Proper project structure (app/, lib/, components/)
- Route groups and private folders
- Server Actions for mutations
- Dynamic routes and parameters
- Static and dynamic rendering

**Combines Well With**:
- `frameworks/react/react.instructions.md` for React patterns
- `typescript-5-es2022.instructions.md` for TypeScript
- `a11y.instructions.md` for accessible apps
- `security-and-owasp.instructions.md` for secure APIs

**Example Pattern**:
```typescript
// ✅ Good: Server Component with data fetching
export default async function ProductPage({ 
  params 
}: { 
  params: { id: string } 
}) {
  // Fetch directly in Server Component
  const product = await fetchProduct(params.id);
  
  return (
    <div>
      <h1>{product.name}</h1>
      <ClientButton productId={product.id} />
    </div>
  );
}
```

---

### Playwright TypeScript

**File**: [playwright-typescript.instructions.md](../instructions/playwright-typescript.instructions.md)

**Description**: Best practices for writing end-to-end tests with Playwright and TypeScript.

**Best for**:
- E2E test automation
- Browser testing
- Integration testing
- Visual regression testing

**Applies to**: `**/*.spec.ts, **/*.test.ts, **/tests/**/*`

**Key Topics**:
- Test structure and organization
- Locator strategies
- Assertions and expectations
- Page Object Model
- Test fixtures and hooks
- Parallel execution
- Visual testing
- Mobile and responsive testing

**Guidelines Include**:
- Use descriptive test names
- Prefer user-facing selectors (role, label)
- Avoid hard-coded waits
- Use auto-waiting locators
- Implement Page Object Model for complex flows
- Proper test isolation
- Screenshot on failure
- Test data management

**Combines Well With**:
- `frameworks/react/react.instructions.md` for testing React components
- `nextjs.instructions.md` for testing Next.js apps
- `a11y.instructions.md` for accessibility testing

**Example Pattern**:
```typescript
// ✅ Good: Well-structured Playwright test
import { test, expect } from '@playwright/test';

test.describe('Shopping Cart', () => {
  test('should add product to cart', async ({ page }) => {
    await page.goto('/products/123');
    
    // Use semantic selectors
    await page.getByRole('button', { name: 'Add to Cart' }).click();
    
    // Clear assertion
    await expect(page.getByRole('status'))
      .toContainText('Item added to cart');
      
    // Verify cart count
    await expect(page.getByTestId('cart-count'))
      .toHaveText('1');
  });
});
```

---

### A11y (Accessibility)

**File**: [a11y.instructions.md](../instructions/a11y.instructions.md)

**Description**: Comprehensive guidance for creating accessible code following WCAG 2.2 Level AA standards.

**Best for**:
- Accessible web applications
- WCAG compliance
- Inclusive design
- Applications using assistive technologies

**Applies to**: `**/*` (all files)

**Key Topics**:
- WCAG 2.2 Level AA compliance
- Semantic HTML
- ARIA attributes and roles
- Keyboard navigation
- Screen reader support
- Color contrast
- Focus management
- Cognitive accessibility
- Form accessibility

**Guidelines Include**:
- Use semantic HTML elements
- Provide text alternatives for images
- Ensure sufficient color contrast
- Make all functionality keyboard accessible
- Manage focus properly
- Use ARIA when HTML semantics are insufficient
- Test with assistive technologies
- Implement skip links
- Handle keyboard traps
- Use people-first language

**Combines Well With**:
- `frameworks/react/react.instructions.md` for accessible React components
- `nextjs.instructions.md` for accessible Next.js apps
- `playwright-typescript.instructions.md` for accessibility testing

**Example Pattern**:
```typescript
// ✅ Good: Accessible modal dialog
export const Modal: React.FC<ModalProps> = ({ isOpen, onClose, title, children }) => {
  const modalRef = useRef<HTMLDivElement>(null);
  
  useEffect(() => {
    if (isOpen) {
      // Trap focus within modal
      modalRef.current?.focus();
    }
  }, [isOpen]);
  
  if (!isOpen) return null;
  
  return (
    <div
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-title"
      ref={modalRef}
      tabIndex={-1}
    >
      <h2 id="modal-title">{title}</h2>
      {children}
      <button onClick={onClose} aria-label="Close dialog">
        ×
      </button>
    </div>
  );
};
```

---

### Security and OWASP

**File**: [security-and-owasp.instructions.md](../instructions/security-and-owasp.instructions.md)

**Description**: Comprehensive secure coding instructions based on OWASP Top 10 and security best practices.

**Best for**:
- Security-conscious applications
- Projects handling sensitive data
- API development
- Authentication/authorization systems

**Applies to**: `*` (all files)

**Key Topics**:
- OWASP Top 10 vulnerabilities
- Input validation and sanitization
- SQL injection prevention
- XSS prevention
- CSRF protection
- Authentication best practices
- Session management
- Cryptography
- Dependency security
- Access control

**Guidelines Include**:
- Use parameterized queries (no SQL injection)
- Sanitize user input
- Implement CSP headers
- Use HTTPS everywhere
- Secure session management
- Strong password hashing (Argon2, bcrypt)
- No hardcoded secrets
- Principle of least privilege
- Rate limiting
- Keep dependencies updated

**Combines Well With**:
- All instruction files (security is universal)
- `nextjs.instructions.md` for secure API routes
- `typescript-5-es2022.instructions.md` for type-safe security

**Example Pattern**:
```typescript
// ✅ Good: Secure database query
async function getUserByEmail(email: string): Promise<User | null> {
  // Parameterized query prevents SQL injection
  const result = await db.query(
    'SELECT * FROM users WHERE email = $1',
    [email]
  );
  return result.rows[0] || null;
}

// ✅ Good: Secure secret management
const apiKey = process.env.API_KEY;
if (!apiKey) {
  throw new Error('API_KEY environment variable not set');
}
```

---

### Self-Explanatory Code Commenting

**File**: [self-explanitory-code-commenting.instructions.md](../instructions/self-explanitory-code-commenting.instructions.md)

**Description**: Guidelines for writing self-documenting code with meaningful comments.

**Best for**:
- Code maintainability
- Team collaboration
- Complex algorithm documentation
- API documentation

**Applies to**: `**/*`

**Key Topics**:
- When to comment vs when code speaks for itself
- JSDoc/TSDoc standards
- Inline comment best practices
- Documenting complex logic
- Explaining "why" not "what"
- API documentation
- TODO and FIXME conventions

**Guidelines Include**:
- Write self-explanatory code first
- Comment the "why", not the "what"
- Use JSDoc for public APIs
- Avoid obvious comments
- Keep comments updated with code
- Document assumptions and edge cases
- Use TODO/FIXME with tickets

**Combines Well With**:
- All instruction files
- `typescript-5-es2022.instructions.md` for type documentation

**Example Pattern**:
```typescript
// ✅ Good: Explains WHY, not WHAT
/**
 * Debounce search to avoid overwhelming the API with requests.
 * 300ms chosen based on user testing - shorter felt unresponsive,
 * longer felt laggy.
 */
const debouncedSearch = useDebouncedCallback(handleSearch, 300);

// ❌ Bad: States the obvious
// This function adds two numbers
function add(a: number, b: number): number {
  return a + b; // Returns the sum
}
```

---

### Update Docs on Code Change

**File**: [update-docs-on-code-change.instructions.md](../instructions/update-docs-on-code-change.instructions.md)

**Description**: Guidelines for automatically updating documentation when code changes.

**Best for**:
- Keeping documentation in sync
- README maintenance
- API documentation updates
- Change log generation

**Applies to**: `**/*`

**Key Topics**:
- When to update documentation
- Which documents to update
- Automated documentation generation
- Documentation testing
- Change log conventions

**Guidelines Include**:
- Update docs in the same PR as code changes
- Keep README current with features
- Update API docs when signatures change
- Maintain accurate examples
- Update troubleshooting guides
- Keep dependency lists current

**Combines Well With**:
- All instruction files
- `self-explanitory-code-commenting.instructions.md` for inline docs

---

### Instructions

**File**: [instructions.instructions.md](../instructions/instructions.instructions.md)

**Description**: Meta-guidelines for creating high-quality custom instruction files for GitHub Copilot.

**Best for**:
- Creating new instruction files
- Standardizing instruction format
- Understanding instruction structure
- Improving existing instructions

**Applies to**: `**/*.instructions.md`

**Key Topics**:
- Instruction file structure
- YAML frontmatter format
- Section organization
- Best practices for writing instructions
- ApplyTo patterns
- Versioning instructions

**Guidelines Include**:
- Required frontmatter fields
- Clear, actionable language
- Code examples (good vs bad)
- Proper scope with applyTo
- Logical section organization
- Testing instructions

**Combines Well With**:
- All instruction files (meta-level)

**Example Pattern**:
```markdown
---
description: "Brief description of purpose"
applyTo: "**/*.ts, **/*.tsx"
---

# Instruction Title

Clear introduction.

## Project Context
- Technologies and versions

## Development Standards
- Guidelines with examples

## Code Examples
✅ Good patterns
❌ Anti-patterns
```

## Usage Recommendations

### Starter Set (Minimum)

For new projects, start with:
1. Language instruction (e.g., `typescript-5-es2022.instructions.md`)
2. Framework instruction (e.g., `frameworks/react/react.instructions.md` or `nextjs.instructions.md`)
3. Security instruction (`security-and-owasp.instructions.md`)

### Recommended Set (Most Projects)

Add these for better coverage:
4. Testing instruction (`playwright-typescript.instructions.md`)
5. Accessibility instruction (`a11y.instructions.md`)
6. Documentation instruction (`self-explanitory-code-commenting.instructions.md`)

### Full Set (Enterprise/Critical Projects)

Include everything for maximum quality:
7. Documentation sync (`update-docs-on-code-change.instructions.md`)
8. Custom project-specific instructions

## Creating Custom Instructions

See [How to Use Instructions](how-to-use-instructions.md) for detailed guidance on creating custom instruction files.

Use [instructions.instructions.md](../instructions/instructions.instructions.md) as a template and reference.

## Related Resources

- **[How to Use Instructions](how-to-use-instructions.md)**: Practical guide for using instructions
- **[Tutorial: Getting Started](tutorial-getting-started.md)**: Step-by-step introduction
- **[Generate Custom Instructions](../prompts/generate-custom-instructions-from-codebase.prompt.md)**: Auto-generate from existing code

---

**Questions?** Check [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions) or open an [issue](https://github.com/bpod/frontend-context-guidelines/issues).
