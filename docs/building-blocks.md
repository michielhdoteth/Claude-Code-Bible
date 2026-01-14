# Claude Code Building Blocks

A comprehensive guide to Claude Code's core features: Markdown Files, Slash Commands, Agents, Skills, Hooks, and Plugins.

## The Six Building Blocks

Claude Code provides six building blocks for creating powerful AI workflows:

| Building Block | Purpose | Invoked By | Cross-Platform |
|---------------|---------|------------|----------------|
| Markdown Files | Context & Memory | Claude loads as needed | Yes (copy files) |
| Slash Commands | Stored Prompts/Procedures | You type /command | Via plugins |
| Agents | Parallel Tasks & Context | Claude or you | Claude Code only |
| Skills | Instructions + Code | Claude (auto) | Yes (web, desktop, CLI) |
| Hooks | Guaranteed Automations | Event triggers | Claude Code only |
| Plugins | Share Workflows | N/A | Via marketplaces |

## 1. Markdown Files: Context and Memory Management

### The Three-Layered Context System

Claude Code supports a hierarchical context system:

```
Layer 1: Global Preferences
~/.claude/CLAUDE.md

Layer 2: Project Context  
project/CLAUDE.md

Layer 3: Reference Files
Any folder, referenced from CLAUDE.md
```

### CLAUDE.md Structure

```markdown
# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview
- This is a React TypeScript project
- Uses Next.js 14, Tailwind CSS

## Common Commands
- npm run dev - Start development server
- npm test - Run tests
- npm run lint - Lint code

## Code Standards
- Use TypeScript for all new files
- Follow React hooks best practices
- Use Tailwind for styling

## Important Notes
- API keys are in .env.local (never commit this file)
- Deploy to Vercel only
```

### Using # for Quick Memory

Add memories on the fly using the # symbol:

```
# Always use MUI components for new UI
# My team prefers Jest over Vitest
# API routes go in /app/api/
```

Claude automatically saves these to the appropriate memory file.

### Hierarchical CLAUDE.md

Create CLAUDE.md files in nested directories for specific context:

```
project/
├── CLAUDE.md              # Project-wide rules
├── src/
│   ├── CLAUDE.md          # Source-specific rules
│   ├── components/
│   │   └── CLAUDE.md      # Component-specific rules
│   └── utils/
```

Claude prioritizes the most specific (most nested) file.

## 2. Slash Commands: Stored Prompts and Procedures

### Creating Slash Commands

Create `.claude/commands/<name>.md`:

```markdown
# /competitive-research

Research competitors and generate comparison tables.

## Usage
`/competitive-research company1 company2 company3`

## Steps
1. Launch sub-agents to research each company in parallel
2. Gather information on: pricing, features, strengths, weaknesses
3. Generate comparison table in Markdown
4. Provide recommendations
```

### Simple vs Complex Commands

**Simple (Brainstorming):**
```markdown
# /headlines

Generate 5 headlines for the provided content.

Consider:
- SEO optimization
- Click-worthy language
- Clarity of value proposition
```

**Complex (Multi-step with APIs):**
```markdown
# /seo

Perform SEO analysis on the provided article.

1. Analyze content structure
2. Identify potential keywords
3. Generate semantic variants
4. Fetch keyword search volumes via API
5. Generate optimization recommendations
```

### Using Arguments

Reference user input with `$ARGUMENTS`:

```markdown
# /test

Create comprehensive tests for: $ARGUMENTS

Requirements:
- Use Jest and React Testing Library
- Place in __tests__ directory
- Mock external dependencies
- Include edge cases
```

### Subfolders for Organization

Create subfolders for categorized commands:

```
.claude/commands/
├── code-review.md
├── debugging.md
└── builder/
    └── plugin.md    # Use as /builder/plugin
```

## 3. Agents: Context Management and Parallel Execution

### Types of Agents in Claude Code

1. **Main Agent**: Claude itself with tool access
2. **General-Purpose Sub-Agents**: Task tool, Explore tool
3. **User-Defined Sub-Agents**: Custom specialized agents

### When to Use Sub-Agents

