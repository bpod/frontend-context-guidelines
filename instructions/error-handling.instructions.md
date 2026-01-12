---
description: "Error handling strategies for frontend applications including error boundaries, logging, user feedback, and graceful degradation"
applyTo: "**/*.{ts,tsx,js,jsx,vue,svelte}"
---

# Error Handling

Comprehensive guidelines for robust error handling in frontend applications, covering error boundaries, logging, user feedback, retry mechanisms, and graceful degradation.

## General Instructions

- Log errors to monitoring services for debugging
- Provide helpful, user-friendly error messages
- Implement retry mechanisms for transient failures
- Use fallback UI for graceful degradation
- Handle async errors in promises and async/await
- Validate and sanitize all external data
- Implement proper HTTP error handling
- Never expose sensitive information in error messages
- Test error scenarios systematically

## Framework-Specific Error Handling

For framework-specific error handling patterns:

- **React**: Error Boundaries and React 19 error handling in [instructions/frameworks/react/react.instructions.md](instructions/frameworks/react/react.instructions.md)
- **Vue**: Error handling patterns in [instructions/frameworks/vue/vue.instructions.md](instructions/frameworks/vue/vue.instructions.md)
- **Svelte**: Error handling patterns in [instructions/frameworks/svelte/svelte.instructions.md](instructions/frameworks/svelte/svelte.instructions.md)

## Best Practices

### Error Categories

**1. Syntax Errors**

- Caught during development/compilation
- Fixed before deployment
- TypeScript helps prevent many of these

**2. Runtime Errors**

- Null/undefined access
- Type mismatches
- Uncaught exceptions
- Require error boundaries and try-catch

**3. Network Errors**

- Failed API requests
- Timeout errors
- Network connectivity issues
- Require retry logic and offline handling

**4. Validation Errors**

- Invalid user input
- Data format mismatches
- Business rule violations
- Require user feedback and correction

**5. State Errors**

- Invalid state transitions
- Race conditions
- Stale data
- Require careful state management

## Async Error Handling

### Try-Catch with Async/Await

```typescript
async function fetchUserData(userId: string) {
  try {
    const response = await fetch(`/api/users/${userId}`);

    // Check HTTP status
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();

    // Validate response data
    if (!isValidUser(data)) {
      throw new Error("Invalid user data received");
    }

    return data;
  } catch (error) {
    if (error instanceof TypeError) {
      // Network error
      console.error("Network error:", error);
      throw new Error(
        "Unable to connect. Please check your internet connection."
      );
    }

    if (error instanceof SyntaxError) {
      // JSON parsing error
      console.error("Invalid JSON response:", error);
      throw new Error("Received invalid data from server.");
    }

    // Re-throw other errors
    throw error;
  }
}

// Usage in component
function UserProfile({ userId }: { userId: string }) {
  const [user, setUser] = useState<User | null>(null);
  const [error, setError] = useState<string | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let cancelled = false;

    fetchUserData(userId)
      .then((data) => {
        if (!cancelled) {
          setUser(data);
          setError(null);
        }
      })
      .catch((err) => {
        if (!cancelled) {
          setError(err.message);
          setUser(null);
        }
      })
      .finally(() => {
        if (!cancelled) {
          setLoading(false);
        }
      });

    return () => {
      cancelled = true;
    };
  }, [userId]);

  if (loading) return <Spinner />;
  if (error) return <ErrorMessage message={error} />;
  if (!user) return <NotFound />;

  return <div>{user.name}</div>;
}
```

### Error Handling with TanStack Query

```typescript
import { useQuery, useMutation } from "@tanstack/react-query";

// Query with error handling
function useUser(userId: string) {
  return useQuery({
    queryKey: ["users", userId],
    queryFn: async () => {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) {
        throw new Error(`Failed to fetch user: ${response.statusText}`);
      }
      return response.json();
    },
    retry: 3, // Retry failed requests 3 times
    retryDelay: (attemptIndex) => Math.min(1000 * 2 ** attemptIndex, 30000),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
}

// Component with error handling
function UserProfile({ userId }: { userId: string }) {
  const { data, error, isLoading, isError, refetch } = useUser(userId);

  if (isLoading) {
    return <Spinner />;
  }

  if (isError) {
    return (
      <div role="alert">
        <h3>Failed to load user</h3>
        <p>{error.message}</p>
        <button onClick={() => refetch()}>Try again</button>
      </div>
    );
  }

  return <div>{data.name}</div>;
}

// Mutation with error handling
function useUpdateUser() {
  return useMutation({
    mutationFn: async (user: User) => {
      const response = await fetch(`/api/users/${user.id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user),
      });

      if (!response.ok) {
        const error = await response.json();
        throw new Error(error.message || "Failed to update user");
      }

      return response.json();
    },
    onError: (error, variables, context) => {
      console.error("Update failed:", error);
      // Optionally show toast notification
      showToast({
        type: "error",
        message: error.message,
      });
    },
    onSuccess: (data) => {
      showToast({
        type: "success",
        message: "User updated successfully",
      });
    },
  });
}
```

## Error Logging and Monitoring

### Error Logging Service

```typescript
type ErrorLog = {
  message: string;
  stack?: string;
  level: "error" | "warning" | "info";
  context?: Record<string, any>;
  timestamp: string;
  userAgent: string;
  url: string;
  userId?: string;
};

