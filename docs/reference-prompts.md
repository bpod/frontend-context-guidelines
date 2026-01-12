# Prompts Reference

Complete catalog of all prompt templates available in this repository with descriptions, usage patterns, and examples.

## Overview

This reference provides detailed information about each prompt template. Each entry includes:
- **Purpose**: What the prompt accomplishes
- **When to use**: Ideal scenarios
- **Required input**: Information needed
- **Output**: What gets generated
- **Category**: Prompt classification

## Quick Reference Table

| Prompt | Category | Complexity | Agent Mode |
|--------|----------|------------|------------|
| [github-copilot-starter](#github-copilot-starter) | Setup | ⭐⭐⭐⭐ | ✅ |
| [architecture-blueprint-generator](#architecture-blueprint-generator) | Documentation | ⭐⭐⭐⭐ | ✅ |
| [folder-structure-blueprint-generator](#folder-structure-blueprint-generator) | Setup | ⭐⭐⭐ | Optional |
| [documentation-writer](#documentation-writer) | Documentation | ⭐⭐⭐ | ✅ |
| [readme-blueprint-generator](#readme-blueprint-generator) | Documentation | ⭐⭐⭐ | ✅ |
| [create-agentsmd](#create-agentsmd) | Documentation | ⭐⭐ | ✅ |
| [playwright-generate-test](#playwright-generate-test) | Code Gen | ⭐⭐⭐ | Optional |
| [review-and-refactor](#review-and-refactor) | Quality | ⭐⭐⭐⭐ | ✅ |
| [ai-prompt-engineering-safety-review](#ai-prompt-engineering-safety-review) | Quality | ⭐⭐⭐ | Optional |
| [prompt-builder](#prompt-builder) | Meta | ⭐⭐⭐ | ✅ |
| [generate-custom-instructions-from-codebase](#generate-custom-instructions-from-codebase) | Meta | ⭐⭐⭐⭐ | ✅ |
| [copilot-instructions-blueprint-generator](#copilot-instructions-blueprint-generator) | Setup | ⭐⭐⭐ | ✅ |
| [first-ask](#first-ask) | Utility | ⭐⭐ | Optional |
| [remember](#remember) | Utility | ⭐ | Optional |
| [model-recommendation](#model-recommendation) | Utility | ⭐ | Optional |

## Categories

- **Setup**: Project initialization and configuration
- **Documentation**: Creating or updating documentation
- **Code Gen**: Generating code artifacts
- **Quality**: Code review and improvement
- **Meta**: Creating prompts and instructions
- **Utility**: Helper prompts for workflows

---

## Project Setup Prompts

### github-copilot-starter

**File**: [github-copilot-starter.prompt.md](../prompts/github-copilot-starter.prompt.md)

**Purpose**: Create complete GitHub Copilot configuration for a new project based on technology stack.

**When to use**:
- Starting a new project
- Adding AI tooling to existing project
- Setting up team standards
- Configuring multi-language projects

**Required input**:
- Primary language/framework (e.g., JavaScript/React, Python/Django)
- Project type (web app, API, mobile app, library)
- Additional technologies (database, cloud provider, testing frameworks)
- Team size (solo, small team, enterprise)
- Development style (strict standards, flexible, specific patterns)

**Output**:
- `.github/copilot-instructions.md` (main configuration)
- `.github/instructions/` directory with multiple instruction files:
  - Language-specific guidelines
  - Testing standards
  - Documentation requirements
  - Security best practices
  - Performance optimization
  - Code review standards
- `.github/prompts/` directory with reusable prompts
- Customized for your specific stack

**Agent mode**: Yes (required)

**Example usage**:
```
@workspace Use /prompts/github-copilot-starter.prompt.md

Project details:
- Language: TypeScript
- Framework: Next.js 14 (App Router)
- Project type: E-commerce web application
- Stack: React, Tailwind CSS, PostgreSQL, Stripe
- Team: 5 developers
- Style: Strict standards with TypeScript and ESLint
```

**Configuration time**: 2-5 minutes

---

### folder-structure-blueprint-generator

**File**: [folder-structure-blueprint-generator.prompt.md](../prompts/folder-structure-blueprint-generator.prompt.md)

**Purpose**: Design optimal folder structure for your project based on best practices.

**When to use**:
- Project initialization
- Restructuring existing projects
- Establishing team conventions
- Planning scalable architecture

**Required input**:
- Project type and framework
- Scale (small, medium, large, enterprise)
- Team structure
- Architectural pattern preferences

**Output**:
- Detailed folder structure diagram
- Explanation of each directory's purpose
- File naming conventions
- Organization best practices
- Scalability considerations

**Example usage**:
```
@workspace Use /prompts/folder-structure-blueprint-generator.prompt.md

Create structure for:
- Next.js 14 with App Router
- Multi-tenant SaaS application
- Team of 8 developers
- Feature-based organization
- Include: API routes, authentication, admin panel, customer portal
```

---

### copilot-instructions-blueprint-generator

**File**: [copilot-instructions-blueprint-generator.prompt.md](../prompts/copilot-instructions-blueprint-generator.prompt.md)

**Purpose**: Generate a comprehensive `.github/copilot-instructions.md` file for your project.

**When to use**:
- Setting up main Copilot configuration
- Establishing project-wide guidelines
- Centralizing AI context

**Required input**:
- Project description and goals
- Technology stack
- Team preferences
- Coding standards

**Output**:
- `.github/copilot-instructions.md` file
- Project overview
- Technology stack documentation
- Coding standards
- Architecture overview

**Example usage**:
```
@workspace Use /prompts/copilot-instructions-blueprint-generator.prompt.md

Project: Customer relationship management (CRM) platform
Stack: React 19, Next.js 14, TypeScript, PostgreSQL, tRPC
Team guidelines: Functional programming preferred, comprehensive testing required
```

---

## Documentation Prompts

### documentation-writer

**File**: [documentation-writer.prompt.md](../prompts/documentation-writer.prompt.md)

**Purpose**: Create high-quality documentation following the Diátaxis framework (tutorials, how-tos, reference, explanation).

**When to use**:
- Documenting features or components
- Creating user guides
- Writing API documentation
- Building knowledge bases

**Required input**:
- Document type (tutorial, how-to, reference, explanation)
- Target audience
- User's goal
- Scope (what to include/exclude)

**Output**:
- Well-structured markdown documentation
- Appropriate for document type
- User-focused content
- Clear, consistent formatting

**Agent mode**: Yes (recommended)

**Example usage**:
```
@workspace Use /prompts/documentation-writer.prompt.md

Create a how-to guide for:
- Topic: Integrating Stripe payments
- Audience: Mid-level developers
- Goal: Successfully implement checkout flow
- Include: Setup, configuration, error handling, testing
- Exclude: Advanced webhook customization
```

**Workflow**:
1. Clarifies requirements through questions
2. Proposes document structure for approval
3. Generates complete documentation

---

### readme-blueprint-generator

**File**: [readme-blueprint-generator.prompt.md](../prompts/readme-blueprint-generator.prompt.md)

**Purpose**: Generate comprehensive README.md from project documentation and code analysis.

**When to use**:
- Creating new project README
- Updating outdated README
- Standardizing README format
- Documenting complex projects

**Required input**:
- Access to `.github/copilot/` directory
- `copilot-instructions.md` file
- Project files

**Output**:
- Complete README.md with sections:
  - Project description
  - Technology stack
  - Architecture overview
  - Getting started guide
  - Project structure
  - Key features
  - Development workflow
  - Coding standards
  - Testing approach
  - Contributing guidelines
  - License

**Example usage**:
```
@workspace Use /prompts/readme-blueprint-generator.prompt.md

Analyze the .github/copilot directory and generate a comprehensive README for this project.
```

---

### create-agentsmd

**File**: [create-agentsmd.prompt.md](../prompts/create-agentsmd.prompt.md)

**Purpose**: Create AGENTS.md file following the agents.md specification for coding agent context.

**When to use**:
- Adding agent-friendly documentation
- Improving AI tool understanding
- Standardizing project context
- Following agents.md standard

**Required input**:
- Repository access

**Output**:
- `AGENTS.md` file at repository root
- Project overview
- Architecture details
- Development guidelines
- Testing practices
- Following https://agents.md/ format

**Example usage**:
```
@workspace Use /prompts/create-agentsmd.prompt.md

Create AGENTS.md for this repository.
```

---

### architecture-blueprint-generator

**File**: [architecture-blueprint-generator.prompt.md](../prompts/architecture-blueprint-generator.prompt.md)

**Purpose**: Generate comprehensive architecture documentation by analyzing codebase patterns.

**When to use**:
- Documenting existing architecture
- Onboarding new team members
- Architecture reviews
- Planning refactoring

**Required input**:
- Codebase access
- Optional: specific architectural focus areas

**Configuration options**:
- Project type (auto-detect or specify)
- Architecture pattern (auto-detect or specify)
- Diagram type (C4, UML, Flow, Component, or None)
- Detail level (High-level to Implementation-Ready)
- Include code examples (yes/no)
- Include implementation patterns (yes/no)
- Include decision records (yes/no)

**Output**:
- `Project_Architecture_Blueprint.md` file
- Architecture overview and principles
- Visual diagrams (if requested)
- Component documentation
- Implementation patterns
- Extension points
- Architectural decision records

**Agent mode**: Yes (recommended)

**Example usage**:
```
@workspace Use /prompts/architecture-blueprint-generator.prompt.md

Analyze the codebase and create comprehensive architecture documentation.
Focus on: Clean Architecture patterns, include C4 diagrams, comprehensive detail level.
```

---

## Code Generation Prompts

### playwright-generate-test

**File**: [playwright-generate-test.prompt.md](../prompts/playwright-generate-test.prompt.md)

**Purpose**: Generate comprehensive end-to-end tests using Playwright by analyzing application code.

**When to use**:
- Adding test coverage for new features
- Converting manual tests to automated tests
- Establishing testing patterns
- Testing user flows

**Required input**:
- Component or feature to test
- Test scenarios/user flows
- Expected behaviors

**Output**:
- Complete Playwright test file
- Proper test structure
- Semantic selectors
- Clear assertions
- Best practices applied

**Example usage**:
```
@workspace Use /prompts/playwright-generate-test.prompt.md

Generate tests for: src/components/ShoppingCart.tsx

Scenarios:
1. Add product to cart
2. Update product quantity
3. Remove product from cart
4. Apply discount code
5. Proceed to checkout

Include: data-testid selectors, error handling, edge cases
```

---

## Code Quality Prompts

### review-and-refactor

**File**: [review-and-refactor.prompt.md](../prompts/review-and-refactor.prompt.md)

**Purpose**: Comprehensive code review with refactoring suggestions based on project instructions.

**When to use**:
- Before merging pull requests
- During code review process
- Technical debt cleanup
- Enforcing coding standards

**Required input**:
- Files or directories to review
- `.github/instructions/` files present
- `.github/copilot-instructions.md` file

**Output**:
- Code review findings
- Refactoring suggestions
- Applied fixes (if approved)
- Maintains test coverage
- Follows project guidelines

**Agent mode**: Yes (required)

**Example usage**:
```
@workspace Use /prompts/review-and-refactor.prompt.md

Review and refactor: src/services/payment-processor.ts

Focus on:
- Code quality and maintainability
- Security issues
- Performance optimizations
- Testing coverage
```

**Process**:
1. Reviews all instruction files
2. Analyzes code against guidelines
3. Suggests refactorings
4. Applies changes if approved
5. Verifies tests still pass

---

### ai-prompt-engineering-safety-review

**File**: [ai-prompt-engineering-safety-review.prompt.md](../prompts/ai-prompt-engineering-safety-review.prompt.md)

**Purpose**: Review prompts for safety, security, bias, and ethical concerns.

**When to use**:
- Before deploying custom prompts
- Auditing AI integrations
- Ensuring responsible AI use
- Compliance requirements

**Required input**:
- Prompt files to review
- Context about prompt usage

**Output**:
- Safety assessment
- Security concerns identified
- Bias analysis
- Recommendations for improvements
- Risk mitigation strategies

**Example usage**:
```
@workspace Use /prompts/ai-prompt-engineering-safety-review.prompt.md

Review all prompts in .github/prompts/ for:
- Security vulnerabilities
- Potential bias
- Privacy concerns
- Ethical issues
```

---

## Meta Prompts

### prompt-builder

**File**: [prompt-builder.prompt.md](../prompts/prompt-builder.prompt.md)

**Purpose**: Guide users through creating high-quality GitHub Copilot prompts.

**When to use**:
- Creating new custom prompts
- Learning prompt engineering
- Standardizing team prompts
- Improving existing prompts

**Required input**:
- Task description
- Required tools
- Expected output
- Context requirements

**Output**:
- Well-structured prompt file
- Proper frontmatter
- Clear instructions
- Usage examples
- Best practices applied

**Agent mode**: Yes (recommended)

**Example usage**:
```
@workspace Use /prompts/prompt-builder.prompt.md

Help me create a prompt for:
- Task: Generate API endpoint documentation from OpenAPI specs
- Tools needed: file reading, code analysis
- Output: Markdown documentation with examples
- Should include: authentication, error codes, rate limits
```

---

### generate-custom-instructions-from-codebase

**File**: [generate-custom-instructions-from-codebase.prompt.md](../prompts/generate-custom-instructions-from-codebase.prompt.md)

**Purpose**: Analyze existing code and generate instruction files that match your project's patterns.

**When to use**:
- Adding AI tooling to mature projects
- Documenting existing conventions
- Extracting implicit standards
- Onboarding AI tools to legacy code

**Required input**:
- Directory or files to analyze
- Code patterns to extract
- Desired instruction scope

**Output**:
- Custom `.instructions.md` file
- Extracted patterns and conventions
- Code examples from your codebase
- Proper frontmatter

**Agent mode**: Yes (required)

**Example usage**:
```
@workspace Use /prompts/generate-custom-instructions-from-codebase.prompt.md

Analyze src/components/ and create instructions for:
- React component patterns
- Styling conventions
- State management approach
- File organization
- Naming conventions

Output file: .github/instructions/custom-components.instructions.md
```

---

## Utility Prompts

### first-ask

**File**: [first-ask.prompt.md](../prompts/first-ask.prompt.md)

**Purpose**: Interactive task refinement - gathers requirements before execution.

**When to use**:
- Complex tasks with unclear requirements
- Need to clarify scope
- Interactive planning
- Avoiding rework

**Required input**:
- High-level task description

**Output**:
- Refined task understanding
- Clear plan
- Approved approach
- Executed solution

**Prerequisites**: Requires Joyride extension

**Example usage**:
```
@workspace Use /prompts/first-ask.prompt.md

I need to refactor our authentication system to support OAuth2.
```

**Workflow**:
1. Asks clarifying questions interactively
2. Defines scope and deliverables
3. Proposes plan
4. Gets approval
5. Executes task

---

### remember / remember-interactive-programming

**File**: [remember.prompt.md](../prompts/remember.prompt.md), [remember-interactive-programming.prompt.md](../prompts/remember-interactive-programming.prompt.md)

**Purpose**: Maintain context across long coding sessions.

**When to use**:
- Multi-step features
- Complex refactoring
- Long development sessions
- Maintaining conversation state

**Required input**:
- Context to remember

**Output**:
- Persistent context throughout session

**Example usage**:
```
@workspace Use /prompts/remember.prompt.md

Remember that:
- We're using Redux Toolkit for state management
- API calls use tRPC
- All forms use React Hook Form
- Testing with Playwright
```

---

### model-recommendation

**File**: [model-recommendation.prompt.md](../prompts/model-recommendation.prompt.md)

**Purpose**: Get recommendations on which AI model to use for specific tasks.

**When to use**:
- Optimizing model selection
- Cost vs. quality decisions
- Task-specific recommendations
- Learning about model capabilities

**Required input**:
- Task description
- Priorities (speed, quality, cost)

**Output**:
- Model recommendation
- Reasoning
- Trade-offs explained

**Example usage**:
```
@workspace Use /prompts/model-recommendation.prompt.md

What model should I use for:
- Generating comprehensive API documentation
- Need high quality
- Speed is secondary
- One-time task
```

---

## Prompt Chaining Examples

### Complete Project Setup

```bash
# 1. Generate folder structure
@workspace Use /prompts/folder-structure-blueprint-generator.prompt.md

# 2. Set up Copilot configuration
@workspace Use /prompts/github-copilot-starter.prompt.md

# 3. Generate architecture docs
@workspace Use /prompts/architecture-blueprint-generator.prompt.md

# 4. Create README
@workspace Use /prompts/readme-blueprint-generator.prompt.md

# 5. Add AGENTS.md
@workspace Use /prompts/create-agentsmd.prompt.md
```

### Quality Improvement Sprint

```bash
# 1. Review and refactor codebase
@workspace Use /prompts/review-and-refactor.prompt.md

# 2. Generate missing tests
@workspace Use /prompts/playwright-generate-test.prompt.md

# 3. Review prompts for safety
@workspace Use /prompts/ai-prompt-engineering-safety-review.prompt.md

# 4. Update documentation
@workspace Use /prompts/documentation-writer.prompt.md
```

## Tips for Using Prompts

### 1. Be Specific

**❌ Vague**: Use the documentation prompt  
**✅ Specific**: Use /prompts/documentation-writer.prompt.md to create a tutorial for authentication setup

### 2. Provide Complete Context

Include all required information upfront to avoid back-and-forth.

### 3. Customize for Your Needs

Most prompts can be adapted. Don't hesitate to modify workflows or add constraints.

### 4. Chain Prompts

Complex tasks often require multiple prompts in sequence.

### 5. Save Common Workflows

Document your frequently used prompt chains in your project docs.

## Related Resources

- **[How to Use Prompts](how-to-use-prompts.md)**: Detailed guide for leveraging prompts
- **[Prompt Builder](../prompts/prompt-builder.prompt.md)**: Create custom prompts
- **[Instructions Reference](reference-instructions.md)**: Complement prompts with instructions

---

**Questions?** Check [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions) or open an [issue](https://github.com/bpod/frontend-context-guidelines/issues).