**Context Window Management:**
```
Main task: Write blog post about AI trends
  └─ Sub-agent: Research AI trends (uses its own context)
  └─ Sub-agent: Research competitors (uses its own context)
  └─ Main agent: Synthesize and write
```

**Parallel Execution:**
```
Main task: Analyze 10 competitor companies
  ├─ Agent 1: Research Ford
  ├─ Agent 2: Research Chevrolet
  ├─ Agent 3: Research Toyota
  └─ ... (all run in parallel)
```

### Creating a Sub-Agent

Create `.claude/agents/<name>.md`:

```yaml
---
name: technical-reviewer
description: Review technical content for accuracy and clarity
tools: Read, Edit, Grep, Bash
model: sonnet
---

You are a technical reviewer. When reviewing technical content:

1. Verify code examples compile and work
2. Check for security vulnerabilities
3. Assess technical accuracy
4. Suggest improvements with examples
5. Check for outdated information

Provide:
- Issues found (severity: high/medium/low)
- Recommendations
- Approval status
```

### Using Sub-Agents

**Explicit invocation:**
```
Use the technical-reviewer agent to check my blog post
```

**In slash commands:**
```markdown
# /blog-post

Write a blog post about: $ARGUMENTS

Use the research-agent to gather information first.
Then use the technical-reviewer to verify accuracy.
```

## 4. Skills: Portable Instruction Packages

### What Are Skills?

Skills combine prompts, procedures, context files, and scripts into portable packages that work across Claude web, desktop, and Claude Code.

### Skills vs Other Building Blocks

| Feature | Slash Commands | Skills |
|---------|---------------|--------|
| Code bundling | No | Yes |
| Cross-platform | No | Yes |
| Auto-invocation | No | Yes (in theory) |
| Shareability | Via plugins | Built-in |

### Creating a Skill

Skills live in directories with `SKILL.md`:

```
.claude/skills/
├── flashcard-generator/
│   └── SKILL.md
├── markdown-converter/
│   ├── SKILL.md
│   └── converter.py
└── promptify/
    └── SKILL.md
```

```yaml
---
name: flashcard-generator
description: Convert text into Anki flashcard decks
---

# Flashcard Generator

You are a flashcard generation expert. Convert provided text into Anki flashcards.

## Format
Each flashcard should have:
- Front: Question or term
- Back: Answer or definition

## Guidelines
- One concept per card
- Use simple, clear language
- Include examples where helpful
- Target 20-30 cards per 1000 words

## Output Format
```txt
Q: [question]
A: [answer]
---
Q: [question]
A: [answer]
```
```

### Skill Examples

**Flashcard Generator:**
```yaml
---
name: flashcard-generator
description: Convert educational content into Anki flashcards
---

Convert the provided content into Anki flashcard format.
Focus on key concepts, definitions, and important facts.
```

**Markdown Converter:**
```yaml
---
name: markdown-converter
description: Convert PDFs, Word, PowerPoint to Markdown
---

Use markitdown to convert documents to Markdown.
Handle:
- PDF files
- Word documents
- PowerPoint presentations
- Excel spreadsheets
- Images with OCR
- Audio files with transcription
```

**Promptify:**
```yaml
---
name: promptify
description: Transform casual dictation into structured prompts
---

Transform casual/verbal requests into well-structured prompts.
Add:
- Clear task definition
- Context and background
- Specific constraints
- Expected output format
- Examples if helpful
```

## 5. Hooks: Guaranteed Automations

### Hook Types

| Hook | When It Runs |
|------|-------------|
| PreToolUse | Before tool calls (can block) |
| PostToolUse | After tool calls complete |
| PermissionRequest | When permission dialog appears |
| UserPromptSubmit | When user submits prompt |
| Notification | When Claude sends notifications |
| Stop | When Claude finishes responding |
| SubagentStop | When subagent tasks complete |
| PreCompact | Before context compaction |
| SessionStart | When session starts/resumes |
| SessionEnd | When session ends |

### Creating Hooks via JSON

