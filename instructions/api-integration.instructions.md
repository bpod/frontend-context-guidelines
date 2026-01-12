---
description: "API integration patterns for HTTP clients, error handling, retry logic, request/response transformation, and data fetching"
applyTo: "**/*.{js,jsx,ts,tsx,vue,svelte}"
---

# API Integration

Guidelines for integrating with HTTP APIs including client setup, error handling, retry logic, caching, and data transformation.

## General Instructions

- Create typed API clients with clear contracts
- Implement comprehensive error handling
- Use appropriate HTTP methods and status codes
- Handle loading, error, and success states
- Implement retry logic with exponential backoff
- Cache responses appropriately
- Transform API responses to match application models
- Never expose secrets in client-side code

## Best Practices

### HTTP Client Setup

- Use modern HTTP clients (fetch, axios, ky) consistently across the project
- Create a centralized API client configuration
- Configure base URL from environment variables
- Set default headers (Content-Type, Accept)
- Implement request/response interceptors for cross-cutting concerns
- Configure timeouts to prevent hanging requests
- Use proper CORS configuration when needed
- Implement proper TypeScript typing for requests and responses

### Request Patterns

- Use appropriate HTTP methods (GET, POST, PUT, PATCH, DELETE)
- Send proper Content-Type headers for request body
- Implement request body validation before sending
- Use query parameters for filtering, sorting, pagination
- Send authentication tokens in headers (not query params)
- Implement request cancellation for aborted operations
- Handle file uploads with proper multipart/form-data
- Batch related requests when API supports it

### Response Handling

- Check HTTP status codes and handle appropriately
- Parse response bodies based on Content-Type
- Transform API responses to application-specific types
- Handle empty responses (204 No Content)
- Extract and handle API error messages
- Validate response data against expected schema
- Handle pagination metadata (total, page, limit)
- Normalize nested response data when needed

### Error Handling

- Catch and handle network errors (offline, timeout, DNS)
- Handle HTTP error responses (4xx, 5xx) appropriately
- Provide meaningful error messages to users
- Log errors for debugging and monitoring
- Distinguish between client errors (4xx) and server errors (5xx)
- Handle authentication errors (401, 403) with proper redirects
- Implement global error handling for unhandled API errors
- Show user-friendly error messages (not technical details)
- Provide retry options for recoverable errors

### Retry Logic

- Implement exponential backoff for failed requests
- Set maximum retry attempts (typically 3-5)
- Only retry idempotent requests (GET, PUT, DELETE)
- Don't retry on client errors (4xx) except 429 (Rate Limited)
- Retry on network errors and 5xx server errors
- Respect Retry-After header for rate limiting
- Implement circuit breaker pattern for repeatedly failing endpoints
- Provide feedback to users during retries

### Loading States

- Show loading indicators during API calls
- Implement skeleton screens for better perceived performance
- Handle concurrent requests appropriately
- Prevent duplicate requests for same resource
- Debounce rapid-fire requests (search, autocomplete)
- Implement optimistic updates for better UX
- Show progress for long-running operations
- Disable submit buttons during submission

### Caching Strategies

- Cache GET requests based on URL and query parameters
- Set appropriate cache expiration times
- Implement cache invalidation on mutations (POST, PUT, DELETE)
- Use ETags for conditional requests
- Implement stale-while-revalidate for non-critical data
- Use React Query, SWR, or similar for automatic caching
- Cache in-memory for session duration when appropriate
- Clear cache on authentication state changes

### Data Fetching Libraries

- Use React Query, SWR, Apollo Client, or similar for data fetching
- Leverage automatic caching and deduplication
- Implement proper key management for cache invalidation
- Use optimistic updates for immediate UI feedback
- Handle background refetching appropriately
- Configure stale time and cache time properly
- Implement pagination and infinite scroll patterns
- Handle dependent queries correctly

### Authentication

- Store tokens securely (httpOnly cookies preferred)
- Refresh tokens before expiration
- Handle token refresh during active requests
- Redirect to login on authentication errors
- Include auth tokens in request headers
- Implement proper logout with token invalidation
- Handle concurrent requests during token refresh
- Clear sensitive data on logout

### Request Cancellation

- Cancel requests when component unmounts
- Cancel previous search requests when new search starts
- Use AbortController for fetch API
- Use axios cancel tokens or similar
- Handle cancellation errors gracefully
- Clean up pending requests on navigation
- Implement request deduplication when appropriate

### Type Safety

- Define TypeScript interfaces for request payloads
- Define TypeScript interfaces for response data
- Use generics for reusable API functions
- Validate API responses at runtime when critical
- Generate types from OpenAPI/Swagger specs when available
- Use discriminated unions for different response types
- Type error responses separately from success responses

## Code Standards

### API Client Structure

```typescript
// api/client.ts
import axios from "axios";

const apiClient = axios.create({
  baseURL: process.env.REACT_APP_API_URL,
  timeout: 10000,
  headers: {
    "Content-Type": "application/json",
  },
});

// Request interceptor
apiClient.interceptors.request.use(
  (config) => {
    const token = getAuthToken();
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor
apiClient.interceptors.response.use(
  (response) => response.data,
  async (error) => {
    if (error.response?.status === 401) {
      // Handle authentication error
      await refreshToken();
      return apiClient.request(error.config);
    }
    return Promise.reject(error);
  }
);

export default apiClient;
```

