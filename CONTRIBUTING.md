# Contributing to Frontend Context Guidelines

Thank you for your interest in contributing! This repository serves the front-end development community, and we welcome contributions from developers of all experience levels.

## Table of Contents

- [How to Contribute](#how-to-contribute)
- [Types of Contributions](#types-of-contributions)
- [Contribution Guidelines](#contribution-guidelines)
- [Submitting Changes](#submitting-changes)
- [Review Process](#review-process)
- [Community Standards](#community-standards)

## How to Contribute

### 1. Find Something to Work On

- **Browse open issues**: Check [GitHub Issues](https://github.com/bpod/frontend-context-guidelines/issues) for tasks
- **Suggest improvements**: Open an issue to discuss new ideas
- **Fix bugs**: Report and fix issues you encounter
- **Add resources**: Contribute new instructions, prompts, or skills

### 2. Set Up Your Environment

```bash
# Fork the repository on GitHub, then:

# Clone your fork
git clone https://github.com/YOUR-USERNAME/frontend-context-guidelines.git
cd frontend-context-guidelines

# Add upstream remote
git remote add upstream https://github.com/bpod/frontend-context-guidelines.git

# Create a branch for your changes
git checkout -b feature/your-feature-name
```

### 3. Make Your Changes

- Follow the guidelines below for your contribution type
- Test your changes
- Update documentation if needed

### 4. Submit a Pull Request

```bash
# Commit your changes
git add .
git commit -m "feat: add new React hooks instruction file"

# Push to your fork
git push origin feature/your-feature-name

# Open a pull request on GitHub
```

## Types of Contributions

### Adding Instructions

**When to add**: You have domain-specific or framework-specific guidelines that would benefit others.

**Requirements**:
1. Follow the [instruction file template](instructions/instructions.instructions.md)
2. Include proper YAML frontmatter
3. Provide clear, actionable guidance
4. Include code examples (good and bad)
5. Test with GitHub Copilot

**Template**:
```markdown
---
description: "Brief description of purpose"
applyTo: "**/*.ts, **/*.tsx"
---

# Instruction Title

Introduction explaining purpose and scope.

## Project Context
- Key technologies
- Target versions
- Related tools

## Development Standards

### Category 1
Guidelines with examples

### Category 2
Guidelines with examples

## Code Examples

### ✅ Good Example
\`\`\`typescript
// Preferred pattern
\`\`\`

### ❌ Bad Example
\`\`\`typescript
// Anti-pattern to avoid
\`\`\`

## Best Practices
- Practice 1
- Practice 2
```

**File location**: `instructions/your-topic.instructions.md`

**Naming convention**: `lowercase-with-hyphens.instructions.md`

**Testing checklist**:
- [ ] File matches `applyTo` pattern
- [ ] GitHub Copilot follows guidelines when file is active
- [ ] No conflicts with existing instructions
- [ ] Examples are accurate and tested

### Adding Prompts

**When to add**: You have a reusable workflow that others might need.

**Requirements**:
1. Include frontmatter with description and agent mode
2. Provide step-by-step workflow
3. Document required inputs
4. Include usage examples
5. Test end-to-end

**Template**:
```markdown
---
agent: "agent"  # or omit if not required
tools: ["edit", "search"]  # required tools
description: "Clear description of what this prompt accomplishes"
---

# Prompt Title

Clear statement of purpose.

## Prerequisites

List requirements:
- Requirement 1
- Requirement 2

## Required Input

What user must provide:
- Input 1
- Input 2

## Workflow

1. **Step 1: Name**
   - Detailed instructions
   - What AI should do

2. **Step 2: Name**
   - Detailed instructions
   - What AI should do

## Output Format

Describe expected results

## Example Usage

\`\`\`
@workspace Use /prompts/your-prompt.prompt.md

Specific example with actual values
\`\`\`

## Troubleshooting

Common issues and solutions
```

**File location**: `prompts/your-prompt.prompt.md`

**Naming convention**: `lowercase-with-hyphens.prompt.md`

**Testing checklist**:
- [ ] Prompt works with GitHub Copilot Chat
- [ ] All required tools available
- [ ] Clear, unambiguous instructions
- [ ] Handles edge cases
- [ ] Includes examples

### Adding Skills

**When to add**: You have a complex, multi-step workflow that requires iteration or external tools.

**Requirements**:
1. Create skill directory with proper structure
2. Write comprehensive SKILL.md
3. Include reference documentation
4. Provide helper scripts if needed
5. Test complete workflow

**Template**:
```markdown
skills/
└── your-skill-name/
    ├── SKILL.md              # Main documentation
    ├── references/           # Supporting docs
    │   ├── guide.md
    │   └── checklist.md
    └── helpers/             # Optional utilities
        └── helper.js
```

**SKILL.md structure**:
```markdown
---
name: your-skill-name
description: 'Brief description and trigger phrases'
---

# Skill Name

Purpose and capabilities.

## When to Use This Skill

- Scenario 1
- Scenario 2

## Prerequisites

- Required tools
- Required services
- Required permissions

## Core Capabilities

1. Capability 1
2. Capability 2

## Workflow Overview

Step-by-step process with diagrams if helpful

## Usage Examples

Concrete examples with expected outputs

## Guidelines

Best practices for using this skill

## Limitations

What the skill cannot do
```

**Testing checklist**:
- [ ] Complete workflow tested end-to-end
- [ ] Prerequisites clearly documented
- [ ] Edge cases handled
- [ ] Reference materials accurate
- [ ] Helper scripts work as expected

### Improving Documentation

**When to contribute**: You find unclear explanations, typos, or missing information.

**Types of documentation improvements**:
- Fix typos or grammatical errors
- Clarify confusing sections
- Add missing examples
- Update outdated information
- Improve formatting

**Process**:
1. Identify the issue
2. Make the correction
3. Verify links and examples still work
4. Submit pull request

### Reporting Issues

**When to report**: You find bugs, have questions, or want to suggest features.

**Good issue template**:
```markdown
### Description
Clear description of the issue or suggestion

### Steps to Reproduce (for bugs)
1. Step 1
2. Step 2
3. Expected vs actual behavior

### Environment
- GitHub Copilot version
- VS Code version
- Operating system

### Suggested Solution (optional)
Your ideas for fixing or implementing
```

## Contribution Guidelines

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Provide constructive feedback
- Assume good intentions
- Focus on what's best for the community

### Quality Standards

**All contributions must**:
- Follow existing patterns and conventions
- Include clear documentation
- Be tested (where applicable)
- Not break existing functionality

**Instructions must**:
- Have valid YAML frontmatter
- Include working code examples
- Be clear and actionable
- Avoid overly prescriptive rules

**Prompts must**:
- Have clear, step-by-step workflows
- Include usage examples
- Document prerequisites
- Handle common errors

**Skills must**:
- Have comprehensive documentation
- Include workflow diagrams
- Provide reference materials
- Be thoroughly tested

### Writing Style

**Do**:
- ✅ Use clear, simple language
- ✅ Provide concrete examples
- ✅ Explain the "why" behind guidelines
- ✅ Include both good and bad examples
- ✅ Use consistent formatting

**Don't**:
- ❌ Use jargon without explanation
- ❌ Make vague statements
- ❌ Copy content without attribution
- ❌ Include overly opinionated advice
- ❌ Assume prior knowledge

### File Naming

- Use lowercase with hyphens: `react-hooks.instructions.md`
- Be descriptive: `playwright-typescript.instructions.md` not `testing.instructions.md`
- Follow existing patterns

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```bash
# Types
feat:     New feature or resource
fix:      Bug fix or correction
docs:     Documentation only
refactor: Code restructuring
test:     Adding tests
chore:    Maintenance tasks

# Examples
feat: add Vue.js instruction file
fix: correct TypeScript example in reactjs instructions
docs: improve getting started tutorial
refactor: reorganize prompt templates
```

## Submitting Changes

### Pull Request Process

1. **Create a descriptive pull request**:
   ```markdown
   ## Description
   Brief description of changes
   
   ## Type of Change
   - [ ] New instruction file
   - [ ] New prompt
   - [ ] New skill
   - [ ] Documentation improvement
   - [ ] Bug fix
   
   ## Testing
   Describe how you tested the changes
   
   ## Related Issues
   Closes #123
   ```

2. **Ensure CI passes**: All automated checks must pass

3. **Respond to feedback**: Address reviewer comments promptly

4. **Keep it focused**: One pull request = one logical change

### Review Process

**What reviewers look for**:
- ✅ Follows contribution guidelines
- ✅ High quality and well-documented
- ✅ Tested and working
- ✅ Doesn't break existing functionality
- ✅ Adds clear value

**Timeline**:
- Initial review: Within 3-5 business days
- Follow-up reviews: Within 2 business days
- Merge: After approval and CI passes

**Approval requirements**:
- At least one maintainer approval
- All CI checks passing
- No unresolved review comments

## Getting Help

### Resources

- **[Documentation](docs/)**: Comprehensive guides
- **[GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions)**: Ask questions, share ideas
- **[GitHub Issues](https://github.com/bpod/frontend-context-guidelines/issues)**: Report bugs, request features

### Questions?

- **General questions**: Use [GitHub Discussions](https://github.com/bpod/frontend-context-guidelines/discussions)
- **Bug reports**: Open an [issue](https://github.com/bpod/frontend-context-guidelines/issues)
- **Security issues**: Email the maintainers directly (see repository security policy)

## Recognition

Contributors will be:
- Listed in repository contributors
- Credited in release notes
- Recognized in the community

Thank you for helping make AI-assisted development better for everyone! 🚀

## Community Standards

### Inclusive Environment

We are committed to providing a welcoming and inclusive environment for all contributors, regardless of:
- Experience level
- Gender identity and expression
- Sexual orientation
- Disability
- Personal appearance
- Body size
- Race or ethnicity
- Age
- Religion

### Unacceptable Behavior

The following behaviors are not tolerated:
- Harassment or discrimination
- Trolling or insulting comments
- Personal or political attacks
- Publishing others' private information
- Other conduct that could reasonably be considered inappropriate

### Enforcement

Violations of community standards may result in:
1. Warning
2. Temporary ban
3. Permanent ban

Report violations to the repository maintainers.

---

**Ready to contribute?** Start by browsing [open issues](https://github.com/bpod/frontend-context-guidelines/issues) or reading our [documentation](docs/)!
