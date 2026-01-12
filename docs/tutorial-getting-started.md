# Getting Started with Frontend Context Guidelines

Welcome! This tutorial will guide you through your first experience using this repository to enhance your development workflow with AI-assisted coding tools.

## What You'll Learn

By the end of this tutorial, you'll be able to:

1. Understand what instructions, prompts, and skills are
2. Set up your first project with GitHub Copilot instructions
3. Use a prompt template to generate code
4. Customize guidelines for your specific needs

**Time required**: ~15 minutes

## Prerequisites

Before you begin, ensure you have:

- ✅ GitHub Copilot or another AI coding assistant enabled in VS Code
- ✅ A front-end project (or willingness to create a sample project)
- ✅ Basic familiarity with Git and your project's technology stack
- ✅ Node.js and npm/yarn installed (for JavaScript/TypeScript projects)

## Step 1: Understanding the Building Blocks

### Instructions
Instructions are markdown files that teach AI tools about your project's standards and conventions. Think of them as a style guide that the AI reads before generating code.

**Example**: The [frameworks/react/react.instructions.md](../instructions/frameworks/react/react.instructions.md) file tells the AI to use functional components with hooks, proper TypeScript types, and modern React patterns.

### Prompts
Prompts are templates for specific tasks. They provide structured workflows that guide the AI through complex operations.

**Example**: The [playwright-generate-test.prompt.md](../prompts/playwright-generate-test.prompt.md) helps generate end-to-end tests by analyzing your application code.

### Skills
Skills are advanced modules that combine instructions, prompts, and workflows for specialized tasks.

**Example**: The [web-design-reviewer](../skills/web-design-reviewer/) skill can visually inspect your website and identify design issues.

## Step 2: Your First Instruction File

Let's add your first instruction file to a project.

### Option A: New Project Setup

1. **Create a new React project** (skip if you already have one):
   ```bash
   npx create-react-app my-ai-project --template typescript
   cd my-ai-project
   ```

2. **Create the instructions directory**:
   ```bash
   mkdir -p .github/instructions
   ```

3. **Copy a basic instruction file**:
   ```bash
   # From this repository's root
   cp instructions/frameworks/react/react.instructions.md .github/instructions/
   ```

4. **Open your project in VS Code**:
   ```bash
   code .
   ```

### Option B: Existing Project Setup

1. **Navigate to your project**:
   ```bash
   cd /path/to/your/project
   ```

