# How to Use Instructions

A practical guide for integrating, customizing, and creating instruction files for GitHub Copilot and AI coding assistants.

## Overview

Instruction files are markdown documents that provide context and guidelines to AI tools. They help AI understand your project's conventions, standards, and best practices, resulting in more consistent and project-appropriate code generation.

## Table of Contents

- [Adding Instructions to Your Project](#adding-instructions-to-your-project)
- [Customizing Existing Instructions](#customizing-existing-instructions)
- [Creating New Instructions](#creating-new-instructions)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Adding Instructions to Your Project

### Step 1: Choose Relevant Instructions

Browse the [instructions directory](../instructions/) and select files that match your technology stack:

| Your Stack | Recommended Instructions |
|------------|-------------------------|
| React + TypeScript | `reactjs.instructions.md`, `typescript-5-es2022.instructions.md` |
| Next.js + TypeScript | `nextjs.instructions.md`, `typescript-5-es2022.instructions.md` |
| Testing with Playwright | `playwright-typescript.instructions.md` |
| Accessibility focus | `a11y.instructions.md` |
| Security-conscious | `security-and-owasp.instructions.md` |

### Step 2: Copy to Your Project

Create the instructions directory and copy files:

```bash
# Navigate to your project root
cd /path/to/your/project

# Create the directory structure
mkdir -p .github/instructions

# Copy instruction files (adjust paths as needed)
cp /path/to/frontend-context-guidelines/instructions/reactjs.instructions.md .github/instructions/
cp /path/to/frontend-context-guidelines/instructions/typescript-5-es2022.instructions.md .github/instructions/
```

### Step 3: Verify Installation

1. Open VS Code in your project
2. Create a new file matching the instruction's `applyTo` pattern
3. Start typing a comment describing what you want
4. GitHub Copilot suggestions should reflect the instruction guidelines

**Example**: With `reactjs.instructions.md` active, typing `// Button component` should generate a functional component with proper TypeScript types.

## Customizing Existing Instructions

### When to Customize

Customize instructions when:
- Your team has specific conventions not covered by defaults
- You're using additional libraries or tools
- You need stricter or more lenient guidelines
- Your project has unique architectural patterns

### How to Customize

#### 1. Add Project-Specific Context

Open the instruction file and update the **Project Context** section:

```markdown
## Project Context

- React 18.2 with TypeScript 5.0
- Using Tailwind CSS for styling (NOT CSS Modules)
- Redux Toolkit for global state
- React Query for server state
- Storybook for component documentation
- Jest + React Testing Library for unit tests
```

#### 2. Define Team Conventions

Add a **Team Conventions** section:

```markdown
## Team Conventions

### File Naming
- Components: PascalCase (e.g., `UserProfile.tsx`)
- Utilities: camelCase (e.g., `formatDate.ts`)
- Hooks: camelCase with 'use' prefix (e.g., `useAuth.ts`)
- Test files: `*.test.tsx` or `*.spec.tsx`

### Import Order
1. React and third-party libraries
2. Internal components and utilities
3. Types and interfaces
4. Styles

### State Management
- Use `useState` for component-local state
- Use `useReducer` for complex component state
- Use Redux Toolkit for global application state
- Use React Query for server state (no Redux for API calls)
```

#### 3. Add Code Examples

Include examples that demonstrate your preferred patterns:

```markdown
## Preferred Patterns

### Component Structure

```typescript
// ✅ Good: Clear separation, proper typing
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

export const Button: React.FC<ButtonProps> = ({ 
  label, 
  onClick, 
  variant = 'primary',
  disabled = false 
}) => {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn-${variant}`}
    >
      {label}
    </button>
  );
};
```

```typescript
// ❌ Bad: Default export, inline types
export default ({ label, onClick, variant, disabled }: any) => {
  return <button onClick={onClick}>{label}</button>;
};
```
\`\`\`
```

#### 4. Specify What to Avoid

Be explicit about anti-patterns:

```markdown
## What to Avoid

- ❌ Default exports (use named exports)
- ❌ Any types (always provide explicit types)
- ❌ Class components (use functional components)
- ❌ Inline styles (use Tailwind classes)
- ❌ Direct DOM manipulation (use refs only when necessary)
- ❌ Prop drilling (use context or state management)
```

## Creating New Instructions

### Step 1: Identify the Need

Create new instruction files for:
- New technologies or frameworks in your stack
- Specific domain knowledge (e.g., financial calculations, healthcare compliance)
- Custom design systems or component libraries
- Unique architectural patterns

### Step 2: Follow the Template

Create a new file: `.github/instructions/your-topic.instructions.md`

```markdown
---
description: "Brief description of what this instruction covers"
applyTo: "**/*.ts, **/*.tsx"  # Glob pattern for applicable files
---

# Your Topic Instructions

Brief introduction explaining the purpose and scope.

## Project Context

- Key technologies and versions
- Related tools and libraries
- Target environment (browser, Node.js, etc.)

## Development Standards

### Architecture
- High-level architectural patterns
- Component organization
- Data flow principles

### Coding Conventions
- Naming conventions
- File structure
- Import organization

### Best Practices
- Performance considerations
- Security guidelines
- Testing requirements

## Code Examples

### Good Examples
\`\`\`typescript
// ✅ Preferred pattern
\`\`\`

### Bad Examples
\`\`\`typescript
// ❌ Anti-pattern to avoid
\`\`\`

## Common Patterns

### Pattern 1: [Name]
Description and example

### Pattern 2: [Name]
Description and example

## Testing

- Testing approach
- Required test coverage
- Example test structure

## Documentation

- Documentation requirements
- Comment standards
- JSDoc/TSDoc patterns
```

### Step 3: Set Proper Scope

The `applyTo` field determines when the instruction is active:

```yaml
# Apply to all TypeScript files
applyTo: "**/*.ts, **/*.tsx"

# Apply only to components
applyTo: "src/components/**/*.tsx"

# Apply to test files
applyTo: "**/*.test.ts, **/*.spec.ts"

# Apply to specific feature
applyTo: "src/features/auth/**/*"

# Apply everywhere
applyTo: "**"
```

### Step 4: Test and Refine

1. **Create test files** that match the `applyTo` pattern
2. **Generate code** using GitHub Copilot
3. **Evaluate results**: Does the generated code follow your guidelines?
4. **Refine instructions**: Make them more specific if needed
5. **Iterate**: Test again until results are consistent

## Best Practices

### Keep Instructions Focused

**✅ Good**: One instruction file per major technology
```
typescript-5-es2022.instructions.md
reactjs.instructions.md
nextjs.instructions.md
```

**❌ Bad**: One mega-file with everything
```
frontend-everything.instructions.md  (500+ lines)
```

### Be Specific and Actionable

**✅ Good**:
```markdown
## Component Structure
- Always define TypeScript interfaces for props above the component
- Use named exports: `export const Button: React.FC<ButtonProps>`
- Destructure props in the function signature
```

**❌ Bad**:
```markdown
## Component Structure
- Write good components
- Use proper patterns
```

### Provide Examples

Always include code examples showing:
- ✅ What TO do
- ❌ What NOT to do

### Use Clear Language

Write instructions in:
- Simple, direct sentences
- Present tense
- Active voice
- Bullet points for easy scanning

### Limit Instruction Count

**Recommended**: 3-5 instruction files per project

More instructions = diluted focus for the AI. Start with core files and add more only when needed.

### Version Your Instructions

When your tech stack changes, update instructions accordingly:

```markdown
## Project Context

- React 19.0 (updated from 18.2 on 2026-01-01)
- Using Server Components (new architecture)
```

## Advanced Techniques

### Conditional Instructions

Use the `applyTo` field to create context-specific guidelines:

**File**: `src-components.instructions.md`
```yaml
---
description: "Guidelines for src/components directory"
applyTo: "src/components/**/*"
---
```

**File**: `pages-routing.instructions.md`
```yaml
---
description: "Next.js pages and routing"
applyTo: "src/app/**/*"
---
```

### Layered Instructions

Create a hierarchy of guidelines:

1. **Base**: `typescript-5-es2022.instructions.md` (applies to all `.ts` files)
2. **Framework**: `reactjs.instructions.md` (applies to all `.tsx` files)
3. **Specific**: `design-system.instructions.md` (applies to `src/components/**`)

The AI considers all applicable instructions together.

### Instruction Composition

Reference other instruction files for shared concepts:

```markdown
## Related Guidelines

This instruction builds on:
- [TypeScript Guidelines](typescript-5-es2022.instructions.md)
- [React Guidelines](reactjs.instructions.md)

Refer to those files for base conventions.
```

## Troubleshooting

### Problem: AI Doesn't Follow Instructions

**Possible causes**:
- Instructions are too vague
- Too many conflicting instructions
- `applyTo` pattern doesn't match your files
- Instructions contradict each other

**Solutions**:
1. Make instructions more specific with examples
2. Reduce to 3-5 core instruction files
3. Verify glob patterns match your file structure
4. Resolve contradictions by consolidating files

### Problem: Generated Code is Too Verbose

**Cause**: Instructions are too detailed or prescriptive

**Solution**: 
- Focus on principles, not implementation details
- Remove redundant or obvious guidelines
- Trust the AI for standard patterns

### Problem: Instructions Ignored for Certain Files

**Cause**: `applyTo` pattern doesn't match

**Solution**:
```yaml
# Check your pattern
applyTo: "**/*.tsx"  # Matches all .tsx files

# vs.
applyTo: "src/**/*.tsx"  # Only matches .tsx in src/
```

### Problem: Inconsistent Results

**Cause**: Instructions are ambiguous or conflicting

**Solution**:
- Be explicit about priorities
- Remove contradictions
- Add more examples

## Examples from Real Projects

### E-commerce Project

```markdown
# Instructions used:
- typescript-5-es2022.instructions.md
- reactjs.instructions.md
- nextjs.instructions.md
- a11y.instructions.md
- security-and-owasp.instructions.md

# Custom additions:
- product-catalog.instructions.md (domain-specific)
- checkout-flow.instructions.md (business logic)
```

### SaaS Dashboard

```markdown
# Instructions used:
- typescript-5-es2022.instructions.md
- reactjs.instructions.md
- playwright-typescript.instructions.md

# Custom additions:
- data-visualization.instructions.md (charts/graphs)
- api-integration.instructions.md (backend communication)
```

## Related Resources

- **[Instructions Reference](reference-instructions.md)**: Complete catalog of available instructions
- **[Creating Custom Instructions Prompt](../prompts/generate-custom-instructions-from-codebase.prompt.md)**: Auto-generate instructions from existing code
- **[Instructions for Instructions](../instructions/instructions.instructions.md)**: Meta-guide for writing instruction files

## Next Steps

- **[How to Use Prompts](how-to-use-prompts.md)**: Learn to leverage prompt templates
- **[Reference: All Instructions](reference-instructions.md)**: Browse the complete catalog
- **[Contributing](../CONTRIBUTING.md)**: Share your custom instructions with the community

---

**Questions?** Check [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions) or open an [issue](https://github.com/bpod/frontend-context-guidelines/issues).
