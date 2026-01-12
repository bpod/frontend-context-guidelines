---
description: "Next.js 15+ development standards using App Router, Server Components, and modern patterns"
applyTo: "**/*.{ts,tsx,js,jsx}"
---

# Next.js Development Instructions

Guidelines for building high-quality Next.js applications using the App Router, Server Components, and modern best practices.

## General Instructions

- Always use the `app/` directory (App Router) for all new projects
- Prefer Server Components by default; use Client Components only when necessary
- Implement proper TypeScript with strict mode enabled
- Prioritize maintainability, performance, and security
- Follow Next.js official documentation at https://nextjs.org/docs
- Use modern build tools and Next.js built-in optimizations

## Best Practices

### Project Structure

- Use `app/` for routing, layouts, pages, and route handlers
- Organize top-level folders: `app/`, `components/`, `lib/`, `hooks/`, `types/`, `public/`
- Use route groups with parentheses (e.g., `(admin)`, `(auth)`) to organize routes without URL changes
- Prefix private folders with underscore (e.g., `_internal/`) to signal implementation details
- For large applications, group features into feature folders (e.g., `app/dashboard/`, `app/auth/`)
- Place co-located components, styles, and tests near where they're used

### Server and Client Components

- Use Server Components by default for data fetching, heavy computation, and non-interactive content
- Add `'use client'` directive only when you need hooks, browser APIs, or client-only libraries
- Never use `next/dynamic` with `{ ssr: false }` inside Server Components
- Move all client-only logic into dedicated Client Components and import them directly in Server Components
- Compose Client Components inside Server Components, never the reverse
- Keep Client Components as small and focused as possible to minimize bundle size

### Component Best Practices

- Use PascalCase for component files and exports (e.g., `UserCard.tsx`)
- Use camelCase for hooks and utilities (e.g., `useUser.ts`, `formatDate.ts`)
- Use kebab-case for directories (e.g., `user-profile/`)
- Match component names to file names for consistency
- Use TypeScript interfaces for all props and return types
- Avoid deeply nested component structures; use barrel exports for organization

### Data Fetching

- Use Server Components for data fetching; avoid client-side fetching when possible
- Leverage Next.js built-in fetch caching and revalidation
- Use `fetch()` with `cache` and `revalidate` options for granular control
- Implement `revalidatePath()` and `revalidateTag()` for targeted invalidation
- Use modern data fetching libraries (React Query, SWR) for client-side data when needed
- Handle loading and error states with Suspense and Error Boundaries
- Never expose secrets in client-side code

### API Routes (Route Handlers)

- Place API routes in `app/api/` using the route handler pattern
- Export async functions named after HTTP verbs (`GET`, `POST`, `PUT`, `DELETE`)
- Use `NextRequest` and `NextResponse` for advanced features
- Always validate and sanitize input with schemas (zod, yup)
- Return appropriate HTTP status codes and structured error responses
- Implement authentication and authorization checks on sensitive endpoints
- Avoid long-running operations; consider background jobs or webhooks

### Performance Optimization

- Use Next.js Image component for all images with proper sizing
- Implement Font optimization with `next/font`
- Use dynamic imports for route-based code splitting
- Keep Server Components for heavy logic to minimize JavaScript sent to browser
- Implement Suspense boundaries with loading UI for async operations
- Avoid large client bundles by maximizing Server Component usage
- Use `useTransition()` for non-blocking state updates

### Styling

- Use CSS Modules for component-scoped styling
- Use Tailwind CSS, Styled Components, or CSS-in-JS consistently
- Implement responsive design with mobile-first approach
- Use CSS custom properties for theming and design system consistency
- Keep styling practices consistent across the project
- Implement dark mode support if part of design system

### Routing and Navigation

- Use the App Router for all routing; avoid legacy Pages Router
- Implement proper route organization with clear folder structure
- Use route parameters and query strings appropriately
- Implement layouts for shared UI across routes
- Handle 404 and error pages with `not-found.tsx` and `error.tsx`
- Use `<Link>` component for client-side navigation
- Implement breadcrumbs and navigation state management

### Middleware

- Use Next.js middleware for authentication, redirects, and request preprocessing
- Keep middleware logic lightweight to avoid slowing down requests
- Implement proper error handling for middleware failures
- Use conditional logic to optimize when middleware runs

### Testing

- Write tests for critical components and logic using Jest and React Testing Library
- Test user interactions and component behavior, not implementation
- Test API routes with integration tests
- Use Playwright for end-to-end testing
- Mock external API calls appropriately
- Test accessibility features and keyboard navigation

### Security

- Never commit secrets; use `.env.local` for development
- Validate all user input on both client and server
- Use HTTPS in production
- Implement proper CORS headers when needed
- Sanitize and escape user-generated content
- Use Content Security Policy (CSP) headers
- Keep dependencies updated and monitor security advisories
- Use environment variables for sensitive configuration

### Accessibility

- Use semantic HTML elements (button, nav, form, etc.)
- Implement ARIA attributes and roles appropriately
- Ensure keyboard navigation works for all interactive elements
- Provide descriptive alt text for images
- Maintain proper color contrast ratios (WCAG AA minimum)
- Test with screen readers and accessibility testing tools

## Code Standards

### Naming Conventions

- **Folders**: kebab-case (e.g., `user-profile/`)
- **Components**: PascalCase (e.g., `UserCard.tsx`)
- **Hooks**: camelCase with `use` prefix (e.g., `useUser.ts`)
- **Utilities**: camelCase (e.g., `formatDate.ts`)
- **Variables/Functions**: camelCase
- **Types/Interfaces**: PascalCase
- **Constants**: UPPER_SNAKE_CASE

### Type System

- Use TypeScript strict mode for all projects
- Define explicit types for props, state, and API responses
- Use union types for component variants and states
- Avoid `any` type; use `unknown` with proper type guards
- Create shared type definitions in `types/` directory
- Export types from component files for external usage

## Common Patterns

### Server Component with Data Fetching

```typescript
// app/users/page.tsx
export default async function UsersPage() {
  const response = await fetch("https://api.example.com/users", {
    cache: "revalidate",
    next: { revalidate: 3600 },
  });

  if (!response.ok) {
    throw new Error("Failed to fetch users");
  }

  const users = await response.json();

  return (
    <div>
      {users.map((user) => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
}
```

### Client Component with Interactivity

```typescript
// components/UserForm.tsx
"use client";

import { useState } from "react";

export function UserForm() {
  const [isSubmitting, setIsSubmitting] = useState(false);

  async function handleSubmit(formData: FormData) {
    setIsSubmitting(true);
    try {
      const response = await fetch("/api/users", {
        method: "POST",
        body: formData,
      });
      if (!response.ok) throw new Error("Submission failed");
      // Handle success
    } finally {
      setIsSubmitting(false);
    }
  }

  return <form action={handleSubmit}>{/* form fields */}</form>;
}
```

### API Route Handler

```typescript
// app/api/users/route.ts
import { NextRequest, NextResponse } from "next/server";
import { z } from "zod";

const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
});

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    const userData = userSchema.parse(body);

    // Process user data

    return NextResponse.json({ success: true }, { status: 201 });
  } catch (error) {
    return NextResponse.json({ error: "Invalid request" }, { status: 400 });
  }
}
```

## Validation and Verification

- Run `npm run build` to verify compilation and production build
- Run `npm run lint` to check code style and quality
- Run `npm run test` to execute test suite
- Test API routes with manual requests or integration tests
- Verify performance with built-in Analytics dashboard
- Check for unused imports and dead code regularly
