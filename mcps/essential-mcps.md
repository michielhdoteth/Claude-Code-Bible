# Essential MCP Servers for Claude Code

A curated list of the best Model Context Protocol servers to supercharge your Claude Code workflow.

## What is MCP?

MCP (Model Context Protocol) is an open standard that connects AI assistants to external tools and data sources. It replaces brittle API integrations with a universal standard.

## MCP Server Categories

### Learn: External Knowledge

| Server | Purpose | Install |
|--------|---------|---------|
| **Context7** | Current documentation grounding | `npx @upstash/context7-mcp` |
| **Brave Search** | Web search | `npx brave-search-mcp-server` |
| **Firecrawl** | URL to clean Markdown | `npx firecrawl-mcp-server` |
| **Exa** | Semantic search | `npx exa-mcp-server` |
| **GPT Researcher** | Deep research agent | `npx gptr-mcp` |

### Create: Design & Frontend

| Server | Purpose | Install |
|--------|---------|---------|
| **Figma Dev Mode** | Design-to-code | Via Figma |
| **Magic UI** | React/Tailwind components | `npx magicui-mcp-server` |

### Build: Engineering

| Server | Purpose | Install |
|--------|---------|---------|
| **Filesystem** | Read/edit local files | Official MCP |
| **Desktop Commander** | Terminal + process management | Community |
| **E2B** | Secure code execution | `npx @e2b/mcp-server` |
| **Git** | Repo understanding | Official MCP |
| **GitHub** | Code search, PRs, issues | `npm install github-mcp-server` |
| **GitLab** | CI/CD, MRs | Official |
| **Semgrep** | Security scanning | `npx semgrep-mcp` |

### Data: Backend

| Server | Purpose | Install |
|--------|---------|---------|
| **Prisma Postgres** | Schema + queries | `npx prisma mcp` |
| **Supabase** | PostgreSQL + Auth | `npx supabase-mcp` |
| **Convex** | Full-stack functions | `npx convex-mcp-server` |
| **MongoDB** | Document database | `npx mongodb-mcp-server` |
| **SQLite** | Local database | Official MCP |
| **Stripe** | Payments | `npx stripe-mcp` |
| **AWS** | DynamoDB, Aurora | Official |

### Test: Quality

| Server | Purpose | Install |
|--------|---------|---------|
| **Playwright** | Browser automation | `npx playwright-mcp-server` |
| **Chrome DevTools** | Runtime debugging | `npx chrome-devtools-mcp` |
| **BrowserStack** | Cross-browser testing | `npx browserstack-mcp-server` |

### Run: Deploy & Observe

| Server | Purpose | Install |
|--------|---------|---------|
| **Netlify** | Deploy frontend | `npx netlify-mcp-server` |
| **Vercel** | Deploy Next.js | `npx vercel-mcp-server` |
| **Sentry** | Error tracking | `npx sentry-mcp-server` |
| **Datadog** | Metrics/logs/traces | `npx datadog-mcp-server` |

### Work: Productivity

| Server | Purpose | Install |
|--------|---------|---------|
| **Slack** | Channel reading/posting | `npx slack-mcp-server` |
| **Linear** | Issue tracking | `npx linear-mcp-server` |
| **Jira** | Enterprise tracking | `npx jira-mcp-server` |
| **Notion** | Wiki search | `npx notion-mcp-server` |
| **Google Drive** | Docs/Sheets | `npx google-drive-mcp-server` |
| **Task Master** | AI task management | `npx claude-task-master` |

### Automate: Workflows

| Server | Purpose | Install |
|--------|---------|---------|
| **n8n** | Workflow automation | `npx n8n-mcp-server` |
| **Zapier** | 5000+ app integrations | `npx zapier-mcp-server` |
| **Pipedream** | Serverless workflows | `npx pipedream-mcp-server` |

### Brain: Memory & Reasoning

| Server | Purpose | Install |
|--------|---------|---------|
| **Sequential Thinking** | Structured reasoning | `npx sequential-thinking-mcp` |
| **Knowledge Graph** | Entity relationships | Via Zep/Cognee |
| **Memory** | Persistent context | Various |

## Top 10 Essential MCP Servers

### 1. GitHub MCP
```bash
# Install
npm install -g github-mcp-server

# Configure in claude_desktop_config.json
{
  "mcpServers": {
    "github": {
      "command": "github-mcp-server",
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-token"
      }
    }
  }
}
```
**Use for:** Searching code, reviewing PRs, managing issues, understanding repos

