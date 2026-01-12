# Frontend Context Guidelines

A comprehensive resource repository for front-end development teams to accelerate AI-assisted development with GitHub Copilot and other AI coding tools.

## Overview

This repository provides a curated collection of **instructions**, **prompts**, and **skills** designed to help development teams effectively leverage AI tools in their daily workflow. Whether you're starting a new project or enhancing an existing one, these resources provide the context and guidance needed for AI tools to generate high-quality, project-specific code.

## What's Inside

### 📋 Instructions
Reusable guideline files that teach AI tools about your project's standards, conventions, and best practices.

- **Language-specific standards**: TypeScript, React, Next.js
- **Domain-specific guidance**: Accessibility (a11y), security (OWASP), testing (Playwright)
- **Documentation standards**: Self-explanatory code, auto-updating docs

[View all instructions →](instructions/)

### 💬 Prompts
Pre-built prompt templates for common development tasks and workflows.

- **Project setup**: GitHub Copilot configuration, architecture blueprints
- **Code generation**: Test generation, documentation writing
- **Code quality**: Review and refactor, safety reviews
- **Meta prompts**: Prompt builder, instructions generator

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
├── instructions/          # Reusable guideline files
│   ├── typescript-5-es2022.instructions.md
│   ├── reactjs.instructions.md
│   ├── nextjs.instructions.md
│   ├── a11y.instructions.md
│   ├── security-and-owasp.instructions.md
│   └── ...
├── prompts/              # Pre-built prompt templates
│   ├── github-copilot-starter.prompt.md
│   ├── documentation-writer.prompt.md
│   ├── playwright-generate-test.prompt.md
│   └── ...
├── skills/               # Advanced workflow modules
│   ├── web-design-reviewer/
│   └── webapp-testing/
└── docs/                 # Comprehensive documentation
    ├── tutorial-getting-started.md
    ├── how-to-*.md
    ├── reference-*.md
    └── explanation-architecture.md
```

## Use Cases

### Setting Up a New React/Next.js Project
Use the [github-copilot-starter](prompts/github-copilot-starter.prompt.md) prompt to generate a complete GitHub Copilot configuration tailored to your stack.

### Maintaining Code Quality
Apply [reactjs](instructions/reactjs.instructions.md), [typescript](instructions/typescript-5-es2022.instructions.md), and [security](instructions/security-and-owasp.instructions.md) instructions to ensure consistent standards.

### Generating Tests
Use [playwright-generate-test](prompts/playwright-generate-test.prompt.md) to create comprehensive end-to-end tests from your application code.

### Code Review & Refactoring
Leverage [review-and-refactor](prompts/review-and-refactor.prompt.md) to analyze code quality and suggest improvements.

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

This repository focuses on front-end development with:

- **Languages**: TypeScript, JavaScript (ES2022+)
- **Frameworks**: React 19+, Next.js 14+
- **Testing**: Playwright, Jest
- **Standards**: Accessibility (WCAG), Security (OWASP), Performance

## License

MIT License - See [LICENSE](LICENSE) for details.

## Support

- **Issues**: Report bugs or request features via [GitHub Issues](https://github.com/bpod/frontend-context-guidelines/issues)
- **Discussions**: Join the conversation in [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions)

---

**Ready to accelerate your development workflow with AI?** Start with the [Getting Started Tutorial](docs/tutorial-getting-started.md)!
