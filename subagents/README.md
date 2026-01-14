# Subagents

Specialized AI agents that can be invoked for specific tasks. Subagents provide focused expertise and clean context for complex workflows.

## What are Subagents?

Subagents are specialized AI agents configured for specific tasks like code review, testing, debugging, security analysis, and more. They provide:
- **Focused Expertise**: Each subagent is optimized for a specific domain
- **Clean Context**: Fresh context for each task without conversation history pollution
- **Parallel Execution**: Run multiple specialized agents simultaneously
- **Reusable Workflows**: Share consistent workflows across your team

## Getting Started

### Basic Subagent Structure

```yaml
---
name: code-reviewer
description: Use for code review tasks
tools: Read, Edit, Grep, Bash
model: sonnet
---

You are an expert code reviewer. When reviewing code:
1. Check for security vulnerabilities
2. Look for performance issues
3. Verify code follows project conventions
4. Suggest improvements with examples
```

### Creating a Subagent

1. Create `.claude/agents/` directory in your project
2. Add a markdown file with the subagent definition
3. Invoke with: `Use the code-reviewer agent to review my changes`

## Common Subagents

### Code Reviewer

```yaml
---
name: code-reviewer
description: Automated code review with security focus
tools: Read, Edit, Grep, Bash, Glob
model: sonnet
---

You are a senior code reviewer. For each PR:
- Identify potential security issues
- Check for code smells and anti-patterns
- Verify adequate test coverage
- Suggest concrete improvements
```

### Test Runner

```yaml
---
name: test-runner
description: Run test suite and fix failures
tools: Bash, Read, Edit
model: haiku
---

You are a test specialist. When tests fail:
1. Run the test suite and capture failures
2. Analyze the root cause of each failure
3. Fix the failing tests
4. Verify all tests pass
```

### Debugger

```yaml
---
name: debugger
description: Debug errors and unexpected behavior
tools: Read, Edit, Bash, Grep
model: opus
---

You are an expert debugger. When invoked:
1. Capture error message and stack trace
2. Identify reproduction steps
3. Isolate the failure location
4. Implement minimal fix
5. Verify solution works
```

### Security Auditor

```yaml
---
name: security-auditor
description: Security analysis for authentication and authorization
tools: Read, Grep, Bash
model: opus
---

You are a security expert specializing in authentication vulnerabilities.
Check for:
- SQL injection
- XSS vulnerabilities
- Authentication bypass
- Authorization issues
- Sensitive data exposure
```

## Using Subagents

### Direct Invocation

```
Use the code-reviewer agent to check my recent changes
```

### In Commands

Create commands that invoke subagents:

```markdown
# /review
Use the code-reviewer agent to review changes since the last commit
```

### Programmatic Usage

```javascript
import { query } from '@anthropic-ai/claude-agent-sdk';

for await (const message of query({
  prompt: 'Review the authentication module',
  options: {
    agents: {
      'code-reviewer': {
        description: 'Security-focused code review',
        prompt: 'You are a security auditor...',
        tools: ['Read', 'Grep'],
        model: 'sonet'
      }
    }
  }
})) {
  console.log(message);
}
```

## Best Practices

1. **Keep subagents focused** - Single responsibility per subagent
2. **Limit tools** - Only grant tools needed for the task
3. **Use appropriate models** - Simple tasks use haiku, complex use opus
4. **Document clearly** - Description helps Claude decide when to use
5. **Test subagents** - Verify they work before team adoption

## Resources

- [Official Subagents Documentation](https://docs.anthropic.com/en/docs/claude-code/sub-agents)
- [Claude Code SDK](https://docs.anthropic.com/en/docs/claude-code/sdk)
