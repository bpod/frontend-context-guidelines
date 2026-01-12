# Frontend Context Guidelines

A comprehensive resource repository for front-end development teams to accelerate AI-assisted development with GitHub Copilot and other AI coding tools.

## Overview

This repository provides a curated collection of **instructions**, **prompts**, and **skills** designed to help development teams effectively leverage AI tools in their daily workflow. Whether you're starting a new project or enhancing an existing one, these resources provide the context and guidance needed for AI tools to generate high-quality, project-specific code.

## What's Inside

### 📋 Instructions
Reusable guideline files that teach AI tools about your project's standards, conventions, and best practices.

- **Core Standards**: Performance optimization, API integration, TypeScript, testing patterns
- **Framework-Specific**: React, Next.js, Angular, Vue, Svelte with modern patterns
- **Domain Guidance**: Accessibility (a11y), security (OWASP), code commenting, documentation
- **Workflow Standards**: Auto-updating docs, self-explanatory code

[View all instructions →](instructions/)

### 💬 Prompts
Pre-built prompt templates for common development tasks and workflows.

- **Code generation**: Component generators, test generation, documentation writing
- **Code quality**: Component refactoring, review and safety audits
- **Project setup**: GitHub Copilot configuration, architecture blueprints, folder structure
- **Meta prompts**: Prompt builder, instructions generator, model recommendations

[View all prompts →](prompts/)

### 🎯 Skills
Specialized workflow modules that combine instructions and prompts for complex tasks.

- **Web Design Reviewer**: Visual inspection and design validation
- **WebApp Testing**: Automated testing workflows

[View all skills →](skills/)

## Quick Start

### For New Projects

1. **Choose your stack**: Identify the technologies you're using (React, Next.js, TypeScript, etc.)
2. **Copy relevant instructions**: Add instruction files to your project's `.github/instructions/` directory
3. **Use setup prompts**: Run the [github-copilot-starter](prompts/github-copilot-starter.prompt.md) to generate a complete configuration
4. **Start coding**: AI tools will now understand your project context and standards

### For Existing Projects

1. **Browse the catalog**: Review available [instructions](docs/reference-instructions.md), [prompts](docs/reference-prompts.md), and [skills](docs/reference-skills.md)
2. **Select what you need**: Copy relevant files to your project
3. **Customize**: Adapt the guidelines to match your specific requirements
4. **Integrate**: Follow the [integration guide](docs/how-to-use-instructions.md)

## Documentation

- **📖 [Getting Started Tutorial](docs/tutorial-getting-started.md)** - Step-by-step guide for first-time users
- **🔧 [How to Use Instructions](docs/how-to-use-instructions.md)** - Integrate and customize instruction files
- **💡 [How to Use Prompts](docs/how-to-use-prompts.md)** - Leverage prompt templates effectively
- **⚡ [How to Use Skills](docs/how-to-use-skills.md)** - Implement advanced workflow modules
- **📚 [Instructions Reference](docs/reference-instructions.md)** - Complete catalog of instruction files
- **📚 [Prompts Reference](docs/reference-prompts.md)** - Complete catalog of prompts
- **📚 [Skills Reference](docs/reference-skills.md)** - Complete catalog of skills
- **🧠 [Architecture & Philosophy](docs/explanation-architecture.md)** - Understanding the design approach

## Repository Structure

```
frontend-context-guidelines/
├── instructions/                    # Reusable guideline files
│   ├── Core (Framework-Agnostic)
│   │   ├── frontend-performance.instructions.md
│   │   ├── api-integration.instructions.md
│   │   ├── typescript-5-es2022.instructions.md
│   │   ├── a11y.instructions.md
│   │   ├── security-and-owasp.instructions.md
│   │   └── ...
│   └── frameworks/                  # Framework-specific guidelines
│       ├── react/
│       ├── nextjs/
│       ├── angular/
│       ├── vue/
│       └── svelte/
├── prompts/                         # Pre-built prompt templates
│   ├── Core
│   │   ├── component-refactor.prompt.md
│   │   ├── test-generator.prompt.md
│   │   ├── documentation-writer.prompt.md
│   │   └── ...
│   └── frameworks/                  # Framework-specific prompts
│       ├── react/
│       ├── nextjs/
│       ├── angular/
│       ├── vue/
│       └── svelte/
├── skills/                          # Advanced workflow modules
│   ├── web-design-reviewer/
│   └── webapp-testing/
└── docs/                            # Comprehensive documentation
    ├── tutorial-getting-started.md
    ├── how-to-*.md
    ├── reference-*.md
    └── explanation-architecture.md
```

## Use Cases

### Setting Up a New React/Next.js Project
Use the [github-copilot-starter](prompts/github-copilot-starter.prompt.md) prompt to generate a complete GitHub Copilot configuration tailored to your stack.

### Maintaining Code Quality
Apply framework-specific instructions ([React](instructions/frameworks/react/), [Next.js](instructions/frameworks/nextjs/), etc.) with [TypeScript](instructions/typescript-5-es2022.instructions.md), [performance](instructions/frontend-performance.instructions.md), and [security](instructions/security-and-owasp.instructions.md) standards.

### Generating Components
Use framework-specific component generators to create production-ready components with proper typing, accessibility, and patterns.

### Refactoring Existing Code
Leverage [component-refactor](prompts/component-refactor.prompt.md) to analyze and improve components for better patterns, performance, and accessibility.

### Generating Tests
Use [test-generator](prompts/test-generator.prompt.md) to create comprehensive tests following modern best practices.

### API Integration
Follow [api-integration](instructions/api-integration.instructions.md) patterns for proper error handling, retry logic, and data fetching.

### Performance Optimization
Apply [frontend-performance](instructions/frontend-performance.instructions.md) guidelines to optimize bundle size, loading times, and runtime performance.

### Code Review
Use [review-and-refactor](prompts/review-and-refactor.prompt.md) to analyze code quality and suggest improvements.

### Visual Design Validation
Deploy the [web-design-reviewer](skills/web-design-reviewer/) skill to automatically identify and fix UI/UX issues.

## Contributing

We welcome contributions from the community! Whether you want to add new instructions, prompts, or skills, please see our [Contributing Guide](CONTRIBUTING.md) for details on:

- Adding new instruction files
- Creating prompt templates
- Developing skills modules
- Improving documentation

## Best Practices

1. **Start small**: Begin with 2-3 instruction files relevant to your core stack
2. **Customize**: Adapt the guidelines to match your team's specific needs
3. **Iterate**: Refine instructions based on AI tool output quality
4. **Share**: Contribute improvements back to help the community
5. **Stay updated**: Regularly check for new instructions and prompts

## Technology Stack

This repository focuses on modern front-end development with:

- **Languages**: TypeScript, JavaScript (ES2022+)
- **Frameworks**: React 19+, Next.js 15+, Angular 18+, Vue 3, Svelte 4+
- **Testing**: Playwright, Jest, Vitest, React Testing Library
- **Standards**: Performance optimization, API integration, Accessibility (WCAG), Security (OWASP)

## License

MIT License - See [LICENSE](LICENSE) for details.

## Support

- **Issues**: Report bugs or request features via [GitHub Issues](https://github.com/bpod/frontend-context-guidelines/issues)
- **Discussions**: Join the conversation in [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions)

---

**Ready to accelerate your development workflow with AI?** Start with the [Getting Started Tutorial](docs/tutorial-getting-started.md)!
