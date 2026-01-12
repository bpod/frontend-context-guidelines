---
description: "Angular 18+ development standards for enterprise applications with TypeScript and RxJS"
applyTo: "**/*.ts, **/*.html, **/*.scss"
---

# Angular Development Instructions

Guidelines for building high-quality Angular applications following official Angular documentation at https://angular.io/docs.

## General Instructions

- Use Angular 18+ with standalone components as the primary pattern
- Implement TypeScript strict mode for type safety
- Use RxJS observables for reactive programming
- Organize features by domain/business logic
- Prioritize testability, maintainability, and performance
- Follow Angular style guide and naming conventions

## Best Practices

### Component Architecture

- Use standalone components (no NgModules) for new development
- Design components with single responsibility principle
- Implement smart (container) and dumb (presentational) component patterns
- Use proper component composition over complex templates
- Define clear component boundaries and communication patterns
- Use `@Input`, `@Output` for component communication
- Implement `OnInit` lifecycle hook for initialization logic

### TypeScript and Type Safety

- Use TypeScript strict mode with full type coverage
- Define explicit types for all variables, parameters, and return values
- Use interfaces for component inputs and service contracts
- Create shared type definitions in `types/` or `models/` directories
- Use discriminated unions for complex state shapes
- Avoid `any` type; use `unknown` with proper type narrowing

### Dependency Injection

- Register services at appropriate levels (component, feature, root)
- Use `providedIn: 'root'` for application-wide services
- Use constructor injection for dependencies
- Create service interfaces for better testability
- Implement proper service lifecycles and cleanup
- Use factory functions for complex service creation

### Reactive Programming with RxJS

- Use observables instead of promises for async operations
- Implement proper subscription management with `async` pipe or `takeUntilDestroyed()`
- Use RxJS operators (`map`, `filter`, `switchMap`, etc.) for transformations
- Avoid nested subscriptions; use higher-order operators
- Implement error handling with `catchError` operator
- Use `shareReplay()` for caching observable results
- Create reusable operators for common patterns

### Forms

- Use reactive forms (FormBuilder, FormControl) as primary approach
- Define form structure with strong typing
- Implement custom validators for business logic
- Handle form submission and error states properly
- Use async validators for server-side validation
- Implement proper accessibility attributes on form elements

### HTTP Communication

- Use `HttpClient` for API calls
- Create typed API services with proper error handling
- Implement interceptors for cross-cutting concerns (auth, logging)
- Use strong typing for request and response bodies
- Implement proper error handling and retry logic
- Handle loading states and race conditions

### State Management

- Use RxJS BehaviorSubject for simple state management
- Consider NgRx or Akita for complex state requirements
- Keep state immutable and use pure reducers
- Implement proper selectors for state access
- Avoid excessive state; keep local state local
- Use store effects for side effects and async operations

### Template Best Practices

- Use structural directives (`*ngIf`, `*ngFor`) appropriately
- Avoid complex logic in templates; use component methods
- Implement OnPush change detection strategy
- Use async pipe to manage subscriptions in templates
- Use proper event binding and two-way binding
- Implement proper accessibility attributes

### Testing

- Write unit tests for components using TestBed
- Test component logic, inputs, outputs, and lifecycle
- Mock services and dependencies appropriately
- Use async/fakeAsync for testing async operations
- Write tests for services with comprehensive coverage
- Test accessibility and keyboard navigation

### Routing

- Organize routes by feature
- Use lazy loading for feature modules
- Implement route guards for authentication/authorization
- Handle route parameters and query strings
- Implement proper navigation and back button handling
- Use resolver guards for data pre-fetching

### Styling

- Use component-scoped styles (StyleUrls or inline styles)
- Use CSS or SCSS consistently
- Implement responsive design with mobile-first approach
- Use CSS custom properties for theming
- Keep global styles minimal; prefer component styles
- Ensure proper ARIA attributes and semantic HTML

### Security

- Validate all user input on server side
- Use Angular's built-in sanitization for HTML content
- Implement CSRF protection
- Use HTTPS for all API calls
- Never expose secrets in client-side code
- Keep dependencies updated and monitor security advisories
- Use Content Security Policy headers

### Performance

- Implement OnPush change detection strategy
- Use lazy loading for routes and modules
- Implement virtual scrolling for large lists
- Use trackBy function in `*ngFor`
- Minimize bundle size through tree shaking
- Profile and optimize detection cycles

### Accessibility

- Use semantic HTML elements
- Implement proper ARIA attributes and roles
- Ensure keyboard navigation works throughout app
- Provide descriptive alt text for images
- Maintain proper color contrast ratios
- Test with screen readers and accessibility tools

## Code Standards

### Naming Conventions

- **Components**: kebab-case file names, PascalCase class names (e.g., `user-card.component.ts`)
- **Services**: kebab-case file names, PascalCase class names (e.g., `user.service.ts`)
- **Directives**: kebab-case file names (e.g., `highlight.directive.ts`)
- **Pipes**: kebab-case file names (e.g., `safe-html.pipe.ts`)
- **Modules**: kebab-case file names (e.g., `shared.module.ts`)
- **Variables/Functions**: camelCase
- **Types/Interfaces**: PascalCase
- **Constants**: UPPER_SNAKE_CASE

### File Organization

- Follow directory structure by feature
- Keep templates, styles, and logic together
- Create shared directory for cross-feature code
- Use barrel files (index.ts) for clean exports
- Keep component-related files in same directory

## Common Patterns

### Service with Observable Pattern

```typescript
export class UserService {
  private users$ = new BehaviorSubject<User[]>([]);
  public readonly users = this.users$.asObservable();

  constructor(private http: HttpClient) {}

  loadUsers(): Observable<User[]> {
    return this.http.get<User[]>("/api/users").pipe(
      tap((users) => this.users$.next(users)),
      catchError((err) => {
        console.error("Failed to load users", err);
        return of([]);
      })
    );
  }
}
```

### Reactive Form with Validation

```typescript
export class UserFormComponent implements OnInit {
  form: FormGroup;

  constructor(private fb: FormBuilder, private userService: UserService) {
    this.form = this.fb.group({
      name: ["", [Validators.required, Validators.minLength(3)]],
      email: ["", [Validators.required, Validators.email]],
      password: ["", [Validators.required, Validators.minLength(8)]],
    });
  }

  submit(): void {
    if (this.form.valid) {
      this.userService.createUser(this.form.value).subscribe(() => {
        // Handle success
      });
    }
  }
}
```

## Validation and Verification

- Run `npm run build` to verify compilation
- Run `npm run lint` to check code style
- Run `npm run test` for unit tests
- Run `npm run e2e` for end-to-end tests
- Check bundle size regularly
- Test with screen readers and accessibility tools
