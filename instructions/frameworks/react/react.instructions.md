---
description: "React 19+ development standards and best practices for functional components and modern patterns"
applyTo: "**/*.{jsx,tsx,js,ts,css,scss}"
---

# React Development Instructions

Guidelines for building high-quality React applications with modern patterns, hooks, and best practices following the official React documentation at https://react.dev.

## Project Context

- Latest React version (React 19+)
- TypeScript preferred for type safety when available
- Functional components with hooks as the default pattern
- Modern build tooling (Vite, Next.js, CRA alternatives, or custom Webpack)
- Component composition and reusable hooks as core architecture principles

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

#### Zustand (Lightweight Global State)

```typescript
// stores/cartStore.ts
import { create } from "zustand";
import { devtools, persist } from "zustand/middleware";

type CartItem = {
  id: string;
  name: string;
  price: number;
  quantity: number;
};

type CartStore = {
  items: CartItem[];
  addItem: (item: Omit<CartItem, "quantity">) => void;
  removeItem: (id: string) => void;
  updateQuantity: (id: string, quantity: number) => void;
  clearCart: () => void;
  total: () => number;
};

export const useCartStore = create<CartStore>()(
  devtools(
    persist(
      (set, get) => ({
        items: [],
        addItem: (item) =>
          set((state) => {
            const existing = state.items.find((i) => i.id === item.id);
            if (existing) {
              return {
                items: state.items.map((i) =>
                  i.id === item.id ? { ...i, quantity: i.quantity + 1 } : i
                ),
              };
            }
            return { items: [...state.items, { ...item, quantity: 1 }] };
          }),
        removeItem: (id) =>
          set((state) => ({
            items: state.items.filter((item) => item.id !== id),
          })),
        updateQuantity: (id, quantity) =>
          set((state) => ({
            items: state.items.map((item) =>
              item.id === id ? { ...item, quantity } : item
            ),
          })),
        clearCart: () => set({ items: [] }),
        total: () =>
          get().items.reduce(
            (sum, item) => sum + item.price * item.quantity,
            0
          ),
      }),
      { name: "cart-storage" }
    )
  )
);
```

#### Redux Toolkit (Complex Global State)

```typescript
// features/todos/todosSlice.ts
import { createAsyncThunk, createSlice, PayloadAction } from "@reduxjs/toolkit";

type Todo = { id: string; title: string; completed: boolean };
type TodosState = {
  items: Todo[];
  loading: boolean;
  error: string | null;
  filter: "all" | "active" | "completed";
};

export const fetchTodos = createAsyncThunk("todos/fetchTodos", async () => {
  const response = await fetch("/api/todos");
  return response.json();
});

const todosSlice = createSlice({
  name: "todos",
  initialState: {
    items: [],
    loading: false,
    error: null,
    filter: "all",
  } as TodosState,
  reducers: {
    addTodo: (state, action: PayloadAction<Omit<Todo, "id" | "completed">>) => {
      state.items.push({
        id: crypto.randomUUID(),
        completed: false,
        ...action.payload,
      });
    },
    toggleTodo: (state, action: PayloadAction<string>) => {
      const todo = state.items.find((t) => t.id === action.payload);
      if (todo) todo.completed = !todo.completed;
    },
    setFilter: (state, action: PayloadAction<TodosState["filter"]>) => {
      state.filter = action.payload;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchTodos.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(fetchTodos.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message || "Failed to fetch todos";
      });
  },
});

export const { addTodo, toggleTodo, setFilter } = todosSlice.actions;
export default todosSlice.reducer;
```

#### URL State Management

```typescript
import { useSearchParams } from "react-router-dom";

function ProductList() {
  const [searchParams, setSearchParams] = useSearchParams();

  const page = Number(searchParams.get("page")) || 1;
  const category = searchParams.get("category") || "all";
  const sort = searchParams.get("sort") || "name";

  const updateFilters = (updates: Record<string, string>) => {
    setSearchParams((prev) => {
      const next = new URLSearchParams(prev);
      Object.entries(updates).forEach(([key, value]) => {
        next.set(key, value);
      });
      return next;
    });
  };

  return (
    <div>
      <select
        value={category}
        onChange={(e) => updateFilters({ category: e.target.value, page: "1" })}
      >
        <option value="all">All</option>
        <option value="electronics">Electronics</option>
      </select>
    </div>
  );
}
```

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

