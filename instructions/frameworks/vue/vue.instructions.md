---
description: "Vue 3 development standards using Composition API and modern patterns"
applyTo: "**/*.vue, **/*.ts, **/*.js"
---

# Vue Development Instructions

Guidelines for building high-quality Vue 3 applications using Composition API and modern best practices.

## General Instructions

- Use Vue 3 Composition API as primary pattern
- Implement proper TypeScript for type safety
- Use reactive data and computed properties appropriately
- Organize components by feature or domain
- Prioritize maintainability, readability, and performance
- Follow Vue style guide at https://vuejs.org/guide/

## Best Practices

### Component Design

- Use Single File Components (.vue) with script, template, and style
- Use Composition API with `<script setup>` syntax
- Keep components focused on single responsibility
- Use proper TypeScript typing for props and emits
- Implement component composition over complexity
- Use slots for flexible component composition

### Reactivity System

- Use `ref()` for reactive primitive values
- Use `reactive()` for complex objects (when appropriate)
- Use `computed()` for derived reactive state
- Implement watchers with proper cleanup
- Avoid unnecessary reactivity to improve performance
- Use readonly() to prevent mutations when needed

### Props and Emits

- Define props with TypeScript interface in Composition API
- Use type validation for props
- Define emits explicitly for type safety
- Implement two-way binding with v-model appropriately
- Keep prop interfaces focused and clear
- Document prop usage clearly

### State Management

- Use Pinia for application state management
- Define stores with clear separation of concerns
- Use state, getters, and actions in stores
- Implement proper typing for store state
- Keep stores simple and focused
- Use composition functions for reusable logic

### Forms and Validation

- Use v-model for form input binding
- Implement form validation with libraries like Vee-Validate
- Handle form submission and error states
- Use proper input types and attributes
- Implement accessible form markup
- Handle dynamic form fields cleanly

### Async Operations

- Use async/await in setup or actions
- Handle loading and error states properly
- Implement proper cleanup for async operations
- Use computed properties for conditional rendering
- Handle race conditions appropriately
- Show meaningful error messages to users

### Styling

- Use scoped styling within components
- Use CSS or SCSS preprocessor
- Implement responsive design with mobile-first approach
- Use CSS custom properties for theming
- Keep styles co-located with components
- Ensure accessibility in styles

### Routing

- Use Vue Router for client-side navigation
- Organize routes by feature
- Implement route guards for authentication
- Use lazy loading for route-based code splitting
- Handle route parameters and query strings
- Implement proper error and not-found pages

### Performance Optimization

- Use v-show vs v-if appropriately
- Implement proper component boundaries
- Use key binding in v-for for correct updates
- Leverage Vue's built-in optimizations
- Use lazy loading for images and components
- Minimize bundle size through code splitting

### Data Fetching

- Use composition functions for data fetching logic
- Implement proper error handling and loading states
- Use stores for caching fetched data
- Handle race conditions in concurrent requests
- Implement optimistic updates for better UX
- Use proper cleanup for pending requests

### Testing

- Write unit tests using Vitest and Vue Test Utils
- Test component behavior and user interactions
- Test composition functions and stores
- Mock external dependencies appropriately
- Test accessibility features
- Keep tests maintainable and focused

### Accessibility

- Use semantic HTML elements
- Implement proper ARIA attributes
- Ensure keyboard navigation works
- Provide descriptive labels for form inputs
- Maintain proper color contrast
- Test with screen readers and tools

### Security

- Sanitize user input to prevent XSS
- Validate input on both client and server
- Use HTTPS for all communication
- Never expose secrets in client-side code
- Keep dependencies updated
- Implement proper authentication

## Code Standards

### Naming Conventions

- **Components**: PascalCase file names (e.g., `UserCard.vue`)
- **Stores**: camelCase file names (e.g., `userStore.ts`)
- **Composables**: camelCase with `use` prefix (e.g., `useUser.ts`)
- **Utilities**: camelCase file names (e.g., `formatDate.ts`)
- **Variables/Functions**: camelCase
- **Types/Interfaces**: PascalCase
- **Constants**: UPPER_SNAKE_CASE

### File Organization

- Keep components in `src/components/` or feature directories
- Keep stores in `src/stores/`
- Keep composables in `src/composables/`
- Keep utilities in `src/utils/`
- Use clear directory structure by feature
- Use barrel files (index.ts) for clean imports

## Common Patterns

### Component with Composition API

```vue
<script setup lang="ts">
import { ref, computed } from "vue";

interface Props {
  initialCount?: number;
}

const props = withDefaults(defineProps<Props>(), {
  initialCount: 0,
});

const emit = defineEmits<{
  update: [value: number];
}>();

const count = ref(props.initialCount);
const doubled = computed(() => count.value * 2);

const increment = () => {
  count.value += 1;
  emit("update", count.value);
};
</script>

<template>
  <button @click="increment">Count: {{ count }}, Doubled: {{ doubled }}</button>
</template>
```

### Composable for Reusable Logic

```typescript
// useUser.ts
import { ref, computed } from "vue";

export function useUser(userId: string) {
  const user = ref(null);
  const loading = ref(false);
  const error = ref(null);

  const fetchUser = async () => {
    loading.value = true;
    try {
      const response = await fetch(`/api/users/${userId}`);
      user.value = await response.json();
    } catch (err) {
      error.value = err;
    } finally {
      loading.value = false;
    }
  };

  return { user, loading, error, fetchUser };
}
```

### Pinia Store

```typescript
// stores/user.ts
import { defineStore } from "pinia";
import { ref, computed } from "vue";

export const useUserStore = defineStore("user", () => {
  const user = ref(null);
  const isAuthenticated = computed(() => user.value !== null);

  const login = async (credentials) => {
    const response = await fetch("/api/login", {
      method: "POST",
      body: JSON.stringify(credentials),
    });
    user.value = await response.json();
  };

  const logout = () => {
    user.value = null;
  };

  return { user, isAuthenticated, login, logout };
});
```

## Validation and Verification

- Run `npm run build` to verify compilation
- Run `npm run lint` to check code style
- Run `npm run test` for unit tests
- Run `npm run preview` for production preview
- Check bundle size and performance
- Test accessibility with screen readers
