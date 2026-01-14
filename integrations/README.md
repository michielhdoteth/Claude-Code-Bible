# Claude Code Integrations

This directory serves as a comprehensive guide and documentation hub for integrating various tools, platforms, and services with your Claude Code workflow.

## The Importance of Seamless Integrations

AI tools are not isolated entities but integral parts of a larger development ecosystem. Seamless integrations ensure that:

- **Context Flows Freely:** Information can be shared between your IDE, version control, AI assistant, and other tools.
- **Workflows are Automated:** Repetitive tasks can be automated across different tools.
- **Productivity is Maximized:** Developers stay within their preferred environment.
- **Collaboration is Enhanced:** Integrated tools facilitate smoother collaboration.
- **Quality is Maintained:** Automated checks at various pipeline stages.

## MCP Server Integrations

### .mcp.json Configuration

MCP (Model Context Protocol) servers enable Claude Code to connect with external tools. Create `.mcp.json` in your project root:

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

## Available Integrations

### Version Control

| Integration | Description | Setup |
|-------------|-------------|-------|
| **GitHub** | PR creation, code review, issue tracking | `.mcp.json` with GitHub token |
| **GitLab** | Merge requests, CI/CD pipelines | MCP server configuration |
| **Bitbucket** | Pull request workflows | MCP server configuration |

### IDE Integrations

| Integration | Description | Setup |
|-------------|-------------|-------|
| **VS Code** | Claude Code extension available | VS Code Marketplace |
| **Cursor** | AI-first editor with Claude integration | cursor.sh |
| **Neovim** | claude-code.nvim plugin | Plugin manager |
| **Emacs** | claude-code.el package | MELPA |

### Cloud Platforms

| Integration | Description | Setup |
|-------------|-------------|-------|
| **AWS** | EC2, S3, Lambda management | AWS CLI + MCP |
| **Vercel** | Frontend deployment | Vercel CLI |
| **GitHub Codespaces** | Cloud development | GitHub integration |

### Error Monitoring

| Integration | Description | Setup |
|-------------|-------------|-------|
| **Sentry** | Error tracking | MCP server |
| **Datadog** | APM and monitoring | MCP server |

### Databases

| Integration | Description | Setup |
|-------------|-------------|-------|
| **PostgreSQL** | Database queries | MCP server |
| **Supabase** | Backend as a service | MCP server |
| **Prisma** | ORM with MCP | Prisma MCP |

### Communication

| Integration | Description | Setup |
|-------------|-------------|-------|
| **Slack** | Team notifications | MCP server |
| **Discord** | Bot integration | MCP server |

## Contribution Guidelines

1. Fork the repository
2. Create a new Markdown file in `integrations/`
3. Document your integration with:
   - Tool overview
   - Why integrate with Claude Code
   - Integration steps
   - Key features and benefits
   - Usage examples
   - Best practices
4. Add to the list below
5. Submit a pull request

## Available Integrations

### Code Review

*   [CodeRabbit](coderabbit.md): AI-driven code review tool that integrates with GitHub/GitLab/Bitbucket

### IDE Integrations

*   **VS Code**: Use Claude Code extension or `code` CLI
*   **Cursor**: AI-first editor built on VS Code
*   **JetBrains AI Assistant**: Native IDE intelligence for IntelliJ/PyCharm/WebStorm

### Cloud Development

*   **Replit**: Cloud-native IDE with Ghostwriter AI
*   **GitHub Codespaces**: Cloud-hosted development environments

### Error Monitoring

*   **Sentry**: AI-powered error monitoring and performance insights

### Testing

*   **Vitest**: Fast unit test framework
*   **Playwright**: End-to-end browser testing
*   **Jest**: JavaScript testing framework

### Documentation

*   **Mintlify**: AI-powered documentation
*   **Docusaurus**: Static site generator with AI support

### Deployment

*   **Vercel**: Zero-config deployment for frontend
*   **Railway**: Simple deployment with AI insights
*   **Render**: Managed hosting with AI assistance

### Version Control

*   **GitHub**: Native integration with PR review
*   **GitLab**: Merge request automation
*   **Bitbucket**: Pull request workflows