### 2. Context7 MCP
```bash
# Install
npx -y @upstash/context7-mcp
```
**Use for:** Getting current documentation instead of outdated training data

### 3. Git MCP (Official)
```bash
# Part of official MCP servers
# Available at: github.com/modelcontextprotocol/servers
```
**Use for:** Safe repo operations (branches, commits, diffs)

### 4. Supabase MCP
```bash
# Install
npx -y supabase-mcp install
```
**Use for:** Database queries with RLS awareness, auth management

### 5. Filesystem MCP (Official)
```bash
# Install from official repo
# github.com/modelcontextprotocol/servers
```
**Use for:** Safe file reading and editing

### 6. Playwright MCP
```bash
# Install
npx -y playwright-mcp-server
```
**Use for:** Browser automation, E2E testing verification

### 7. Sentry MCP
```bash
# Install
npm install -g sentry-mcp-server
```
**Use for:** Pulling error context, checking performance issues

### 8. Linear MCP
```bash
# Install
npx -y linear-mcp-server
```
**Use for:** Creating issues, tracking projects, sprint management

### 9. Stripe MCP
```bash
# Install
npm install -g stripe-mcp
```
**Use for:** Managing subscriptions, payments, customer data

### 10. E2B MCP
```bash
# Install
npx -y @e2b/mcp-server install
```
**Use for:** Running generated code in secure sandboxes

## MCP Security Best Practices

### Do
- Start with read-only servers
- Use sandboxed environments for code execution
- Scope permissions to minimum needed
- Store API keys in secrets managers
- Log MCP usage for audit

### Don't
- Grant production database write access immediately
- Skip authentication on sensitive servers
- Run untrusted MCP servers
- Expose credentials in MCP configs

## Quick Install Script

```bash
#!/bin/bash
# Essential MCP servers setup

echo "Installing essential MCP servers..."

# GitHub
npm install -g github-mcp-server

# Context7
npx -y @upstash/context7-mcp

# Supabase
npx -y supabase-mcp install

# Filesystem (official)
git clone https://github.com/modelcontextprotocol/servers.git
cd servers/src/filesystem && npm install

# Git (official)
cd ../../src/git && npm install

# Playwright
npx -y playwright-mcp-server install

# Sentry
npm install -g sentry-mcp-server

echo "MCP servers installed!"
echo "Configure in ~/.config/claude/claude_desktop_config.json"
```

## Configuration Example

```json
{
  "mcpServers": {
    "github": {
      "command": "github-mcp-server",
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "filesystem": {
      "command": "uvx",
      "args": ["--from", "mcp-server-filesystem", "mcp-server-filesystem", "/path/to/allowed/dir"]
    },
    "git": {
      "command": "uvx",
      "args": ["--from", "mcp-server-git", "mcp-server-git"]
    },
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "supabase": {
      "command": "npx",
      "args": ["-y", "supabase-mcp", "start"]
    },
    "sentry": {
      "command": "sentry-mcp-server",
      "env": {
        "SENTRY_AUTH_TOKEN": "${SENTRY_TOKEN}"
      }
    }
  }
}
```

## Finding More MCP Servers

- [Awesome MCP Servers](https://mcpservers.org/)
- [Glama.ai MCP Marketplace](https://glama.ai/mcp/servers)
- [Official Repository](https://github.com/modelcontextprotocol/servers)
- [MCP Servers Registry](https://github.com/topics/mcp-server)

## Usage Examples

### Web Research + Code
```bash
# Research topic, then implement
claude "Research React Server Components best practices"
claude "Now build a demo using what you learned"
```

### Database + Git
```bash
# Check schema, then migrate
claude "Show me the users table schema"
claude "Generate migration to add email_verified column"
```

### Deploy + Monitor
```bash
# Deploy, then check for issues
claude "Deploy to production"
claude "Check Sentry for any new errors"
```

## Troubleshooting

### Tools Not Appearing
1. Restart Claude Code after config change
2. Check config JSON is valid
3. Verify server is installed and running
4. Check API keys are set

### Permission Errors
1. Ensure MCP server has required scopes
2. Check token permissions
3. Verify directory access is allowed

### Server Crashes
1. Check server logs
2. Verify Node.js/npm versions
3. Try reinstalling the server
