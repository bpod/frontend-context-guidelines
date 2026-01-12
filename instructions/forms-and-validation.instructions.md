---
description: "Form handling and validation patterns for accessible, user-friendly forms with proper error handling"
applyTo: "**/*.{ts,tsx,js,jsx,vue,svelte}"
---

# Forms and Validation

Comprehensive guidelines for building accessible, user-friendly forms with robust validation, error handling, and excellent user experience.

## General Instructions

- Implement both client-side and server-side validation
- Provide immediate, helpful feedback on validation errors
- Use semantic HTML form elements
- Ensure forms are fully keyboard accessible
- Associate labels with form controls
- Group related fields with fieldset/legend
- Implement progressive enhancement
- Show validation state clearly (error, success, warning)
- Preserve user input on errors
- Support autofill and password managers

## Best Practices

### Form Structure

**Semantic HTML:**

```html
<form>
  <!-- Text input with label -->
  <div>
    <label for="email">Email</label>
    <input
      type="email"
      id="email"
      name="email"
      required
      aria-describedby="email-error"
    />
    <span id="email-error" role="alert"></span>
  </div>

  <!-- Grouped related fields -->
  <fieldset>
    <legend>Shipping Address</legend>

    <label for="street">Street</label>
    <input type="text" id="street" name="street" />

    <label for="city">City</label>
    <input type="text" id="city" name="city" />

    <label for="zip">ZIP Code</label>
    <input type="text" id="zip" name="zip" pattern="[0-9]{5}" />
  </fieldset>

  <!-- Radio buttons -->
  <fieldset>
    <legend>Delivery Method</legend>
    <label>
      <input type="radio" name="delivery" value="standard" checked />
      Standard (5-7 days)
    </label>
    <label>
      <input type="radio" name="delivery" value="express" />
      Express (2-3 days)
    </label>
  </fieldset>

  <!-- Checkbox with description -->
  <label>
    <input
      type="checkbox"
      name="newsletter"
      aria-describedby="newsletter-desc"
    />
    Subscribe to newsletter
  </label>
  <span id="newsletter-desc"> Get weekly updates and special offers </span>

  <!-- Submit button -->
  <button type="submit">Submit</button>
</form>
```

### Input Types and Attributes

**Use Appropriate Input Types:**

```html
<!-- Email validation -->
<input type="email" placeholder="user@example.com" />

<!-- Phone number -->
<input type="tel" placeholder="(555) 123-4567" />

<!-- URL validation -->
<input type="url" placeholder="https://example.com" />

<!-- Number with constraints -->
<input type="number" min="1" max="10" step="1" />

<!-- Date/time -->
<input type="date" />
<input type="time" />
<input type="datetime-local" />

<!-- Password -->
<input type="password" autocomplete="current-password" />

<!-- Search -->
<input type="search" />

<!-- File upload -->
<input type="file" accept="image/*,.pdf" multiple />

<!-- Color picker -->
<input type="color" />

<!-- Range slider -->
<input type="range" min="0" max="100" value="50" />
```

**Helpful Attributes:**

```html
<!-- Autocomplete for better UX -->
<input type="text" name="name" autocomplete="name" />
<input type="email" name="email" autocomplete="email" />
<input type="tel" name="phone" autocomplete="tel" />

<!-- Input pattern -->
<input type="text" pattern="[A-Za-z]{3,}" title="At least 3 letters" />

<!-- Placeholder vs value -->
<input placeholder="Enter your name" />
<!-- Hint, not value -->
<input value="John Doe" />
<!-- Actual value -->

<!-- Required fields -->
<input required aria-required="true" />

<!-- Disabled vs readonly -->
<input disabled />
<!-- Not submitted -->
<input readonly />
<!-- Submitted but not editable -->

<!-- Input mode for mobile keyboards -->
<input inputmode="numeric" />
<!-- Numeric keyboard -->
<input inputmode="email" />
<!-- Email keyboard -->
<input inputmode="url" />
<!-- URL keyboard -->
```

## Validation Strategies

### Client-Side Validation

**HTML5 Validation:**

```html
<form>
  <input
    type="email"
    required
    minlength="5"
    maxlength="100"
    pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$"
  />

  <input
    type="password"
    required
    minlength="8"
    pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}"
    title="Must contain at least one number, one uppercase and lowercase letter, and at least 8 characters"
  />
</form>
```

**JavaScript Validation:**