```json
// .claude/settings.json
{
  "hooks": [
    {
      "matcher": "Edit|Write",
      "hooks": [
        {
          "type": "command",
          "command": "prettier --write \"$CLAUDE_FILE_PATHS\""
        }
      ]
    },
    {
      "matcher": "Edit",
      "hooks": [
        {
          "type": "command",
          "command": "if [[ \"$CLAUDE_FILE_PATHS\" =~ \\.(ts|tsx)$ ]]; then npx tsc --noEmit --skipLibCheck \"$CLAUDE_FILE_PATHS\" || echo 'TypeScript errors detected'; fi"
        }
      ]
    }
  ]
}
```

### Interactive Hook Setup

Run `/hooks` in Claude Code for an interactive menu interface.

### Practical Hook Examples

**Auto-format on Save:**
```json
{
  "matcher": "Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "prettier --write \"$CLAUDE_FILE_PATHS\""
    }
  ]
}
```

**TypeScript Validation:**
```json
{
  "matcher": "Edit",
  "hooks": [
    {
      "type": "command", 
      "command": "npx tsc --noEmit \"$CLAUDE_FILE_PATHS\""
    }
  ]
}
```

**Date Context Hook:**
```python
# date-context.py
from datetime import datetime
import json

data = json.load(sys.stdin)
print(json.dumps({
    "today": datetime.now().strftime("%Y-%m-%d"),
    "tomorrow": (datetime.now() + timedelta(days=1)).strftime("%Y-%m-%d"),
    "week_start": (datetime.now() - timedelta(days=datetime.now().weekday())).strftime("%Y-%m-%d"),
    "week_end": (datetime.now() + timedelta(days=6-datetime.now().weekday())).strftime("%Y-%m-%d")
}))
```

## 6. Plugins: Sharing Workflows

### Plugin Structure

Plugins are GitHub repositories containing:

```
plugin-name/
├── README.md
├── commands/
│   └── *.md
├── agents/
│   └── *.md
├── skills/
│   └── */
├── hooks/
│   └── settings.json
└── CLAUDE.md
```

### Adding Marketplaces

```bash
/plugin marketplace add anthropics/claude-plugins-official
```

### Installing Plugins

```bash
/plugin install <plugin-name>
```

### Creating Shareable Plugins

1. Create a public GitHub repository
2. Structure it with commands, agents, skills, hooks
3. Share the repo URL
4. Others can install via `/plugin`

### Security Considerations

**Never install plugins you haven't reviewed.** Plugins can execute code on your machine. Always:
- Read the source code
- Check what commands/scripts are included
- Verify the author/source is trusted

## Workflow Design Patterns

### Pattern 1: Slash Command with Sub-Agents

```
/competitive-research Tesla Ford GM
    ├─ Agent 1: Research Tesla (parallel)
    ├─ Agent 2: Research Ford (parallel)
    ├─ Agent 3: Research GM (parallel)
    └─ Synthesize results + generate table
```

### Pattern 2: Hook-Enhanced Workflow

```
User: "Fix the bug"
    ├─ PreToolUse: Run formatter
    ├─ Claude: Analyze and fix
    ├─ PostToolUse: Run type check
    └─ Stop: Send notification
```

### Pattern 3: Skill-Informed Session

```
User: "Write documentation"
    ├─ SessionStart: Load documentation skill
    ├─ Claude: Uses skill guidelines
    └─ Output: Skill-compliant documentation
```

### Pattern 4: Plugin Workflow

```
/plugin install team-workflows
    ├─ Commands: /standup, /retrospective
    ├─ Agents: standup-summarizer
    └─ Hooks: daily-reminder
```

## Best Practices

1. **Start Simple**: Begin with CLAUDE.md and slash commands
2. **Organize Incrementally**: Add structure as needs grow
3. **Share via Plugins**: Package workflows for team reuse
4. **Review Plugins**: Always audit code before installing
5. **Use Hooks for Reliability**: When consistency is critical
6. **Leverage Sub-Agents**: For parallel work and context management
7. **Cross-Platform with Skills**: When sharing across platforms

## Resources

- [Official Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code)
- [Agent Skills Standard](https://agentskills.io/)
- [Anthropic Plugins Repository](https://github.com/anthropics/claude-plugins-official)
