# How to Use Prompts

A practical guide for leveraging prompt templates to accelerate complex development tasks with AI coding assistants.

## Overview

Prompts are structured templates that guide AI tools through specific workflows and tasks. Unlike instructions (which provide ongoing context), prompts are used on-demand to accomplish particular goals like generating tests, creating documentation, or setting up new projects.

## Table of Contents

- [Understanding Prompts](#understanding-prompts)
- [Basic Usage](#basic-usage)
- [Prompt Categories](#prompt-categories)
- [Advanced Techniques](#advanced-techniques)
- [Creating Custom Prompts](#creating-custom-prompts)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Understanding Prompts

### What Makes a Good Prompt?

A well-structured prompt includes:

1. **Frontmatter**: Metadata describing the prompt's purpose, required tools, and agent configuration
2. **Clear objective**: Explicit statement of what the prompt will accomplish
3. **Step-by-step workflow**: Ordered instructions for the AI to follow
4. **Context requirements**: What information the AI needs to succeed
5. **Output format**: Expected structure and format of results

### Prompt vs. Instruction

| Feature | Instruction | Prompt |
|---------|-------------|--------|
| **Purpose** | Ongoing context and guidelines | Specific task execution |
| **When used** | Automatically (based on file type) | On-demand (when invoked) |
| **Scope** | Broad (applies to all relevant files) | Narrow (accomplishes one task) |
| **Example** | "Always use TypeScript" | "Generate Playwright tests for this component" |

## Basic Usage

### Method 1: Using GitHub Copilot Chat

1. **Open Copilot Chat** (`Cmd/Ctrl + I` or click the Copilot icon)

2. **Reference the prompt** using `@workspace`:
   ```
   @workspace Use /prompts/documentation-writer.prompt.md to create a tutorial for the UserCard component
   ```

3. **Provide required context**: Mention files, features, or specifics the prompt needs:
   ```
   @workspace Use /prompts/playwright-generate-test.prompt.md to create tests for src/components/LoginForm.tsx
   ```

4. **Review and refine**: The AI will follow the prompt's workflow and generate results

### Method 2: Copy and Customize

1. **Open the prompt file** in your editor
2. **Copy the content** (excluding frontmatter if preferred)
3. **Paste into Copilot Chat** and customize for your specific need
4. **Execute** and review results

### Method 3: Using Chat Participants (Advanced)

Some prompts specify `agent: "agent"` in frontmatter, meaning they work best with the agent mode:

```
@workspace /new Use the github-copilot-starter prompt to set up this project
```

## Prompt Categories

### Project Setup Prompts

These help initialize and configure new projects.

#### [github-copilot-starter.prompt.md](../prompts/github-copilot-starter.prompt.md)

**Purpose**: Create complete GitHub Copilot configuration for a new project

**When to use**: Starting a new project or adding AI tooling to an existing one

**Required input**: 
- Primary language/framework
- Project type
- Additional technologies

**Example usage**:
```
@workspace Use /prompts/github-copilot-starter.prompt.md

My project uses:
- React 19 + TypeScript
- Next.js 14 (App Router)
- Tailwind CSS
- Playwright for testing
- PostgreSQL database
```

**What it creates**:
- `.github/copilot-instructions.md`
- Multiple instruction files in `.github/instructions/`
- Prompt templates in `.github/prompts/`

---

#### [architecture-blueprint-generator.prompt.md](../prompts/architecture-blueprint-generator.prompt.md)

**Purpose**: Generate comprehensive architecture documentation

**When to use**: Starting a new project or documenting existing architecture

**Example usage**:
```
@workspace Use /prompts/architecture-blueprint-generator.prompt.md to create architecture docs for our e-commerce platform
```

---

#### [folder-structure-blueprint-generator.prompt.md](../prompts/folder-structure-blueprint-generator.prompt.md)

**Purpose**: Design optimal folder structure for your project

**When to use**: Project initialization or restructuring

**Example usage**:
```
@workspace Use /prompts/folder-structure-blueprint-generator.prompt.md

Create structure for a Next.js 14 SaaS application with:
- App router
- Server and client components
- API routes
- Authentication
- Multi-tenant support
```

### Code Generation Prompts

These help generate specific types of code.

#### [playwright-generate-test.prompt.md](../prompts/playwright-generate-test.prompt.md)

**Purpose**: Generate comprehensive E2E tests using Playwright

**When to use**: Adding test coverage for new features or existing code

**Example usage**:
```
@workspace Use /prompts/playwright-generate-test.prompt.md

Generate tests for:
- File: src/components/ShoppingCart.tsx
- Test scenarios: Add item, remove item, update quantity, checkout flow
```

**Output**: Complete Playwright test file with proper selectors, assertions, and test organization

### Documentation Prompts

These assist with creating high-quality documentation.

#### [documentation-writer.prompt.md](../prompts/documentation-writer.prompt.md)

**Purpose**: Create structured documentation following Diátaxis framework

**When to use**: Documenting features, APIs, components, or workflows

**Example usage**:
```
@workspace Use /prompts/documentation-writer.prompt.md

Create a how-to guide for integrating Stripe payments in our checkout flow
```

**Output**: Well-structured documentation with tutorials, how-tos, reference, and explanations

---

#### [readme-blueprint-generator.prompt.md](../prompts/readme-blueprint-generator.prompt.md)

**Purpose**: Generate comprehensive README from project documentation

**When to use**: Creating or updating project README files

**Example usage**:
```
@workspace Use /prompts/readme-blueprint-generator.prompt.md

Scan .github/copilot/ directory and create a README
```

---

#### [create-agentsmd.prompt.md](../prompts/create-agentsmd.prompt.md)

**Purpose**: Create AGENTS.md file following the agents.md specification

**When to use**: Adding agent-friendly documentation to repository

**Example usage**:
```
@workspace Use /prompts/create-agentsmd.prompt.md
```

### Code Quality Prompts

These improve existing code quality and structure.

#### [review-and-refactor.prompt.md](../prompts/review-and-refactor.prompt.md)

**Purpose**: Comprehensive code review with refactoring suggestions

**When to use**: Before merging, during code review, or for technical debt cleanup

**Example usage**:
```
@workspace Use /prompts/review-and-refactor.prompt.md

Review src/services/payment-processor.ts for:
- Code quality
- Security issues
- Performance improvements
- Test coverage
```

---

#### [ai-prompt-engineering-safety-review.prompt.md](../prompts/ai-prompt-engineering-safety-review.prompt.md)

**Purpose**: Review prompts for safety, security, and ethical concerns

**When to use**: Before deploying custom prompts or AI integrations

**Example usage**:
```
@workspace Use /prompts/ai-prompt-engineering-safety-review.prompt.md

Review our custom prompts in .github/prompts/ for potential issues
```

### Meta Prompts

These help you create or improve prompts and instructions.

#### [prompt-builder.prompt.md](../prompts/prompt-builder.prompt.md)

**Purpose**: Guide creation of high-quality GitHub Copilot prompts

**When to use**: Creating new custom prompts for your team

**Example usage**:
```
@workspace Use /prompts/prompt-builder.prompt.md

Help me create a prompt for generating API endpoint documentation from OpenAPI specs
```

---

#### [generate-custom-instructions-from-codebase.prompt.md](../prompts/generate-custom-instructions-from-codebase.prompt.md)

**Purpose**: Analyze existing code and generate instruction files

**When to use**: Adding AI tooling to mature projects with established patterns

**Example usage**:
```
@workspace Use /prompts/generate-custom-instructions-from-codebase.prompt.md

Analyze src/components/ and create React component instructions matching our style
```

---

#### [copilot-instructions-blueprint-generator.prompt.md](../prompts/copilot-instructions-blueprint-generator.prompt.md)

**Purpose**: Generate .github/copilot-instructions.md file

**When to use**: Setting up main Copilot configuration for a project

**Example usage**:
```
@workspace Use /prompts/copilot-instructions-blueprint-generator.prompt.md

Create copilot-instructions.md for our React/Next.js e-commerce platform
```

### Utility Prompts

Helper prompts for specific workflows.

#### [first-ask.prompt.md](../prompts/first-ask.prompt.md)

**Purpose**: Interactive task refinement before execution

**When to use**: Complex tasks requiring clarification (requires Joyride extension)

**Example usage**:
```
@workspace Use /prompts/first-ask.prompt.md

I need to refactor our authentication system
```

**Note**: This prompt uses interactive input to gather requirements before proceeding.

---

#### [remember.prompt.md](../prompts/remember.prompt.md) & [remember-interactive-programming.prompt.md](../prompts/remember-interactive-programming.prompt.md)

**Purpose**: Maintain context across long coding sessions

**When to use**: Multi-step features or complex refactoring

---

#### [model-recommendation.prompt.md](../prompts/model-recommendation.prompt.md)

**Purpose**: Get recommendations on which AI model to use for specific tasks

**When to use**: Optimizing AI tool selection for different scenarios

## Advanced Techniques

### Chaining Prompts

Combine multiple prompts for complex workflows:

```
Step 1: Use architecture-blueprint-generator.prompt.md
Step 2: Use folder-structure-blueprint-generator.prompt.md
Step 3: Use github-copilot-starter.prompt.md
Step 4: Use documentation-writer.prompt.md
```

### Customizing Prompts In-Place

Modify prompts for your specific needs:

```
@workspace Use /prompts/playwright-generate-test.prompt.md with these modifications:

- Use data-testid selectors instead of role selectors
- Include screenshot capture on failure
- Add performance timing assertions
```

### Creating Prompt Libraries

Organize prompts by team or project:

```
.github/prompts/
├── standard/              # From this repository
│   ├── documentation-writer.prompt.md
│   └── playwright-generate-test.prompt.md
└── custom/               # Your team's custom prompts
    ├── api-endpoint-generator.prompt.md
    └── database-migration-creator.prompt.md
```

### Prompt Variables

Some prompts support variable substitution:

```
@workspace Use /prompts/documentation-writer.prompt.md

Variables:
- component_name: ShoppingCart
- component_path: src/components/ShoppingCart.tsx
- document_type: how-to-guide
```

## Creating Custom Prompts

### Step 1: Identify the Task

Good candidates for custom prompts:
- Repetitive multi-step tasks
- Complex workflows requiring consistency
- Domain-specific code generation
- Team-specific patterns

### Step 2: Define the Structure

Create a new file: `.github/prompts/your-prompt.prompt.md`

```markdown
---
agent: "agent"  # Use agent mode (optional)
tools: ["edit", "search", "runCommands"]  # Required tools
description: "Brief description of what this prompt does"
---

# Your Prompt Title

Clear statement of what this prompt accomplishes.

## Prerequisites

List what must be in place before using this prompt:
- Files that must exist
- Tools that must be installed
- Context that must be provided

## Workflow

Step-by-step instructions for the AI:

1. **Gather Context**
   - Scan specific files or directories
   - Ask for required information
   - Validate prerequisites

2. **Generate Output**
   - Create files following specific templates
   - Apply transformations
   - Follow patterns

3. **Verify Results**
   - Run tests
   - Check for errors
   - Validate output format

## Output Format

Describe the expected result:
- File structure
- Content format
- Success criteria

## Example Usage

Provide a concrete example of how to use this prompt.
```

### Step 3: Test and Refine

1. **Test with various inputs**: Try edge cases and different scenarios
2. **Refine instructions**: Make them clearer based on results
3. **Add examples**: Include successful usage examples
4. **Document limitations**: Be clear about what the prompt can't do

### Step 4: Share with Team

1. Add to your project's `.github/prompts/` directory
2. Document in your team's prompt reference
3. Gather feedback and iterate

## Best Practices

### Keep Prompts Focused

**✅ Good**: One prompt per specific task
```
playwright-generate-test.prompt.md
documentation-writer.prompt.md
api-generator.prompt.md
```

**❌ Bad**: One prompt that does everything
```
do-all-the-things.prompt.md
```

### Provide Clear Context Requirements

**✅ Good**:
```markdown
## Required Context

Provide the following when using this prompt:
- File path of the component to test
- User flows to cover
- Expected test coverage threshold
```

**❌ Bad**:
```markdown
## Context

Give me whatever information is needed.
```

### Include Examples

Always show example usage:

```markdown
## Example Usage

\`\`\`
@workspace Use /prompts/your-prompt.prompt.md

Target: src/components/LoginForm.tsx
Scenarios: 
- Valid login
- Invalid credentials
- Password reset flow
\`\`\`
```

### Handle Edge Cases

Account for common issues:

```markdown
## Troubleshooting

If the prompt fails to find files:
- Verify file paths are correct
- Ensure files are saved
- Check workspace is properly loaded
```

### Version Your Prompts

Track changes over time:

```markdown
---
description: "Generate Playwright tests (v2.0 - Added visual regression testing)"
---
```

## Troubleshooting

### Problem: Prompt Not Found

**Cause**: Incorrect file path or name

**Solution**:
```bash
# Verify the prompt exists
ls .github/prompts/your-prompt.prompt.md

# Use correct path in chat
@workspace Use /prompts/your-prompt.prompt.md
```

### Problem: Incomplete Output

**Cause**: Missing required context

**Solution**: Provide all required information upfront:
```
@workspace Use /prompts/playwright-generate-test.prompt.md

Component: src/components/ShoppingCart.tsx
Test scenarios:
1. Add product to cart
2. Remove product from cart
3. Update quantity
4. Proceed to checkout

Expected coverage: 90%+
```

### Problem: Prompt Ignores Instructions

**Cause**: Instructions are unclear or contradictory

**Solution**:
- Simplify the prompt
- Use numbered steps
- Be more explicit about requirements
- Remove ambiguous language

### Problem: Results Don't Match Expectations

**Cause**: Prompt lacks specificity

**Solution**: Add concrete examples in the prompt:
```markdown
## Expected Output Format

\`\`\`typescript
// Generated test should look like this:
import { test, expect } from '@playwright/test';

test.describe('ShoppingCart', () => {
  test('adds product to cart', async ({ page }) => {
    // Test implementation
  });
});
\`\`\`
```

## Real-World Examples

### Example 1: Setting Up New Feature

```
# Step 1: Architecture
@workspace Use /prompts/architecture-blueprint-generator.prompt.md

Create architecture for a real-time chat feature in our app

# Step 2: Folder Structure
@workspace Use /prompts/folder-structure-blueprint-generator.prompt.md

Organize the chat feature with WebSocket connections, message history, and typing indicators

# Step 3: Generate Tests
@workspace Use /prompts/playwright-generate-test.prompt.md

Create E2E tests for the chat feature covering message send/receive and real-time updates

# Step 4: Documentation
@workspace Use /prompts/documentation-writer.prompt.md

Create a how-to guide for implementing the chat feature
```

### Example 2: Code Quality Sprint

```
# Review all components
@workspace Use /prompts/review-and-refactor.prompt.md

Review all files in src/components/ for code quality, performance, and security

# Generate missing tests
@workspace Use /prompts/playwright-generate-test.prompt.md

Generate tests for components without test coverage

# Update documentation
@workspace Use /prompts/documentation-writer.prompt.md

Create reference documentation for all public components
```

### Example 3: Onboarding New Developer

```
# Generate README
@workspace Use /prompts/readme-blueprint-generator.prompt.md

# Create getting started guide
@workspace Use /prompts/documentation-writer.prompt.md

Create a tutorial for new developers to set up the project and make their first contribution
```

## Related Resources

- **[Prompts Reference](reference-prompts.md)**: Complete catalog of available prompts
- **[Prompt Builder](../prompts/prompt-builder.prompt.md)**: Create custom prompts
- **[How to Use Instructions](how-to-use-instructions.md)**: Complement prompts with instructions

## Next Steps

- **[How to Use Skills](how-to-use-skills.md)**: Learn about advanced workflow modules
- **[Reference: All Prompts](reference-prompts.md)**: Browse the complete catalog
- **[Contributing](../CONTRIBUTING.md)**: Share your custom prompts with the community

---

**Questions?** Check [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions) or open an [issue](https://github.com/bpod/frontend-context-guidelines/issues).