#### React Query Example

```typescript
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

// Basic query with caching
function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const response = await fetch("/api/users");
      if (!response.ok) throw new Error("Failed to fetch users");
      return response.json();
    },
    staleTime: 5 * 60 * 1000,
    cacheTime: 10 * 60 * 1000,
  });
}

// Mutation with error handling
function useCreateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (user: NewUser) => {
      const response = await fetch("/api/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user),
      });
      if (!response.ok) throw new Error("Failed to create user");
      return response.json();
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["users"] });
    },
  });
}
```

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

#### Controlled Components

```typescript
import { useState } from "react";

function LoginForm() {
  const [values, setValues] = useState({ email: "", password: "" });
  const [errors, setErrors] = useState<Record<string, string>>({});
  const [touched, setTouched] = useState<Record<string, boolean>>({});

  const validateField = (name: string, value: string): string | null => {
    if (name === "email") {
      if (!value) return "Email is required";
      if (!/\S+@\S+\.\S+/.test(value)) return "Invalid email";
      return null;
    }
    if (name === "password") {
      if (!value) return "Password is required";
      if (value.length < 8) return "Password must be at least 8 characters";
      return null;
    }
    return null;
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setValues((prev) => ({ ...prev, [name]: value }));
    if (errors[name]) {
      setErrors((prev) => {
        const next = { ...prev };
        delete next[name];
        return next;
      });
    }
  };

  const handleBlur = (e: React.FocusEvent<HTMLInputElement>) => {
    const { name } = e.target;
    setTouched((prev) => ({ ...prev, [name]: true }));
    const error = validateField(name, values[name as keyof typeof values]);
    if (error) setErrors((prev) => ({ ...prev, [name]: error }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    const newErrors: Record<string, string> = {};
    Object.entries(values).forEach(([key, value]) => {
      const error = validateField(key, value);
      if (error) newErrors[key] = error;
    });
    if (Object.keys(newErrors).length) {
      setErrors(newErrors);
      setTouched({ email: true, password: true });
      return;
    }
    await login(values);
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* inputs with aria-invalid + describedby */}
    </form>
  );
}
```

#### React Hook Form + Zod

```typescript
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  name: z.string().min(2, "Name must be at least 2 characters"),
  email: z.string().email("Invalid email address"),
  age: z.number().min(18, "Must be at least 18").max(120, "Invalid age"),
  terms: z.boolean().refine((val) => val === true, "You must accept the terms"),
});

type FormData = z.infer<typeof schema>;

function RegistrationForm() {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting, isValid },
    setError,
    reset,
  } = useForm<FormData>({
    resolver: zodResolver(schema),
    mode: "onBlur",
    defaultValues: { name: "", email: "", age: 0, terms: false },
  });

  const onSubmit = async (data: FormData) => {
    const response = await fetch("/api/register", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });

    if (!response.ok) {
      const error = await response.json();
      if (error.errors) {
        Object.entries(error.errors).forEach(([field, message]) => {
          setError(field as keyof FormData, {
            type: "server",
            message: message as string,
          });
        });
      }
      return;
    }

    reset();
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} noValidate>
      {/* inputs with aria-invalid + errors.* rendering */}
      <button type="submit" disabled={isSubmitting || !isValid}>
        {isSubmitting ? "Submitting..." : "Register"}
      </button>
    </form>
  );
}
```

### Styling

- Use CSS Modules, Styled Components, or modern CSS-in-JS solutions consistently
- Implement responsive design with mobile-first approach
- Use CSS custom properties (variables) for theming and consistency
- Maintain consistent spacing, typography, and color systems
- Keep styles co-located with components when using CSS-in-JS
- Ensure proper ARIA attributes and semantic HTML for accessibility

