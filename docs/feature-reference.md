# Claude Code Complete Feature Reference

A comprehensive reference of all Claude Code features, commands, workflows, and best practices.

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [Slash Commands](#slash-commands)
3. [Subagents](#subagents)
4. [Hooks](#hooks)
5. [Model Context Protocol (MCP)](#model-context-protocol)
6. [Skills](#skills)
7. [Plugins](#plugins)
8. [Workflows](#workflows)
9. [Configuration](#configuration)
10. [CLI Reference](#cli-reference)
11. [Best Practices](#best-practices)

---

## Core Concepts

### What is Claude Code?

Claude Code is Anthropic's command-line tool for agentic coding. It enables:
- AI-assisted code generation, debugging, and refactoring
- Multi-step task automation
- Context-aware development with persistent memory
- Integration with external tools via MCP

### Key Capabilities

| Capability | Description |
|------------|-------------|
| **Agentic Coding** | Claude autonomously plans and executes complex tasks |
| **Context Preservation** | Sessions persist across work sessions |
| **Tool Integration** | MCP servers connect to external services |
| **Customization** | Slash commands, subagents, hooks, and plugins |
| **Headless Mode** | CI/CD integration for automated workflows |

---

## Slash Commands

### Built-in Commands (40+)

#### Session Management
| Command | Description |
|---------|-------------|
| `/add-dir` | Add additional working directories |
| `/agents` | Manage custom AI subagents |
| `/bashes` | List and manage background tasks |
| `/clear` | Clear conversation history |
| `/compact [instructions]` | Compact conversation with optional focus |
| `/config` | Open Settings interface |
| `/context` | Visualize current context usage |
| `/cost` | Show token usage statistics |
| `/doctor` | Check Claude Code health |
| `/exit` | Exit the REPL |
| `/export [filename]` | Export conversation to file |
| `/help` | Get usage help |
| `/hooks` | Manage hook configurations |
| `/ide` | Manage IDE integrations |
| `/init` | Initialize project with CLAUDE.md |
| `/install-github-app` | Set up GitHub Actions |
| `/login` | Switch Anthropic accounts |
| `/logout` | Sign out |
| `/mcp` | Manage MCP server connections |
| `/memory` | Edit CLAUDE.md memory files |
| `/model` | Select or change AI model |
| `/output-style [style]` | Set output style |
| `/permissions` | View or update permissions |
| `/plan` | Enter plan mode |
| `/plugin` | Manage plugins |
| `/pr-comments` | View pull request comments |
| `/privacy-settings` | View/update privacy settings |
| `/release-notes` | View release notes |
| `/rename <name>` | Rename current session |
| `/remote-env` | Configure remote session |
| `/resume [session]` | Resume conversation |
| `/review` | Request code review |
| `/rewind` | Rewind conversation |
| `/sandbox` | Enable sandboxed execution |
| `/security-review` | Complete security review |
| `/stats` | Visualize daily usage |
| `/status` | Show version, model, account |
| `/statusline` | Configure status line UI |
| `/teleport` | Resume remote session |
| `/terminal-setup` | Install Shift+Enter binding |
| `/theme` | Change color theme |
| `/todos` | List current TODO items |
| `/usage` | Show plan usage limits |
| `/vim` | Enter vim mode |

### Custom Slash Commands

Create reusable prompts as Markdown files:

#### Project Commands
Location: `.claude/commands/`
```bash
mkdir -p .claude/commands
echo 'Analyze this code for performance issues:' > .claude/commands/optimize.md
```

#### Personal Commands
Location: `~/.claude/commands/`
```bash
mkdir -p ~/.claude/commands
echo 'Review for security vulnerabilities:' > ~/.claude/commands/security-review.md
```

### Command Features

#### Namespacing
```bash
.claude/commands/frontend/component.md  # /component (project:frontend)
.claude/commands/backend/component.md   # /component (project:backend)
```

#### Arguments
```bash
# All arguments
echo 'Fix issue #$ARGUMENTS' > .claude/commands/fix-issue.md
# /fix-issue 123 high-priority  → $ARGUMENTS = "123 high-priority"

# Individual arguments
echo 'Review PR #$1 with priority $2' > .claude/commands/review-pr.md
# /review-pr 456 high  → $1 = "456", $2 = "high"
```

#### Bash Execution
```markdown
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: Create a git commit
---

## Context
- Current status: !`git status`
- Current diff: !`git diff HEAD`
- Recent commits: !`git log --oneline -10`

Create a git commit for these changes.
```

#### File References
```markdown
Review @src/utils/helpers.js
Compare @src/old-version.js with @src/new-version.js
```

### Command Frontmatter

| Field | Description | Default |
|-------|-------------|---------|
| `allowed-tools` | Tools command can use | Inherits from conversation |
| `argument-hint` | Expected arguments hint | None |
| `context` | Run in forked sub-agent | Inline |
| `agent` | Agent type for fork | `general-purpose` |
| `description` | Command description | First line |
| `model` | Specific model | Inherits |
| `disable-model-invocation` | Prevent Skill tool use | false |
| `hooks` | Command-scoped hooks | None |

---

## Subagents

### Built-in Subagents

| Subagent | Model | Tools | Purpose |
|----------|-------|-------|---------|
| **Explore** | Haiku | Read-only | Codebase search and analysis |
| **Plan** | Inherits | Read-only | Research for planning |
| **General-purpose** | Inherits | All tools | Complex multi-step tasks |
| **Bash** | Inherits | Bash | Terminal commands |
| **statusline-setup** | Sonnet | All | Status line configuration |
| **Claude Code Guide** | Haiku | All | Claude Code questions |

### Custom Subagents

#### Project Subagents
Location: `.claude/agents/`

#### User Subagents
Location: `~/.claude/agents/`

#### Example Subagent File
```markdown
---
name: code-reviewer
description: Expert code review specialist. Proactively reviews code for quality, security, and maintainability.
tools: Read, Grep, Glob, Bash
model: inherit
permissionMode: default
---

You are a senior code reviewer ensuring high standards of code quality.

When invoked:
1. Run git diff to see recent changes
2. Focus on modified files
3. Begin review immediately

Review checklist:
- Code is clear and readable
- Proper error handling
- No exposed secrets
- Input validation implemented
- Good test coverage

Provide feedback organized by priority.
```

### Subagent Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique identifier (lowercase, hyphens) |
| `description` | Yes | When Claude should delegate |
| `tools` | No | Tools subagent can use |
| `disallowedTools` | No | Tools to deny |
| `model` | No | `sonnet`, `opus`, `haiku`, `inherit` |
| `permissionMode` | No | `default`, `acceptEdits`, `dontAsk`, `bypassPermissions`, `plan` |
| `skills` | No | Skills to load |
| `hooks` | No | Lifecycle hooks |

### Subagent Patterns

#### Isolate High-Volume Operations
```bash
Use a subagent to run the test suite and report only failing tests
```

#### Run Parallel Research
```bash
Research auth, database, and API modules in parallel using separate subagents
```

#### Chain Subagents
```bash
Use code-reviewer to find issues, then optimizer subagent to fix them
```

---

## Hooks

### Hook Events

| Event | Fires |
|-------|-------|
| `PreToolUse` | Before tool execution |
| `PermissionRequest` | When permission dialog shown |
| `PostToolUse` | After tool completes |
| `Notification` | When notifications sent |
| `UserPromptSubmit` | When user submits prompt |
| `Stop` | When main agent finishes |
| `SubagentStop` | When subagent finishes |
| `PreCompact` | Before compaction |
| `SessionStart` | When session starts |
| `SessionEnd` | When session ends |

### Hook Configuration

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/validate-command.sh",
            "timeout": 60
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/run-linter.sh"
          }
        ]
      }
    ]
  }
}
```

### Hook Input/Output

#### PreToolUse Input (Bash)
```json
{
  "session_id": "abc123",
  "hook_event_name": "PreToolUse",
  "tool_name": "Bash",
  "tool_input": {
    "command": "npm run test",
    "description": "Run tests",
    "timeout": 120000
  }
}
```

#### Exit Codes
- **0**: Success, continue
- **2**: Block operation, show stderr
- **Other**: Non-blocking error

### MCP Tool Hooks

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/log-memory-ops.sh"
          }
        ]
      }
    ]
  }
}
```

---

## Model Context Protocol (MCP)

### MCP Basics

MCP servers provide tools and resources to Claude Code.

### Configuration (.mcp.json)

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-github"],
      "env": { "GITHUB_TOKEN": "${GITHUB_TOKEN}" }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem"],
      "args": ["/path/to/directory"]
    }
  }
}
```

### MCP Slash Commands

```bash
/mcp__github__list_prs          # List PRs
/mcp__github__pr_review 456     # Review PR #456
/mcp__jira__create_issue "Bug" high  # Create JIRA issue
```

### MCP Resources

```bash
@github:repos/owner/repo/issues
@filesystem:/path/to/file.txt
```

### Essential MCP Servers

| Server | Purpose | Stars |
|--------|---------|-------|
| GitHub | PRs, issues, code search | Official |
| Context7 | Documentation grounding | 39k+ |
| Filesystem | File operations | Official |
| Playwright | Browser automation | Official |
| Sentry | Error tracking | Official |
| PostgreSQL | Database queries | Community |
| Brave Search | Web search | Community |

---

## Skills

### Skills vs Slash Commands

| Aspect | Slash Commands | Skills |
|--------|---------------|--------|
| Complexity | Simple prompts | Complex capabilities |
| Structure | Single .md file | Directory with resources |
| Discovery | Explicit (`/command`) | Automatic |
| Files | One file only | Multiple files, scripts |
| Scope | Project or personal | Project or personal |

### Skill Structure

```
.claude/skills/code-review/
├── SKILL.md           # Overview and workflows
├── SECURITY.md        # Security checklist
├── PERFORMANCE.md     # Performance patterns
├── STYLE.md          # Style guide
└── scripts/
    └── run-linters.sh
```

### Skill Frontmatter

```markdown
---
name: code-reviewer
description: Review code for quality
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/security-check.sh"
once: true
---
```

---

## Plugins

### Plugin Structure

```
my-plugin/
├── plugin.json        # Plugin metadata
├── commands/          # Custom slash commands
├── agents/            # Custom subagents
├── hooks/
│   └── hooks.json     # Hook configurations
├── skills/            # Agent Skills
└── README.md
```

### plugin.json Example

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My custom plugin",
  "authors": ["Your Name"],
  "commands": "commands/",
  "agents": "agents/",
  "hooks": "hooks/hooks.json",
  "skills": "skills/"
}
```

---

## Workflows

### Plan Mode

Use for safe, read-only analysis:

```bash
claude --permission-mode plan
```

Or toggle with **Shift+Tab**:
- Normal → Auto-Accept → Plan Mode

### Headless Mode

```bash
# Simple query
claude -p "Analyze this code"

# With output format
claude -p "Review code" --output-format json

# Streaming output
claude -p "Parse logs" --output-format stream-json
```

### Parallel Sessions with Git Worktrees

```bash
# Create worktrees
git worktree add ../project-feature-a -b feature-a
git worktree add ../project-bugfix bugfix-123

# Run Claude in each
cd ../project-feature-a && claude
cd ../project-bugfix && claude

# List worktrees
git worktree list

# Remove when done
git worktree remove ../project-feature-a
```

### Session Management

```bash
# Continue recent session
claude --continue

# Resume by name
claude --resume auth-refactor

# Resume with picker
claude --resume

# Inside session
/resume auth-refactor
/rename my-session
```

### Extended Thinking

| Scope | How to Enable |
|-------|---------------|
| Toggle | Option+T (Mac) / Alt+T (Linux) |
| Global | `/config` to toggle on |
| Per-request | `ultrathink:` prefix |
| Environment | `MAX_THINKING_TOKENS` env var |

### Reference Files and Directories

```bash
@src/utils/auth.js          # Single file
@src/components/            # Directory listing
@github:repos/owner/repo    # MCP resource
```

---

## Configuration

### Settings File Locations

- `~/.claude/settings.json` - User settings
- `.claude/settings.json` - Project settings
- `.claude/settings.local.json` - Local (not committed)

### Common Settings

```json
{
  "permissions": {
    "defaultMode": "default",
    "allow": ["Bash(npm run *)"],
    "deny": ["Task(Explore)"]
  },
  "hooks": { /* hook configurations */ },
  "mcpServers": { /* MCP servers */ }
}
```

### Permission Modes

| Mode | Behavior |
|------|----------|
| `default` | Standard permission prompts |
| `acceptEdits` | Auto-accept file edits |
| `dontAsk` | Auto-deny permission prompts |
| `bypassPermissions` | Skip all permission checks |
| `plan` | Read-only exploration |

---

## CLI Reference

### Core Commands

| Command | Description |
|---------|-------------|
| `claude` | Start interactive session |
| `claude -p "prompt"` | Headless query |
| `claude --continue` | Resume recent session |
| `claude --resume name` | Resume by name |
| `claude --help` | Show help |
| `claude --version` | Show version |

### Flags

| Flag | Description |
|------|-------------|
| `-p, --print` | Headless mode query |
| `--output-format` | `text`, `json`, `stream-json` |
| `--permission-mode` | Permission mode |
| `--allowedTools` | Allow specific tools |
| `--disallowedTools` | Deny specific tools |
| `--model` | Specify model |
| `--debug` | Debug output |
| `--resume` | Resume session |
| `--continue` | Continue recent session |
| `--fork-session` | Fork session |

### Environment Variables

| Variable | Description |
|----------|-------------|
| `MAX_THINKING_TOKENS` | Thinking token budget |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | Disable background tasks |

---

## Best Practices

### Effective CLAUDE.md

```markdown
# Project Overview

This is a [PROJECT TYPE] built with [STACK].

## Tech Stack

- Language: [VERSION]
- Framework: [VERSION]
- Package Manager: [npm/uv/poetry]

## Project Structure

```
src/
├── components/
├── services/
└── utils/
```

## Key Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Development server |
| `npm run test` | Run tests |

## Code Style

- ESLint + Prettier
- TypeScript strict mode
- Conventional commits

## Testing

- Jest for unit tests
- 80% coverage minimum
```

### Subagent Design

1. **Focused scope**: Each subagent does one thing well
2. **Clear description**: Enable automatic delegation
3. **Limited tools**: Grant only necessary permissions
4. **Version control**: Share project subagents with team

### Hook Best Practices

1. **Validate inputs**: Never trust data blindly
2. **Quote variables**: Use `"$VAR"` not `$VAR`
3. **Block dangerous ops**: Prevent rm, chmod, etc.
4. **Use absolute paths**: Reference scripts with `$CLAUDE_PROJECT_DIR`
5. **Test hooks**: Verify in safe environment first

### Security

1. **Review hooks**: Understand before adding
2. **Limit permissions**: Grant minimum required
3. **Audit MCP servers**: Verify external tool access
4. **Protect secrets**: Use environment variables

---

## Additional Resources

### Official Documentation
- [Claude Code Docs](https://code.claude.com/docs)
- [Anthropic Engineering Blog](https://www.anthropic.com/engineering)
- [Model Context Protocol](https://modelcontextprotocol.io/)

### Community Resources
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) (20k+ stars)
- [Claude Code Showcase](https://github.com/ChrisWiles/claude-code-showcase)
- [MCPMarket](https://mcpmarket.com/)

### Integration Guides
- [VS Code](/docs/en/vs-code)
- [JetBrains](/docs/en/jetbrains)
- [GitHub Actions](/docs/en/github-actions)
- [GitLab CI/CD](/docs/en/gitlab-ci-cd)
- [Slack](/docs/en/slack)