class ErrorLogger {
  private endpoint = "/api/errors";
  private queue: ErrorLog[] = [];
  private flushInterval = 5000; // 5 seconds

  constructor() {
    // Flush queue periodically
    setInterval(() => this.flush(), this.flushInterval);

    // Flush on page unload
    window.addEventListener("beforeunload", () => this.flush());
  }

  log(
    error: Error | string,
    level: ErrorLog["level"] = "error",
    context?: Record<string, any>
  ) {
    const log: ErrorLog = {
      message: error instanceof Error ? error.message : error,
      stack: error instanceof Error ? error.stack : undefined,
      level,
      context,
      timestamp: new Date().toISOString(),
      userAgent: navigator.userAgent,
      url: window.location.href,
    };

    this.queue.push(log);

    // Also log to console in development
    if (process.env.NODE_ENV === "development") {
      console.error("[ErrorLogger]", log);
    }

    // Flush immediately for errors
    if (level === "error") {
      this.flush();
    }
  }

  private async flush() {
    if (this.queue.length === 0) return;

    const logs = [...this.queue];
    this.queue = [];

    try {
      await fetch(this.endpoint, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ logs }),
      });
    } catch (error) {
      // Failed to send logs, add back to queue
      this.queue.unshift(...logs);
    }
  }
}

export const errorLogger = new ErrorLogger();

// Usage
try {
  dangerousOperation();
} catch (error) {
  errorLogger.log(error, "error", {
    operation: "dangerousOperation",
    userId: currentUser.id,
  });
}
```

### Integration with Error Tracking Services

```typescript
// Sentry integration
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: process.env.REACT_APP_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  integrations: [new Sentry.BrowserTracing(), new Sentry.Replay()],
  tracesSampleRate: 1.0,
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
});

// Custom error logging with context
function logError(error: Error, context?: Record<string, any>) {
  Sentry.captureException(error, {
    tags: {
      component: context?.component,
      action: context?.action,
    },
    extra: context,
  });
}

// Set user context
Sentry.setUser({
  id: user.id,
  email: user.email,
  username: user.name,
});
```

## User-Friendly Error Messages

### Error Message Components

```typescript
type ErrorMessageProps = {
  error: Error | string;
  onRetry?: () => void;
  onDismiss?: () => void;
  type?: "error" | "warning" | "info";
};

function ErrorMessage({
  error,
  onRetry,
  onDismiss,
  type = "error",
}: ErrorMessageProps) {
  const message = error instanceof Error ? error.message : error;

  // Map technical errors to user-friendly messages
  const userMessage = getUserFriendlyMessage(message);

  return (
    <div role="alert" className={`alert alert-${type}`} aria-live="assertive">
      <div className="alert-icon">
        {type === "error" && <ErrorIcon />}
        {type === "warning" && <WarningIcon />}
        {type === "info" && <InfoIcon />}
      </div>

      <div className="alert-content">
        <h3 className="alert-title">
          {type === "error" && "Something went wrong"}
          {type === "warning" && "Warning"}
          {type === "info" && "Information"}
        </h3>
        <p>{userMessage}</p>
      </div>

      <div className="alert-actions">
        {onRetry && <button onClick={onRetry}>Try Again</button>}
        {onDismiss && (
          <button onClick={onDismiss} aria-label="Dismiss">
            ×
          </button>
        )}
      </div>
    </div>
  );
}

// Map technical errors to user-friendly messages
function getUserFriendlyMessage(technicalMessage: string): string {
  const errorMappings: Record<string, string> = {
    "Network Error":
      "Unable to connect. Please check your internet connection.",
    Unauthorized: "Your session has expired. Please log in again.",
    Forbidden: "You don't have permission to access this resource.",
    "Not Found": "The requested item could not be found.",
    "Internal Server Error":
      "Something went wrong on our end. Please try again later.",
    "Bad Gateway": "Service temporarily unavailable. Please try again later.",
  };

  // Find matching error pattern
  for (const [key, value] of Object.entries(errorMappings)) {
    if (technicalMessage.includes(key)) {
      return value;
    }
  }

  // Default message for unknown errors
  return "An unexpected error occurred. Please try again.";
}
```

### Error Toast Notifications

```typescript
import { toast } from "react-hot-toast";

// Success toast
toast.success("Profile updated successfully");