```typescript
// Validation functions
const validators = {
  required: (value: string) => {
    return value.trim() !== "" || "This field is required";
  },

  email: (value: string) => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(value) || "Invalid email address";
  },

  minLength: (min: number) => (value: string) => {
    return value.length >= min || `Must be at least ${min} characters`;
  },

  maxLength: (max: number) => (value: string) => {
    return value.length <= max || `Must be at most ${max} characters`;
  },

  matches: (pattern: RegExp, message: string) => (value: string) => {
    return pattern.test(value) || message;
  },

  custom:
    (fn: (value: string) => boolean, message: string) => (value: string) => {
      return fn(value) || message;
    },
};

// Usage
function validateField(
  value: string,
  rules: Array<(v: string) => true | string>
) {
  for (const rule of rules) {
    const result = rule(value);
    if (result !== true) {
      return result; // Return error message
    }
  }
  return true; // Valid
}

// Example
const emailError = validateField(email, [
  validators.required,
  validators.email,
]);

const passwordError = validateField(password, [
  validators.required,
  validators.minLength(8),
  validators.matches(
    /(?=.*\d)(?=.*[a-z])(?=.*[A-Z])/,
    "Must contain uppercase, lowercase, and number"
  ),
]);
```

### Server-Side Validation

**Always validate on the server:**

```typescript
// Backend validation example (Node.js/Express)
import { z } from "zod";

const userSchema = z.object({
  name: z.string().min(2, "Name must be at least 2 characters"),
  email: z.string().email("Invalid email address"),
  age: z.number().min(18, "Must be at least 18 years old"),
  password: z
    .string()
    .min(8, "Password must be at least 8 characters")
    .regex(/[A-Z]/, "Must contain uppercase letter")
    .regex(/[a-z]/, "Must contain lowercase letter")
    .regex(/[0-9]/, "Must contain number"),
});

app.post("/api/users", async (req, res) => {
  try {
    // Validate request body
    const validated = userSchema.parse(req.body);

    // Additional business logic validation
    const existingUser = await db.users.findByEmail(validated.email);
    if (existingUser) {
      return res.status(400).json({
        errors: {
          email: "Email already registered",
        },
      });
    }

    // Create user
    const user = await db.users.create(validated);
    res.json({ user });
  } catch (error) {
    if (error instanceof z.ZodError) {
      // Format validation errors
      const errors = error.errors.reduce((acc, err) => {
        const field = err.path.join(".");
        acc[field] = err.message;
        return acc;
      }, {} as Record<string, string>);

      return res.status(400).json({ errors });
    }

    res.status(500).json({ error: "Internal server error" });
  }
});
```

## Framework-Specific Form Patterns

- React form patterns and React Hook Form examples live in [instructions/frameworks/react/react.instructions.md](instructions/frameworks/react/react.instructions.md)
- Vue Composition API form patterns live in [instructions/frameworks/vue/vue.instructions.md](instructions/frameworks/vue/vue.instructions.md)

## Vue Form Patterns

### Vue 3 with Composition API

```vue
<script setup lang="ts">
import { ref, reactive, computed } from "vue";

type FormData = {
  name: string;
  email: string;
  message: string;
};

const formData = reactive<FormData>({
  name: "",
  email: "",
  message: "",
});

const errors = reactive<Partial<Record<keyof FormData, string>>>({});
const touched = reactive<Partial<Record<keyof FormData, boolean>>>({});

const isValid = computed(() => {
  return (
    Object.keys(errors).length === 0 &&
    Object.values(formData).every((val) => val.trim() !== "")
  );
});

function validateField(field: keyof FormData): string | null {
  const value = formData[field];

  switch (field) {
    case "name":
      if (!value) return "Name is required";
      if (value.length < 2) return "Name must be at least 2 characters";
      return null;

    case "email":
      if (!value) return "Email is required";
      if (!/\S+@\S+\.\S+/.test(value)) return "Invalid email";
      return null;

    case "message":
      if (!value) return "Message is required";
      if (value.length < 10) return "Message must be at least 10 characters";
      return null;

    default:
      return null;
  }
}

function handleBlur(field: keyof FormData) {
  touched[field] = true;
  const error = validateField(field);

  if (error) {
    errors[field] = error;
  } else {
    delete errors[field];
  }
}

function handleInput(field: keyof FormData) {
  if (touched[field]) {
    const error = validateField(field);
    if (error) {
      errors[field] = error;
    } else {
      delete errors[field];
    }
  }
}

async function handleSubmit() {
  // Mark all as touched
  (Object.keys(formData) as Array<keyof FormData>).forEach((field) => {
    touched[field] = true;
    const error = validateField(field);
    if (error) {
      errors[field] = error;
    }
  });

  if (!isValid.value) return;

  try {
    await submitForm(formData);
    // Reset form
    Object.assign(formData, { name: "", email: "", message: "" });
    Object.keys(errors).forEach((key) => delete errors[key]);
    Object.keys(touched).forEach((key) => delete touched[key]);
  } catch (error) {
    console.error("Failed to submit form", error);
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <div>
      <label for="name">Name</label>
      <input
        type="text"
        id="name"
        v-model="formData.name"
        @blur="handleBlur('name')"
        @input="handleInput('name')"
        :aria-invalid="touched.name && !!errors.name"
        :aria-describedby="errors.name ? 'name-error' : undefined"
      />
      <span v-if="touched.name && errors.name" id="name-error" role="alert">
        {{ errors.name }}
      </span>
    </div>

    <div>
      <label for="email">Email</label>
      <input
        type="email"
        id="email"
        v-model="formData.email"
        @blur="handleBlur('email')"
        @input="handleInput('email')"
        :aria-invalid="touched.email && !!errors.email"
      />
      <span v-if="touched.email && errors.email" role="alert">
        {{ errors.email }}
      </span>
    </div>

    <div>
      <label for="message">Message</label>
      <textarea
        id="message"
        v-model="formData.message"
        @blur="handleBlur('message')"
        @input="handleInput('message')"
        :aria-invalid="touched.message && !!errors.message"
        rows="4"
      />
      <span v-if="touched.message && errors.message" role="alert">
        {{ errors.message }}
      </span>
    </div>

    <button type="submit" :disabled="!isValid">Submit</button>
  </form>
</template>
```

