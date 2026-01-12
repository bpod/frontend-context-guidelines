---
description: "State management patterns for client state, server state, and state management libraries across frontend frameworks"
applyTo: "**/*.{ts,tsx,js,jsx,vue,svelte}"
---

# State Management

Comprehensive guidelines for managing application state in modern frontend applications, covering local state, global state, server state, and state management libraries.

## General Instructions

- Choose the right state management approach for your needs
- Prefer local state over global state when possible
- Separate server state from client state
- Use URL state for shareable, bookmarkable UI states
- Implement proper state typing with TypeScript
- Avoid prop drilling with context or state management libraries
- Optimize re-renders with proper memoization
- Keep state normalized to avoid duplication
- Implement optimistic updates for better UX

## Best Practices

### State Classification

**1. Local (Component) State**

- Owned and managed by a single component
- Not needed by other components
- Examples: form inputs, toggles, temporary UI state

**2. Global (Application) State**

- Shared across multiple components
- Persists across navigation
- Examples: user authentication, theme preferences, shopping cart

**3. Server State**

- Data fetched from external APIs
- Has loading, error, and success states
- Needs caching, revalidation, and synchronization
- Examples: user profiles, product lists, dashboard data

**4. URL State**

- Stored in URL parameters or hash
- Shareable and bookmarkable
- Examples: search queries, pagination, filters, selected tabs

**5. Form State**

- Managed by form libraries
- Includes values, validation, errors, touched fields
- Optimized for form-specific workflows

### When to Use Each State Type

```typescript
// ✅ Local State - Component-specific UI
function Accordion() {
  const [isOpen, setIsOpen] = useState(false);
  return <div>{/* accordion content */}</div>;
}

// ✅ Global State - App-wide settings
const useTheme = create((set) => ({
  theme: "light",
  toggleTheme: () =>
    set((state) => ({
      theme: state.theme === "light" ? "dark" : "light",
    })),
}));

// ✅ Server State - API data
const { data, isLoading, error } = useQuery({
  queryKey: ["users"],
  queryFn: fetchUsers,
});

// ✅ URL State - Shareable filters
const [searchParams, setSearchParams] = useSearchParams();
const page = searchParams.get("page") || "1";
```

## React State Management

### Local State with useState

```typescript
// Simple state
const [count, setCount] = useState(0);
const [isOpen, setIsOpen] = useState(false);

// Complex state objects
const [user, setUser] = useState({
  name: "",
  email: "",
  role: "user",
});

// Update specific property
const updateEmail = (email: string) => {
  setUser((prev) => ({ ...prev, email }));
};

// Lazy initialization (expensive computation)
const [data, setData] = useState(() => {
  return expensiveComputation();
});
```

### State Management with useReducer

```typescript
type State = {
  items: Item[];
  loading: boolean;
  error: string | null;
};

type Action =
  | { type: "FETCH_START" }
  | { type: "FETCH_SUCCESS"; payload: Item[] }
  | { type: "FETCH_ERROR"; payload: string }
  | { type: "ADD_ITEM"; payload: Item }
  | { type: "REMOVE_ITEM"; payload: string };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "FETCH_START":
      return { ...state, loading: true, error: null };
    case "FETCH_SUCCESS":
      return { ...state, loading: false, items: action.payload };
    case "FETCH_ERROR":
      return { ...state, loading: false, error: action.payload };
    case "ADD_ITEM":
      return { ...state, items: [...state.items, action.payload] };
    case "REMOVE_ITEM":
      return {
        ...state,
        items: state.items.filter((item) => item.id !== action.payload),
      };
    default:
      return state;
  }
}

function ItemList() {
  const [state, dispatch] = useReducer(reducer, {
    items: [],
    loading: false,
    error: null,
  });

  // Usage
  const handleFetch = async () => {
    dispatch({ type: "FETCH_START" });
    try {
      const items = await fetchItems();
      dispatch({ type: "FETCH_SUCCESS", payload: items });
    } catch (error) {
      dispatch({ type: "FETCH_ERROR", payload: error.message });
    }
  };
}
```

### Context for Prop Drilling

```typescript
// ThemeContext.tsx
type Theme = "light" | "dark";

type ThemeContextValue = {
  theme: Theme;
  setTheme: (theme: Theme) => void;
};

const ThemeContext = createContext<ThemeContextValue | undefined>(undefined);

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>("light");

  const value = useMemo(() => ({ theme, setTheme }), [theme]);

  return (
    <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error("useTheme must be used within ThemeProvider");
  }
  return context;
}

// Usage
function App() {
  return (
    <ThemeProvider>
      <Dashboard />
    </ThemeProvider>
  );
}

function ThemeToggle() {
  const { theme, setTheme } = useTheme();
  return (
    <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
      Toggle Theme
    </button>
  );
}
```

