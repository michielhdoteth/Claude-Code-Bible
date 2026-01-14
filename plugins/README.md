# Plugins

Extensions that enhance Claude Code's capabilities.

## What are Plugins?

Plugins extend Claude Code's functionality through:
- **MCP Servers**: Model Context Protocol servers for external tools
- **IDE Integrations**: VS Code, JetBrains, and other editor plugins
- **CLI Extensions**: Additional command-line capabilities
- **API Connectors**: Integration with external services

## MCP Servers

Model Context Protocol (MCP) servers enable Claude Code to interact with external tools and services.

### Installing MCP Servers

```bash
# Using npm
npx -y ultra-mcp install

# Manual installation
npm install -g @modelcontextprotocol/server-filesystem
```

### Popular MCP Servers

#### Filesystem
```bash
npm install @modelcontextprotocol/server-filesystem
```

#### Git
```bash
npm install @modelcontextprotocol/server-git
```

#### PostgreSQL
```bash
npm install @modelcontextprotocol/server-postgres
```

#### Memory/Knowledge
```bash
npm install @modelcontextprotocol/server-memory
```

### MCP Configuration

Create `.mcp.json`:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/workspace"]
    },
    "git": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-git"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://user:pass@localhost:5432/db"
      }
    }
  }
}
```

## IDE Integrations

### VS Code

Install the Claude Code extension:
```
Name: Claude Code
ID: anthropic.claude-code
Description: AI-powered coding assistant
Marketplace Link: https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code
```

### JetBrains

Available through JetBrains Marketplace:
- IntelliJ IDEA
- WebStorm
- PyCharm
- PhpStorm

### Neovim

```lua
-- lua/claude-code.lua
require('claude-code').setup({
  api_key = os.getenv("ANTHROPIC_API_KEY"),
  model = "sonnet"
})
```

## CLI Extensions

### Claude Code Collective

```bash
# Initialize with defaults
npx claude-code-collective init --minimal

# Interactive setup
npx claude-code-collective init --interactive

# Testing framework only
npx claude-code-collective init --testing-only
```

### VibeKit

```bash
# Install
npm install -g vibekit

# Run Claude Code through VibeKit
vibekit claude

# View agent logs
vibekit logs --agent claude
```

### BWC CLI (Subagent Manager)

```bash
# Install
npm install -g bwc-cli

# Initialize project configuration
bwc init --project

# Add subagent
bwc add --agent python-pro

# List available
bwc list --agents
```

## Third-Party Plugins

### CodeRabbit

AI-driven code review tool integration.

See [Integrations/CodeRabbit](integrations/coderabbit.md)

### Ultra MCP

Collection of useful MCP tools.

```bash
npx -y ultra-mcp install
```

### Ruv-Swarm

Multi-agent swarm capabilities.

```bash
claude-code --mcp-server ruv-swarm
```

## Creating Custom Plugins

### MCP Server Template

```typescript
// my-plugin.ts
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new Server({
  name: 'my-custom-plugin',
  version: '1.0.0'
});

// Define your tools here
server.setRequestHandler(/* ... */);

const transport = new StdioServerTransport();
server.connect(transport);
```

### Plugin Structure

```
my-plugin/
├── package.json
├── src/
│   ├── index.ts
│   ├── tools.ts
│   └── handlers.ts
├── README.md
└── .mcp.json
```

## Best Practices

1. **Security First**: Validate all inputs from plugins
2. **Resource Management**: Clean up resources on shutdown
3. **Error Handling**: Provide meaningful error messages
4. **Documentation**: Document all tools and their parameters
5. **Testing**: Test plugins before deployment

## Troubleshooting

### Plugin Not Loading

1. Check configuration syntax
2. Verify command paths
3. Check environment variables
4. Review logs with `claude /doctor`

### Connection Issues

1. Verify server is running
2. Check port/connection settings
3. Review firewall settings

## Resources

- [MCP Documentation](https://modelcontextprotocol.io)
- [Plugin SDK](https://docs.anthropic.com/en/docs/claude-code/sdk)
- [Community Plugins](https://github.com/topics/claude-code-plugin)