### Responsive Patterns

- Use a reusable `useMediaQuery` hook for breakpoint checks
- Provide a `BreakpointProvider` context to share breakpoint state across the tree
- Keep navigation and layout components responsive via composition instead of one-off conditionals

```typescript
// useMediaQuery hook
import { useEffect, useState } from "react";

export function useMediaQuery(query: string): boolean {
  const [matches, setMatches] = useState(false);

  useEffect(() => {
    const media = window.matchMedia(query);
    if (media.matches !== matches) setMatches(media.matches);

    const listener = () => setMatches(media.matches);
    media.addEventListener("change", listener);
    return () => media.removeEventListener("change", listener);
  }, [matches, query]);

  return matches;
}

// Breakpoint context
import { createContext, ReactNode, useContext } from "react";

type Breakpoint = "mobile" | "tablet" | "desktop";
const BreakpointContext = createContext<Breakpoint>("mobile");

export function BreakpointProvider({ children }: { children: ReactNode }) {
  const isMobile = useMediaQuery("(max-width: 767px)");
  const isTablet = useMediaQuery("(min-width: 768px) and (max-width: 1023px)");

  const breakpoint: Breakpoint = isMobile
    ? "mobile"
    : isTablet
    ? "tablet"
    : "desktop";

  return (
    <BreakpointContext.Provider value={breakpoint}>
      {children}
    </BreakpointContext.Provider>
  );
}

export const useBreakpoint = () => useContext(BreakpointContext);
```

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

## Implementation Process

1. Plan component architecture and data flow
2. Set up project structure with clear folder organization
3. Define TypeScript interfaces and types
4. Implement core components with styling and accessibility
5. Add state management and data fetching logic
6. Implement routing and navigation
7. Add form handling and validation
8. Implement error handling and loading states
9. Add testing coverage for components and functionality
10. Optimize performance and bundle size
11. Ensure accessibility compliance
12. Document complex components and hooks

## Additional Guidelines

- Follow React naming conventions (PascalCase for components, camelCase for functions)
- Keep dependencies updated and audit for security vulnerabilities
- Use ESLint and Prettier for consistent formatting
- Prefer meaningful commit messages and a clean git history
- Apply code splitting and lazy loading strategies for large surfaces

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

### Error Boundaries

Error Boundaries catch errors in component trees and provide fallback UI.

```typescript
import { Component, ReactNode, ErrorInfo } from "react";

type Props = {
  children: ReactNode;
  fallback?: ReactNode;
  onError?: (error: Error, errorInfo: ErrorInfo) => void;
};

type State = {
  hasError: boolean;
  error: Error | null;
};

class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error("Error caught:", error, errorInfo);
    this.props.onError?.(error, errorInfo);
    logErrorToService({ error, errorInfo, url: window.location.href });
  }

  handleReset = () => {
    this.setState({ hasError: false, error: null });
  };

  render() {
    if (this.state.hasError) {
      if (this.props.fallback) return this.props.fallback;
      return (
        <div role="alert">
          <h2>Something went wrong</h2>
          <button onClick={this.handleReset}>Try again</button>
        </div>
      );
    }
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <MainApp />
    </ErrorBoundary>
  );
}
```

### Error Handling with React 19

React 19 supports error handling with the `use()` hook and Suspense:

```typescript
import { use, Suspense } from "react";

function UserProfile({ userPromise }: { userPromise: Promise<User> }) {
  try {
    const user = use(userPromise);
    return <div>{user.name}</div>;
  } catch (error) {
    return <ErrorMessage error={error} />;
  }
}

function App() {
  return (
    <ErrorBoundary>
      <Suspense fallback={<Loading />}>
        <UserProfile userPromise={fetchUser()} />
      </Suspense>
    </ErrorBoundary>
  );
}
```

## Validation and Verification

- Run tests with `npm run test` before submitting changes
- Run linting with `npm run lint` to catch style violations
- Build project with `npm run build` to verify no compilation errors
- Test components in isolation using Storybook or similar tool
- Verify accessibility with automated and manual testing
