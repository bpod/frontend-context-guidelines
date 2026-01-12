---
description: "Svelte development standards for reactive, performant web applications"
applyTo: "**/*.svelte, **/*.ts, **/*.js"
---

# Svelte Development Instructions

Guidelines for building high-quality Svelte applications following official Svelte documentation at https://svelte.dev/docs.

## General Instructions

- Use Svelte for building reactive, performant web applications
- Leverage Svelte's built-in reactivity and state management
- Implement proper TypeScript for type safety
- Prioritize simplicity, performance, and maintainability
- Use SvelteKit for full-stack applications
- Follow Svelte style guide and conventions

## Best Practices

### Component Design

- Use single-file components with template, script, and style
- Keep components focused on single responsibility
- Use proper prop typing with TypeScript
- Implement reactive declarations using `$:` syntax
- Use component slots for composition
- Implement proper event handling with event dispatching

### Reactivity

- Leverage Svelte's automatic reactivity system
- Use reactive declarations (`$:`) for computed values
- Use reactive statements for side effects
- Implement proper variable scoping
- Avoid unnecessary reactivity by using local variables
- Use `bind:` directive for two-way binding

### State Management

- Use component local state with `let` declarations
- Use stores for shared state (writable, readable, derived)
- Implement custom stores for domain-specific logic
- Use store subscriptions with `$` prefix in components
- Keep state immutable when possible
- Use context API for component tree state sharing

#### Custom Store Example

```typescript
// stores/notifications.ts
import { derived, writable } from "svelte/store";

type Notification = {
  id: string;
  message: string;
  type: "info" | "success" | "warning" | "error";
  duration: number;
};

function createNotificationStore() {
  const { subscribe, update } = writable<Notification[]>([]);

  return {
    subscribe,
    add: (notification: Omit<Notification, "id">) => {
      const id = crypto.randomUUID();
      const next = { id, ...notification };
      update((notifications) => [...notifications, next]);

      if (notification.duration > 0) {
        setTimeout(() => {
          update((notifications) => notifications.filter((n) => n.id !== id));
        }, notification.duration);
      }
    },
    remove: (id: string) => {
      update((notifications) => notifications.filter((n) => n.id !== id));
    },
    clear: () => update(() => []),
  };
}

export const notifications = createNotificationStore();
export const unreadCount = derived(
  notifications,
  ($notifications) => $notifications.length
);
```

```svelte
<script lang="ts">
  import { notifications, unreadCount } from "./stores/notifications";

  function showNotification() {
    notifications.add({ message: "Task completed", type: "success", duration: 3000 });
  }
</script>

<div>
  <button on:click={showNotification}>Show Notification</button>
  <span>Unread: {$unreadCount}</span>
</div>
```

### Forms and Validation

- Use form element binding with `bind:` directive
- Implement validation in components or custom validators
- Handle form submission and error states
- Use two-way binding for form inputs
- Implement proper accessibility attributes
- Handle dynamic form fields cleanly

### Async Operations

- Use async/await within component scripts
- Handle loading and error states properly
- Implement proper cleanup for async operations
- Use stores for managing server state
- Implement optimistic updates for better UX
- Handle network errors gracefully

### Styling

- Use scoped styles within Svelte components
- Use CSS preprocessors (Sass/SCSS) when beneficial
- Implement responsive design with mobile-first approach
- Use CSS custom properties for theming
- Keep styling co-located with components
- Ensure proper accessibility in styles

### Routing (with SvelteKit)

- Use file-based routing in `src/routes`
- Implement layout components for shared UI
- Use page components for route-specific content
- Implement proper error and not-found pages
- Use server-side rendering for performance and SEO
- Handle navigation with proper loading states

### Performance Optimization

- Leverage Svelte's compile-time optimizations
- Use `<svelte:component>` for dynamic components
- Implement code splitting with dynamic imports
- Use `{#key}` blocks when appropriate
- Minimize re-renders by proper component boundaries
- Profile and optimize rendering with Svelte DevTools

### Data Fetching

- Use `load` functions in SvelteKit for server-side data fetching
- Implement proper error handling and loading states
- Use stores for caching fetched data
- Implement proper cache invalidation strategies
- Handle race conditions in async operations
- Use server routes for API endpoints in SvelteKit

### Testing

- Write component tests using Vitest and Svelte Testing Library
- Test user interactions and reactive behavior
- Mock external dependencies appropriately
- Test async operations and loading states
- Test accessibility features
- Keep tests maintainable and focused

### Accessibility

- Use semantic HTML elements
- Implement proper ARIA attributes and roles
- Ensure keyboard navigation works throughout app
- Provide descriptive labels for form inputs
- Maintain proper color contrast ratios
- Test with screen readers and accessibility tools

### Security

- Sanitize user input to prevent XSS attacks
- Validate on both client and server
- Use HTTPS for all communication
- Never expose secrets in client-side code
- Keep dependencies updated
- Implement proper authentication patterns

## Code Standards

### Naming Conventions

- **Components**: PascalCase file names (e.g., `UserCard.svelte`)
- **Stores**: camelCase file names (e.g., `userStore.ts`)
- **Actions**: camelCase names (e.g., `clickOutside`)
- **Utilities**: camelCase file names (e.g., `formatDate.ts`)
- **Variables/Functions**: camelCase
- **Types/Interfaces**: PascalCase
- **Constants**: UPPER_SNAKE_CASE

### File Organization

- Keep components in `src/lib/components/` or feature directories
- Keep stores in `src/lib/stores/`
- Keep utilities in `src/lib/utils/`
- Use `+page.svelte` and `+layout.svelte` for routing
- Keep related files together
- Use barrel files for clean imports

## Common Patterns

### Component with Reactive State

```svelte
<script lang="ts">
  let count = 0;
  $: doubled = count * 2;

  function increment() {
    count += 1;
  }
</script>

<button on:click={increment}>
  Count: {count}, Doubled: {doubled}
</button>
```

### Store Usage

```typescript
// store.ts
import { writable } from "svelte/store";

export const user = writable<User | null>(null);

export const logout = () => {
  user.set(null);
};
```

```svelte
<!-- Component.svelte -->
<script>
  import { user, logout } from './store';
</script>

{#if $user}
  <p>Welcome, {$user.name}</p>
  <button on:click={logout}>Logout</button>
{/if}
```

### SvelteKit Data Loading

```typescript
// +page.ts
export async function load({ fetch }) {
  const response = await fetch("/api/users");
  const users = await response.json();
  return { users };
}
```

```svelte
<!-- +page.svelte -->
<script>
  export let data;
</script>

<ul>
  {#each data.users as user (user.id)}
    <li>{user.name}</li>
  {/each}
</ul>
```

## Validation and Verification

- Run `npm run build` to verify compilation
- Run `npm run lint` to check code style
- Run `npm run test` for unit tests
- Run `npm run preview` to test production build
- Check bundle size and performance
- Test accessibility with screen readers