2. **Create the instructions directory** (if it doesn't exist):
   ```bash
   mkdir -p .github/instructions
   ```

3. **Copy relevant instruction files** based on your stack:
   ```bash
   # For React projects
   cp /path/to/frontend-context-guidelines/instructions/frameworks/react/react.instructions.md .github/instructions/

   # For TypeScript projects
   cp /path/to/frontend-context-guidelines/instructions/typescript-5-es2022.instructions.md .github/instructions/

   # For Next.js projects
   cp /path/to/frontend-context-guidelines/instructions/nextjs.instructions.md .github/instructions/

   # For accessibility-focused projects
   cp /path/to/frontend-context-guidelines/instructions/a11y.instructions.md .github/instructions/
   ```

## Step 3: Test Your First Instruction

Now let's see the instruction in action!

1. **Create a new component file**:
   ```bash
   # In your project
   touch src/components/UserCard.tsx
   ```

2. **Open the file in VS Code** and type this comment:
   ```typescript
   // Create a UserCard component that displays user information
   ```

3. **Trigger GitHub Copilot** by pressing `Enter` and waiting for suggestions.

4. **Observe the results**: The AI should generate a functional React component using:
   - TypeScript interfaces for props
   - Functional component with proper typing
   - Modern React patterns (hooks if state is needed)
   - Clean, readable code structure

**Before adding instructions**, you might have seen:
```typescript
// Class components, inconsistent naming, missing types
```

**After adding instructions**, you should see:
```typescript
interface UserCardProps {
  name: string;
  email: string;
  avatar?: string;
}

export const UserCard: React.FC<UserCardProps> = ({ name, email, avatar }) => {
  return (
    <div className="user-card">
      {avatar && <img src={avatar} alt={name} />}
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
};
```

## Step 4: Using Your First Prompt

Let's use a prompt template to generate documentation.

1. **Copy the documentation writer prompt**:
   ```bash
   cp /path/to/frontend-context-guidelines/prompts/documentation-writer.prompt.md .github/prompts/
   ```

2. **Open GitHub Copilot Chat** in VS Code (usually `Cmd/Ctrl + I`)

3. **Reference the prompt** by typing:
   ```
   @workspace /prompts/documentation-writer.prompt.md

   Create a tutorial for the UserCard component
   ```

4. **Review the generated documentation**: The AI should create structured documentation following the Diátaxis framework with tutorials, how-to guides, reference material, and explanations.

## Step 5: Customize for Your Project

Now let's make it yours!

1. **Open** [.github/instructions/frameworks/react/react.instructions.md](.github/instructions/frameworks/react/react.instructions.md)

2. **Add project-specific context**. For example:
   ```markdown
   ## Project Context

   - Latest React version (React 19+)
   - TypeScript for type safety
   - Using Tailwind CSS for styling
   - Redux Toolkit for state management
   - React Query for server state
   ```

3. **Add team conventions**:
   ```markdown
   ## Team Conventions

   - Component files use PascalCase (e.g., UserCard.tsx)
   - Test files use .test.tsx suffix
   - Use named exports, not default exports
   - Prefix custom hooks with 'use' (e.g., useUserData)
   - Store shared types in src/types/
   ```

4. **Test the changes**: Create another component and see if the AI follows your custom guidelines.

## Step 6: Explore More Resources

Now that you've completed the basics, explore more capabilities:

### Try Different Instructions

- **[typescript-5-es2022.instructions.md](../instructions/typescript-5-es2022.instructions.md)**: Modern TypeScript patterns
- **[a11y.instructions.md](../instructions/a11y.instructions.md)**: Accessibility best practices
- **[security-and-owasp.instructions.md](../instructions/security-and-owasp.instructions.md)**: Security guidelines

### Use More Prompts

- **[github-copilot-starter.prompt.md](../prompts/github-copilot-starter.prompt.md)**: Complete project setup
- **[playwright-generate-test.prompt.md](../prompts/playwright-generate-test.prompt.md)**: E2E test generation
- **[review-and-refactor.prompt.md](../prompts/review-and-refactor.prompt.md)**: Code quality analysis

### Experiment with Skills

- **[web-design-reviewer](../skills/web-design-reviewer/)**: Visual design validation
- **[webapp-testing](../skills/webapp-testing/)**: Automated testing workflows

## Common Questions

### Q: How many instruction files should I use?

**A**: Start with 2-3 core files (e.g., language + framework + testing). Add more as needed. Too many can dilute the AI's focus.

### Q: Can I mix instructions from different sources?

**A**: Yes! These instructions are designed to be composable. Mix and match based on your stack.

### Q: How do I know if instructions are working?

**A**: Compare code suggestions before and after adding instructions. You should see:
- More consistent naming and patterns
- Better adherence to your tech stack conventions
- More complete and production-ready code

### Q: What if the AI ignores my instructions?

**A**: Try:
- Being more specific in the instruction file
- Reducing the number of instructions (focus on key points)
- Explicitly mentioning the guidelines in your prompts
- Checking that files match the `applyTo` glob patterns

## Next Steps

Congratulations! You've completed the getting started tutorial. Here's what to explore next:

1. **[How to Use Instructions](how-to-use-instructions.md)**: Deep dive into instruction file customization
2. **[How to Use Prompts](how-to-use-prompts.md)**: Master prompt templates for complex tasks
3. **[Reference: All Instructions](reference-instructions.md)**: Browse the complete catalog
4. **[Architecture & Philosophy](explanation-architecture.md)**: Understand the design principles

## Troubleshooting

### Instructions Not Being Applied

**Symptom**: AI generates code that doesn't follow your instructions.

**Solutions**:
- Verify instruction files are in `.github/instructions/`
- Check the `applyTo` glob pattern matches your files
- Restart VS Code to reload configurations
- Be more explicit in your code comments/prompts

### Prompts Not Working

**Symptom**: Prompt doesn't produce expected results.

**Solutions**:
- Ensure you're referencing the prompt correctly
- Provide all required context the prompt needs
- Check if the prompt requires specific tools or extensions
- Review the prompt's frontmatter for dependencies

### Need More Help?

- Review [How-to Guides](how-to-use-instructions.md) for specific scenarios
- Check [GitHub Issues](https://github.com/bpod/frontend-context-guidelines/issues) for known problems
- Join discussions in [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions)

---

**Ready for more?** Continue to [How to Use Instructions](how-to-use-instructions.md) to master instruction file customization!