For Zustand, Redux Toolkit, and React Query patterns, see [instructions/frameworks/react/react.instructions.md](instructions/frameworks/react/react.instructions.md).

## Vue State Management

### Composition API with Composables

```typescript
// composables/useCounter.ts
import { ref, computed } from "vue";

export function useCounter(initialValue = 0) {
  const count = ref(initialValue);
  const doubleCount = computed(() => count.value * 2);

  function increment() {
    count.value++;
  }

  function decrement() {
    count.value--;
  }

  function reset() {
    count.value = initialValue;
  }

  return {
    count,
    doubleCount,
    increment,
    decrement,
    reset,
  };
}

// Usage in component
<script setup lang="ts">
  const {(count, doubleCount, increment)} = useCounter(10);
</script>;
```

For Pinia store patterns, see [instructions/frameworks/vue/vue.instructions.md](instructions/frameworks/vue/vue.instructions.md).

## Svelte State Management

For custom Svelte stores and examples, see [instructions/frameworks/svelte/svelte.instructions.md](instructions/frameworks/svelte/svelte.instructions.md).

## Server State with TanStack Query (React Query)

````typescript
// queries/users.ts
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

// Fetch users
export function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const response = await fetch("/api/users");
      if (!response.ok) throw new Error("Failed to fetch users");
      return response.json();
    },
    staleTime: 5 * 60 * 1000, // 5 minutes
    cacheTime: 10 * 60 * 1000, // 10 minutes
  });
}

// Fetch single user with dependencies
export function useUser(userId: string) {
  return useQuery({
    queryKey: ["users", userId],
    queryFn: async () => {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) throw new Error("Failed to fetch user");
      return response.json();
    },
    enabled: !!userId, // Only run if userId is truthy
  });
}

// Mutation with optimistic updates
export function useUpdateUser() {
  const queryClient = useQueryClient();

```typescript
// queries/users.ts
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

// Fetch users
export function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const response = await fetch("/api/users");
      if (!response.ok) throw new Error("Failed to fetch users");
      return response.json();
    },
    staleTime: 5 * 60 * 1000, // 5 minutes
    cacheTime: 10 * 60 * 1000, // 10 minutes
  });
}

// Fetch single user with dependencies
export function useUser(userId: string) {
  return useQuery({
    queryKey: ["users", userId],
    queryFn: async () => {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) throw new Error("Failed to fetch user");
      return response.json();
    },
    enabled: !!userId, // Only run if userId is truthy
  });
}

// Mutation with optimistic updates
export function useUpdateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (user: User) => {
      const response = await fetch(`/api/users/${user.id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user),
      });
      if (!response.ok) throw new Error("Failed to update user");
      return response.json();
    },
    // Optimistic update
    onMutate: async (newUser) => {
      // Cancel outgoing refetches
      await queryClient.cancelQueries({ queryKey: ["users", newUser.id] });

      // Snapshot previous value
      const previousUser = queryClient.getQueryData(["users", newUser.id]);

      // Optimistically update
      queryClient.setQueryData(["users", newUser.id], newUser);

      return { previousUser };
    },
    // Rollback on error
    onError: (err, newUser, context) => {
      queryClient.setQueryData(["users", newUser.id], context?.previousUser);
    },
    // Refetch after success or error
    onSettled: (data, error, variables) => {
      queryClient.invalidateQueries({ queryKey: ["users", variables.id] });
    },
  });
}

// Usage in component
function UserProfile({ userId }: { userId: string }) {
  const { data: user, isLoading, error } = useUser(userId);
  const updateUser = useUpdateUser();

  if (isLoading) return <Spinner />;
  if (error) return <Error message={error.message} />;

  const handleUpdate = (updates: Partial<User>) => {
    updateUser.mutate({ ...user, ...updates });
  };

  return <div>{/* user profile */}</div>;
}
````

## Common Patterns

### URL State Management

```typescript
// React with react-router
function ProductList() {
  const [searchParams, setSearchParams] = useSearchParams();

  const page = Number(searchParams.get("page")) || 1;
  const category = searchParams.get("category") || "all";
  const sort = searchParams.get("sort") || "name";

  const updateFilters = (updates: Record<string, string>) => {
    setSearchParams((prev) => {
      const newParams = new URLSearchParams(prev);
      Object.entries(updates).forEach(([key, value]) => {
        newParams.set(key, value);
      });
      return newParams;
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

## Validation and Verification

- Verify state updates trigger expected re-renders
- Test state persistence (localStorage, sessionStorage)
- Validate TypeScript types for all state
- Check for memory leaks with DevTools
- Profile performance with framework DevTools
- Test optimistic updates and rollback scenarios
- Verify error handling and loading states
- Test state synchronization across tabs/windows
- Validate cache invalidation strategies
- Check for race conditions in async state updates