### Typed API Service

```typescript
// api/userService.ts
interface User {
  id: string;
  name: string;
  email: string;
}

interface CreateUserRequest {
  name: string;
  email: string;
  password: string;
}

interface ApiResponse<T> {
  data: T;
  message?: string;
}

export const userService = {
  async getUser(id: string): Promise<User> {
    const response = await apiClient.get<ApiResponse<User>>(`/users/${id}`);
    return response.data;
  },

  async createUser(userData: CreateUserRequest): Promise<User> {
    const response = await apiClient.post<ApiResponse<User>>(
      "/users",
      userData
    );
    return response.data;
  },

  async updateUser(id: string, userData: Partial<User>): Promise<User> {
    const response = await apiClient.put<ApiResponse<User>>(
      `/users/${id}`,
      userData
    );
    return response.data;
  },

  async deleteUser(id: string): Promise<void> {
    await apiClient.delete(`/users/${id}`);
  },
};
```

## Common Patterns

### Custom Hook for Data Fetching

```typescript
import { useState, useEffect } from "react";

interface FetchState<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
}

function useFetch<T>(url: string): FetchState<T> {
  const [state, setState] = useState<FetchState<T>>({
    data: null,
    loading: true,
    error: null,
  });

  useEffect(() => {
    const controller = new AbortController();

    async function fetchData() {
      try {
        const response = await fetch(url, { signal: controller.signal });

        if (!response.ok) {
          throw new Error(`HTTP error ${response.status}`);
        }

        const data = await response.json();
        setState({ data, loading: false, error: null });
      } catch (error) {
        if (error.name !== "AbortError") {
          setState({ data: null, loading: false, error: error as Error });
        }
      }
    }

    fetchData();

    return () => controller.abort();
  }, [url]);

  return state;
}
```

### Retry with Exponential Backoff

```typescript
async function fetchWithRetry<T>(
  url: string,
  options: RequestInit = {},
  maxRetries: number = 3
): Promise<T> {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(url, options);

      if (!response.ok) {
        // Don't retry on client errors (except 429)
        if (
          response.status >= 400 &&
          response.status < 500 &&
          response.status !== 429
        ) {
          throw new Error(`HTTP ${response.status}`);
        }

        // Last attempt, throw error
        if (attempt === maxRetries) {
          throw new Error(`HTTP ${response.status}`);
        }

        // Wait with exponential backoff
        const delay = Math.min(1000 * Math.pow(2, attempt), 10000);
        await new Promise((resolve) => setTimeout(resolve, delay));
        continue;
      }

      return await response.json();
    } catch (error) {
      if (attempt === maxRetries) {
        throw error;
      }

      // Network error, wait and retry
      const delay = Math.min(1000 * Math.pow(2, attempt), 10000);
      await new Promise((resolve) => setTimeout(resolve, delay));
    }
  }

  throw new Error("Max retries exceeded");
}
```

### React Query Integration

```typescript
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: () => userService.getUsers(),
    staleTime: 5 * 60 * 1000, // 5 minutes
    cacheTime: 10 * 60 * 1000, // 10 minutes
  });
}

function useCreateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: userService.createUser,
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ["users"] });
    },
  });
}
```

### Error Handling Component

```typescript
interface ApiErrorProps {
  error: Error;
  retry?: () => void;
}

function ApiError({ error, retry }: ApiErrorProps) {
  const isNetworkError =
    error.message.includes("network") || error.message.includes("fetch");

  return (
    <div className="error-container">
      <h3>Something went wrong</h3>
      <p>
        {isNetworkError
          ? "Unable to connect. Please check your internet connection."
          : "An error occurred. Please try again later."}
      </p>
      {retry && <button onClick={retry}>Try Again</button>}
    </div>
  );
}
```

### Optimistic Update

```typescript
function useUpdateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: userService.updateUser,
    onMutate: async (updatedUser) => {
      // Cancel outgoing refetches
      await queryClient.cancelQueries({ queryKey: ["users", updatedUser.id] });

      // Snapshot previous value
      const previousUser = queryClient.getQueryData(["users", updatedUser.id]);

      // Optimistically update
      queryClient.setQueryData(["users", updatedUser.id], updatedUser);

      return { previousUser };
    },
    onError: (err, updatedUser, context) => {
      // Rollback on error
      queryClient.setQueryData(
        ["users", updatedUser.id],
        context?.previousUser
      );
    },
    onSettled: (data, error, updatedUser) => {
      // Refetch after success or error
      queryClient.invalidateQueries({ queryKey: ["users", updatedUser.id] });
    },
  });
}
```

## Validation and Verification

- Test API integration with mock servers (MSW, json-server)
- Test error scenarios (network errors, timeouts, 4xx, 5xx)
- Verify proper error messages displayed to users
- Test request cancellation on component unmount
- Verify authentication flows including token refresh
- Test retry logic with network throttling
- Verify caching behavior with React Query DevTools
- Test loading and error states in UI
- Monitor API errors in production