## Accessible Error Messaging

### Error Display Patterns

```typescript
// Error summary at top of form
function FormErrorSummary({ errors }: { errors: Record<string, string> }) {
  const errorKeys = Object.keys(errors);

  if (errorKeys.length === 0) return null;

  return (
    <div role="alert" aria-live="assertive" className="error-summary">
      <h2>Please fix the following errors:</h2>
      <ul>
        {errorKeys.map((key) => (
          <li key={key}>
            <a href={`#${key}`}>{errors[key]}</a>
          </li>
        ))}
      </ul>
    </div>
  );
}

// Inline error with icon
function InputError({ message }: { message: string }) {
  return (
    <span role="alert" className="input-error">
      <svg aria-hidden="true" focusable="false">
        <use xlinkHref="#icon-error" />
      </svg>
      {message}
    </span>
  );
}

// Live region for dynamic errors
function LiveErrorRegion({ error }: { error: string | null }) {
  return (
    <div
      role="status"
      aria-live="polite"
      aria-atomic="true"
      className="sr-only"
    >
      {error}
    </div>
  );
}
```

## Advanced Patterns

### Multi-Step Forms

```typescript
function MultiStepForm() {
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState({
    // Step 1
    name: "",
    email: "",
    // Step 2
    address: "",
    city: "",
    // Step 3
    cardNumber: "",
    cvv: "",
  });

  const totalSteps = 3;

  const handleNext = () => {
    if (validateCurrentStep()) {
      setStep((prev) => Math.min(prev + 1, totalSteps));
    }
  };

  const handlePrev = () => {
    setStep((prev) => Math.max(prev - 1, 1));
  };

  return (
    <form>
      <div role="region" aria-label={`Step ${step} of ${totalSteps}`}>
        <progress value={step} max={totalSteps} />

        {step === 1 && <PersonalInfo data={formData} onChange={setFormData} />}
        {step === 2 && <AddressInfo data={formData} onChange={setFormData} />}
        {step === 3 && <PaymentInfo data={formData} onChange={setFormData} />}

        <div>
          {step > 1 && (
            <button type="button" onClick={handlePrev}>
              Previous
            </button>
          )}

          {step < totalSteps ? (
            <button type="button" onClick={handleNext}>
              Next
            </button>
          ) : (
            <button type="submit">Complete</button>
          )}
        </div>
      </div>
    </form>
  );
}
```

### Async Validation

```typescript
// Debounced username availability check
import { useState, useEffect } from "react";
import { useDebouncedValue } from "./hooks";

function UsernameInput() {
  const [username, setUsername] = useState("");
  const [checking, setChecking] = useState(false);
  const [available, setAvailable] = useState<boolean | null>(null);

  const debouncedUsername = useDebouncedValue(username, 500);

  useEffect(() => {
    if (!debouncedUsername || debouncedUsername.length < 3) {
      setAvailable(null);
      return;
    }

    let cancelled = false;
    setChecking(true);

    checkUsername(debouncedUsername).then((isAvailable) => {
      if (!cancelled) {
        setAvailable(isAvailable);
        setChecking(false);
      }
    });

    return () => {
      cancelled = true;
    };
  }, [debouncedUsername]);

  return (
    <div>
      <label htmlFor="username">Username</label>
      <input
        type="text"
        id="username"
        value={username}
        onChange={(e) => setUsername(e.target.value)}
        aria-describedby="username-status"
      />
      <span id="username-status" role="status">
        {checking && "Checking availability..."}
        {!checking && available === true && "✓ Available"}
        {!checking && available === false && "✗ Already taken"}
      </span>
    </div>
  );
}
```

## Validation and Verification

- Test form submission with valid and invalid data
- Verify all validation errors display correctly
- Test keyboard navigation (Tab, Shift+Tab, Enter)
- Verify screen reader announces errors properly
- Test with browser autofill
- Validate data on server, never trust client-side only
- Test form preservation on errors
- Verify loading and disabled states
- Test with different input methods (touch, mouse, keyboard)
- Check focus management after validation errors
