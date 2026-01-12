---
description: "React 19+ development standards and best practices for functional components and modern patterns"
applyTo: "**/*.jsx, **/*.tsx"
---

# React Development Instructions

Guidelines for building high-quality React applications with modern patterns, hooks, and best practices following the official React documentation at https://react.dev.

## General Instructions

- Use functional components with hooks as the primary pattern
- Implement component composition over inheritance
- Organize components by feature or domain for scalability
- Follow the single responsibility principle for components
- Use TypeScript for type safety (when applicable)
- Prioritize maintainability and clarity over clever shortcuts

## Best Practices

### Component Design

- Create reusable, focused components that handle one concern
- Use descriptive and consistent naming conventions (PascalCase for components)
- Design components to be testable by testing behavior, not implementation
- Implement proper prop typing with TypeScript interfaces or PropTypes
- Avoid prop drilling by using Context or state management libraries
- Use composition patterns (render props, children as functions) when beneficial

### State Management

- Use `useState` for local component state
- Implement `useReducer` for complex state logic requiring multiple related state updates
- Leverage `useContext` for sharing state across component trees
- Consider external libraries (Redux Toolkit, Zustand, Jotai) for complex applications
- Keep state as close to where it's used as possible
- Use React Query or SWR for server state management and caching

### Hooks and Effects

- Use `useEffect` with proper dependency arrays to prevent unintended side effects
- Always include cleanup functions in effects to prevent memory leaks and dangling connections
- Use `useMemo` and `useCallback` only when profiling reveals performance issues
- Create custom hooks to extract and reuse stateful logic
- Follow the rules of hooks: only call at the top level, not conditionally
- Use `useRef` for accessing DOM elements or storing mutable values that don't cause re-renders

### Performance Optimization

- Use `React.memo` for components that receive expensive prop objects
- Implement code splitting with `React.lazy` and `Suspense` for route-based features
- Profile components with React DevTools to identify actual bottlenecks before optimizing
- Implement virtual scrolling only when rendering lists exceeding 100+ items
- Keep bundle size minimal through tree shaking and dynamic imports
- Defer non-critical updates using `useTransition` (React 18+)

### Data Fetching

- Use modern data fetching libraries (React Query, SWR) for API integration
- Implement proper loading, error, and success states
- Handle race conditions using cleanup functions in useEffect
- Use optimistic updates for better perceived performance
- Implement proper caching strategies to avoid redundant requests
- Handle offline scenarios gracefully with fallback UI

### Error Handling

- Implement Error Boundaries for component-level error handling
- Use proper error states in data fetching with descriptive messages
- Implement fallback UI for error scenarios
- Handle async errors in effects and event handlers with try/catch
- Provide meaningful error messages to users, not technical details
- Log errors appropriately for debugging and monitoring

### Forms and Validation

- Use controlled components for form inputs
- Implement validation with libraries like React Hook Form or Formik
- Handle form submission and error states with clear feedback
- Implement debounced validation for better user experience
- Use proper ARIA attributes and labels for accessibility
- Handle complex scenarios like file uploads and dynamic fields cleanly

### Styling

- Use CSS Modules, Styled Components, or modern CSS-in-JS solutions consistently
- Implement responsive design with mobile-first approach
- Use CSS custom properties (variables) for theming and consistency
- Maintain consistent spacing, typography, and color systems
- Keep styles co-located with components when using CSS-in-JS
- Ensure proper ARIA attributes and semantic HTML for accessibility

### Routing

- Use React Router for client-side navigation
- Implement nested routes and route protection when needed
- Handle route parameters and query strings properly
- Lazy load route-based components for code splitting
- Use proper navigation patterns with back button support
- Implement breadcrumbs and navigation state management

### Testing

- Write unit tests for components using React Testing Library
- Test component behavior and user interactions, not implementation details
- Use Jest for test runner and assertions
- Mock external dependencies and API calls appropriately
- Test accessibility features and keyboard navigation
- Keep tests maintainable by avoiding over-testing implementation details

### Security

- Sanitize user inputs to prevent XSS attacks
- Validate and escape data before rendering in templates
- Use HTTPS for all external API calls
- Implement proper authentication and authorization patterns
- Avoid storing sensitive data in localStorage or sessionStorage
- Never expose secrets in client-side code or environment variables

### Accessibility

- Use semantic HTML elements appropriately (button, form, nav, etc.)
- Implement proper ARIA attributes and roles when semantic HTML isn't sufficient
- Ensure keyboard navigation works for all interactive elements with visible focus indicators
- Provide descriptive alt text for images and labels for form inputs
- Maintain proper color contrast ratios (WCAG AA minimum)
- Test with screen readers and accessibility tools like Accessibility Insights

## Code Standards

### Naming Conventions

- Use PascalCase for component files and exports (e.g., `UserCard.tsx`)
- Use camelCase for variables, functions, and hooks (e.g., `const isLoading = false`)
- Use camelCase for hook files (e.g., `useUser.ts`, `useFetch.ts`)
- Use UPPER_SNAKE_CASE for constants (e.g., `const MAX_RETRIES = 3`)
- Name components for their behavior or domain, not their implementation

### File Organization

- Follow the repository's folder and responsibility layout for new code
- Use kebab-case for directory names (e.g., `user-profile/`)
- Keep components, tests, and related assets in the same directory when possible
- Reuse or extend shared utilities in `lib/` or `utils/` before adding new ones
- Create an `index.ts` barrel file in directories with multiple exports

### Code Style

- Run the repository's lint/format scripts before submitting (e.g., `npm run lint`)
- Match the project's indentation, quote style, and trailing comma rules
- Keep functions focused; extract helpers when logic branches grow complex
- Use immutable patterns and pure functions when practical
- Avoid deeply nested conditionals; guard early to reduce complexity

## Common Patterns

### Custom Hook Pattern

```typescript
function useUser(userId: string) {
  const [user, setUser] = useState<User | null>(null);
  const [error, setError] = useState<Error | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const controller = new AbortController();

    const fetchUser = async () => {
      try {
        const response = await fetch(`/api/users/${userId}`, {
          signal: controller.signal,
        });
        if (!response.ok) throw new Error("Failed to fetch user");
        const data = await response.json();
        setUser(data);
      } catch (err) {
        if (err instanceof Error && err.name !== "AbortError") {
          setError(err);
        }
      } finally {
        setLoading(false);
      }
    };

    fetchUser();

    return () => controller.abort();
  }, [userId]);

  return { user, error, loading };
}
```

### Error Boundary Pattern

```typescript
class ErrorBoundary extends React.Component<
  { children: React.ReactNode },
  { hasError: boolean }
> {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error("Error caught:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <div>Something went wrong. Please try again.</div>;
    }
    return this.props.children;
  }
}
```

### Controlled Component Pattern

```typescript
function SearchForm() {
  const [query, setQuery] = useState("");

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setQuery(e.currentTarget.value);
  };

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    // Handle search with query
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={query} onChange={handleChange} placeholder="Search..." />
      <button type="submit">Search</button>
    </form>
  );
}
```

## Validation and Verification

- Run tests with `npm run test` before submitting changes
- Run linting with `npm run lint` to catch style violations
- Build project with `npm run build` to verify no compilation errors
- Test components in isolation using Storybook or similar tool
- Verify accessibility with automated and manual testing