// Error toast
toast.error("Failed to save changes");

// Custom error toast with retry
toast.error(
  (t) => (
    <div>
      <p>Failed to load data</p>
      <button
        onClick={() => {
          toast.dismiss(t.id);
          refetch();
        }}
      >
        Retry
      </button>
    </div>
  ),
  { duration: 5000 }
);

// Promise-based toast
toast.promise(saveData(), {
  loading: "Saving...",
  success: "Saved successfully",
  error: "Failed to save",
});
```

## Retry Mechanisms

### Exponential Backoff Retry

```typescript
type RetryConfig = {
  maxRetries: number;
  initialDelay: number;
  maxDelay: number;
  backoffMultiplier: number;
};

async function fetchWithRetry<T>(
  fn: () => Promise<T>,
  config: RetryConfig = {
    maxRetries: 3,
    initialDelay: 1000,
    maxDelay: 30000,
    backoffMultiplier: 2,
  }
): Promise<T> {
  let lastError: Error;

  for (let attempt = 0; attempt <= config.maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error as Error;

      // Don't retry on last attempt
      if (attempt === config.maxRetries) {
        break;
      }

      // Don't retry on client errors (4xx)
      if (
        error instanceof Response &&
        error.status >= 400 &&
        error.status < 500
      ) {
        break;
      }

      // Calculate delay with exponential backoff
      const delay = Math.min(
        config.initialDelay * Math.pow(config.backoffMultiplier, attempt),
        config.maxDelay
      );

      console.log(`Retry attempt ${attempt + 1} after ${delay}ms`);

      // Wait before retrying
      await new Promise((resolve) => setTimeout(resolve, delay));
    }
  }

  throw lastError!;
}

// Usage
const data = await fetchWithRetry(() =>
  fetch("/api/data").then((r) => r.json())
);
```

### Conditional Retry

```typescript
function shouldRetry(error: Error, attempt: number): boolean {
  // Don't retry after max attempts
  if (attempt >= 3) return false;

  // Retry on network errors
  if (error instanceof TypeError) return true;

  // Retry on 5xx server errors
  if (error instanceof Response && error.status >= 500) return true;

  // Retry on specific errors
  if (error.message.includes("timeout")) return true;

  // Don't retry on client errors
  return false;
}

async function fetchWithConditionalRetry<T>(fn: () => Promise<T>): Promise<T> {
  let attempt = 0;

  while (true) {
    try {
      return await fn();
    } catch (error) {
      if (!shouldRetry(error as Error, attempt)) {
        throw error;
      }

      attempt++;
      const delay = 1000 * Math.pow(2, attempt);
      await new Promise((resolve) => setTimeout(resolve, delay));
    }
  }
}
```

## Graceful Degradation

### Feature Detection and Fallbacks

```typescript
function FeatureWithFallback() {
  const [supportsFeature, setSupportsFeature] = useState(false);

  useEffect(() => {
    // Check if feature is supported
    if ("IntersectionObserver" in window) {
      setSupportsFeature(true);
    }
  }, []);

  if (supportsFeature) {
    return <LazyLoadedComponent />;
  }

  // Fallback for unsupported browsers
  return <BasicComponent />;
}
```

### Offline Support

```typescript
import { useState, useEffect } from "react";

function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener("online", handleOnline);
    window.addEventListener("offline", handleOffline);

    return () => {
      window.removeEventListener("online", handleOnline);
      window.removeEventListener("offline", handleOffline);
    };
  }, []);

  return isOnline;
}

// Usage
function App() {
  const isOnline = useOnlineStatus();

  if (!isOnline) {
    return (
      <div role="alert">
        <h2>You're offline</h2>
        <p>Some features may be unavailable.</p>
      </div>
    );
  }

  return <MainApp />;
}
```

## Global Error Handlers

### Window Error Handler

```typescript
// Catch unhandled errors
window.addEventListener("error", (event) => {
  console.error("Unhandled error:", event.error);

  errorLogger.log(event.error, "error", {
    filename: event.filename,
    lineno: event.lineno,
    colno: event.colno,
  });

  // Prevent default error display
  event.preventDefault();
});

// Catch unhandled promise rejections
window.addEventListener("unhandledrejection", (event) => {
  console.error("Unhandled promise rejection:", event.reason);

  errorLogger.log(
    event.reason instanceof Error
      ? event.reason
      : new Error(String(event.reason)),
    "error",
    { type: "unhandledRejection" }
  );

  event.preventDefault();
});
```

## Validation and Verification

- Test error boundaries with intentional errors
- Verify error messages are user-friendly
- Test retry logic with network failures
- Verify error logging to monitoring services
- Test offline functionality
- Verify graceful degradation in unsupported browsers
- Test error recovery flows
- Verify no sensitive data in error messages
- Test error handling in async operations
- Verify accessibility of error messages (ARIA live regions)
