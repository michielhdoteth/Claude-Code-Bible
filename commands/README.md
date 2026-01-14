# Commands

Custom slash commands and CLI commands that streamline common workflows.

## What are Commands?

Commands are reusable templates for common tasks:
- **Slash Commands**: `/command-name` shortcuts
- **CLI Commands**: Command-line shortcuts
- **Script Commands**: Complex multi-step workflows

## Creating Commands

### Slash Commands

Create `.claude/commands/<command-name>.md`:

```markdown
# /review-code

Review code for best practices, security issues, and performance concerns.

## Usage

`/review-code [file|directory]`

## Steps

1. Read the target files
2. Check for common issues:
   - Security vulnerabilities
   - Performance anti-patterns
   - Code style violations
3. Provide actionable recommendations
```

### Command Structure

```markdown
# /command-name

Brief description of the command.

## Usage

`/command-name [arguments]`

## Description

Detailed description of what this command does.

## Examples

`/command-name feature-branch`
```

### Using Arguments

```markdown
# /fix-issue

Find and fix GitHub issues.

## Usage

`/fix-issue ISSUE_NUMBER`

## Steps

1. Fetch issue details from GitHub
2. Locate relevant code
3. Implement solution
4. Add tests
5. Create commit message referencing issue

Use $ARGUMENTS to inject user input:
```

## Example Commands

### Commit and Push

```markdown
# /commit-push

Commit all changes with a descriptive message and push.

## Usage

`/commit-push "Your commit message"`

## Steps

1. Stage all changes
2. Create commit with provided message
3. Push to current branch
```

### Code Review

```markdown
# /review-code

Review code in specified directory for quality issues.

## Usage

`/review-code [directory]`

## Focus Areas

- Code clarity and maintainability
- Security best practices
- Performance considerations
- Test coverage
```

### Documentation Update

```markdown
# /update-docs

Update documentation after code changes.

## Usage

`/update-docs [files]`

## Steps

1. Identify changed files needing docs
2. Update existing documentation
3. Generate API docs if needed
4. Verify documentation builds
```

### Feature Development

```markdown
# /feature

Implement a new feature following project conventions.

## Usage

`/feature "Feature description"`

## Steps

1. Create feature branch
2. Implement feature
3. Add tests
4. Update documentation
5. Create PR
```

## Command Directory Structure

```
.claude/
├── commands/
│   ├── commit-push.md
│   ├── review-code.md
│   ├── update-docs.md
│   └── feature.md
└── settings.json
```

## CLI Commands

### Installation

```bash
# Copy commands to global directory
cp commands/*.md ~/.claude/commands/

# Or project-specific
cp commands/*.md .claude/commands/
```

### Using Custom Commands

```bash
# In Claude Code session
claude /commit-push "Add new feature"

# Or create a command wrapper
echo '/commit-push "Add new feature"' | claude -p
```

## Advanced Commands

### Multi-step Workflows

```markdown
# /full-review

Complete code review workflow.

## Usage

`/full-review [target]`

## Steps

1. Run static analysis
2. Check test coverage
3. Review code style
4. Security audit
5. Generate report
```

### Conditional Commands

```markdown
# /test-deploy

Run tests and deploy if successful.

## Steps

1. Run test suite
2. If tests pass:
   - Build application
   - Deploy to staging
   - Run integration tests
3. Report results
```

## Command Templates

### Code Review Template

```markdown
# /code-review

Perform comprehensive code review.

## Review Checklist

- [ ] Code follows style guide
- [ ] Tests are adequate
- [ ] No security vulnerabilities
- [ ] Performance is acceptable
- [ ] Documentation updated

Provide a summary with:
1. Issues found (severity: high/medium/low)
2. Recommendations
3. Approval status
```

### Bug Fix Template

```markdown
# /fix-bug

Fix a bug following the workflow.

## Usage

`/fix-bug "Bug description"`

## Workflow

1. Understand the bug
2. Write failing test
3. Fix the bug
4. Verify test passes
5. Check for similar issues
```

## Best Practices

1. **Single Responsibility**: Each command should do one thing well
2. **Clear Names**: Use descriptive, memorable names
3. **Document Usage**: Explain how to use the command
4. **Provide Examples**: Show common use cases
5. **Keep Updated**: Maintain commands as project evolves

## Managing Commands

### List Available Commands

```bash
# View all commands
ls ~/.claude/commands/
```

### Update Commands

```bash
# Copy updated commands
cp new-commands/*.md ~/.claude/commands/

# Restart Claude Code to apply
```

### Share Commands

```bash
# Export for team sharing
tar -czf commands.tar.gz .claude/commands/
```

## Resources

- [Official Slash Commands](https://docs.anthropic.com/en/docs/claude-code/slash-commands)
- [Command Examples](https://github.com/topics/claude-code-commands)
