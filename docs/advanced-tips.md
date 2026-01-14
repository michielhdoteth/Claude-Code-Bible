# Claude Code Advanced Tips

Advanced patterns, anti-patterns, and insights from power users.

## CLAUDE.md Philosophy

### Treat It as a Constitution, Not a Manual

**Shrivu's approach:**
- For hobby projects: Let Claude dump whatever it wants
- For professional work: Strictly maintained, currently ~13KB
- Document tools used by 30%+ of engineers
- Use max token limits per tool (like "ad space" for teams)

### Writing Principles

**Start with guardrails, not a manual:**
```
# Bad
Everything about this project...

# Good (starts small, grows based on what Claude gets wrong)
# Only document what Claude keeps getting wrong
```

**Don't @-file docs:**
```markdown
# Bloats context on every run
See @docs/complex-setup.md

# Better
For complex setup, see docs/complex-setup.md
Explain why and when to read it
```

**Don't just say "never":**
```markdown
# Bad
Never use --force flag

# Good
Don't use --force. Use --interactive instead, which prompts
before dangerous operations.
```

**Use it as a forcing function:**
```
If your CLI is complex and verbose, don't write paragraphs
to explain it. Write a simple bash wrapper with clear API.

Keep CLAUDE.md as short as possible = forcing function
for simplifying your codebase.
```

## Context Management Deep Dive

### The /context Command

Run `/context` mid-session to understand token usage:
```
Fresh session in monorepo: ~20k tokens (10%)
Remaining for changes: 180k tokens

This fills up fast. Monitor it.
```

### Three Workflows for Context

1. **Avoid `/compact`**
   - Automatic compaction is opaque
   - Error-prone, not well-optimized
   - Lost nuance mid-debug

2. **Simple Restart: `/clear` + `/catchup`**
   ```
   /clear
   /catchup  # reads changed files in git branch
   ```

3. **Complex Restart: "Document & Clear"**
   ```
   Claude: "Dump plan and progress to .md"
   /clear
   Claude: "Read .md and continue"
   ```

### Why Avoid Auto-Compaction

- Summarization loses nuance
- Debug mental models get destroyed
- Context window degrades quality
- You control what matters, not the algorithm

## Custom Subagents: Master-Clone Pattern

### The Problem with Custom Subagents

Shrivu's analysis:

1. **Gatekeep context** - Main agent can't reason holistically
   ```
   If you make a PythonTests subagent, you've hidden all
   testing context from the main agent.
   ```

2. **Force human workflows** - Dictate how Claude must delegate
   ```
   This is the problem you're trying to get the agent to solve.
   ```

### The Better Approach: Master-Clone

```markdown
Put all context in CLAUDE.md
Let the main agent use Task/Explore to spawn clones of itself
The agent manages its own orchestration dynamically
```

**Benefits:**
- All context-saving benefits of subagents
- No drawbacks of hidden context
- Dynamic, intelligent delegation

### When Custom Subagents Make Sense

- Truly specialized domains (security auditor)
- Complex external integrations (AWS, database)
- When the main agent consistently makes the same mistake

## Hooks: Block-at-Submit Pattern

### Two Types of Hooks

1. **Block-at-Submit (Recommended)**
   ```json
   {
     "matcher": "Bash(git commit)",
     "hooks": [{
       "type": "command",
       "command": "check-for-agent-pre-commit-pass"
     }]
   }
   ```
   - Runs before commit
   - Blocks if tests didn't pass
   - Forces test-and-fix loop

2. **Hint Hooks (Non-blocking)**
   ```json
   {
     "matcher": "Edit",
     "hooks": [{
       "type": "command",
       "command": "hint-about-optimal-pattern"
     }]
   }
   ```
   - Provides feedback after the fact
   - Doesn't block or confuse the agent

### Don't: Block-at-Write

```json
{
  "matcher": "Edit",
  "hooks": [{ "type": "block" }]  // Don't do this
}
```

Blocking mid-plan confuses or "frustrates" the agent. Let it finish, then check at commit time.

## Skills vs MCP

### Shrivu's Take: Skills Are Bigger Than MCP

Three stages of agent autonomy:

1. **Single Prompt** - All context in one massive prompt (brittle)
2. **Tool Calling** - Hand-crafted tools abstract reality (better)
3. **Scripting** - Agent has raw environment access, writes code on fly (best)

### Skills Formalize the Scripting Layer

```
If you've been favoring CLIs over MCP:
You've been getting Skills benefits all along.

SKILL.md is just a more organized, shareable way
to document CLIs and scripts.
```

### When to Use Each

**Skills:**
- Cross-platform workflows (web, desktop, CLI)
- Shareable with others
- Combining prompts + scripts

**MCP:**
- Complex, stateful environments
- Secure data gateways
- Authentication/networking boundaries

**CLIs:**
- Simple, stateless tools
- Internal team workflows
- When you control the environment

### MCP Anti-Pattern to Avoid

```markdown
# Bad MCP
read_thing_a(), read_thing_b(), update_thing_c()...
# Just mirrors REST API, creates abstraction overhead

# Good MCP
download_raw_data(filters...)
execute_code_in_environment(code...)
# Provides entry point, gets out of the way
```

## Claude Code SDK for Batch Processing

### Three Main Uses

1. **Massive Parallel Scripting**
   ```bash
   # For large-scale refactors
   for path in /pathA /pathB /pathC; do
     claude -p "change X to Y in $path" &
   done
   wait
   ```

2. **Building Internal Chat Tools**
   ```
   Complex installer that falls back to Claude Code SDK
   to just fix problems for users.
   
   v0-at-home for design teams to vibe-code mock frontends.
   ```

3. **Rapid Agent Prototyping**
   ```
   Before building full infrastructure,
   prototype with Claude Code SDK.
   
   Test the agent logic, then decide on deployment.
   ```

## Claude Code GitHub Action

### Why It's Powerful

- Runs Claude Code in a GHA container
- Full control over environment
- Stronger sandboxing than other products
- Supports all advanced features (hooks, MCP)
- Full audit logs = self-improving system

### Use Cases

**PR-from-Anywhere:**
- Trigger from Slack, Jira, CloudWatch alerts
- Agent fixes bug or adds feature
- Returns fully tested PR

**Meta-Analysis:**
```bash
query-claude-gha-logs --since 5d | claude -p \
  "see what the other claudes were getting stuck on and fix it"
```

This creates a data-driven flywheel:
```
Bugs → Improved CLAUDE.md / CLIs → Better Agent
```

## settings.json Configuration

### Essential Settings

```json
{
  "HTTPS_PROXY": "http://proxy:8080",
  "HTTP_PROXY": "http://proxy:8080",
  "MCP_TOOL_TIMEOUT": 120000,
  "BASH_MAX_TIMEOUT_MS": 300000,
  "permissions": {
    "allow": ["Bash", "Read", "Edit"],
    "auto-allow": ["Bash(lint:test)", "Bash(format)"]
  }
}
```

### Why Configure These

- **HTTPS_PROXY**: Debug traffic, inspect prompts
- **MCP_TOOL_TIMEOUT**: Run longer commands
- **Enterprise API keys**: Usage-based pricing vs per-seat

### Enterprise API Key Benefits

- Massive variance in usage (1:100x between engineers)
- Engineers can tinker with non-Claude Code LLM scripts
- Single enterprise account for all usage

## Resume and Continue

### Basic Usage

```bash
claude --resume       # Restart previous session
claude --continue     # Continue interrupted session
```

### Advanced: Historical Analysis

Claude Code stores all session history in:
```
~/.claude/projects/
```

Run meta-analysis on logs:
- Find common exceptions
- Identify permission requests
- Spot error patterns
- Improve agent-facing context

```bash
# Example analysis
cat ~/.claude/projects/*/session.json | jq '.errors[]' | \
  claude -p "find common error patterns"
```

## Planning Mode Deep Dive

### When to Use

Always for "large" feature changes with AI IDE.

### The Pattern

1. Enter planning mode (Shift+Tab)
2. Define what to build AND how
3. Set "inspection checkpoints"
4. Claude must stop and show work at checkpoints
5. Exit planning to execute

### Builds Intuition

Regular use builds intuition for:
- What minimal context is needed
- How to structure good plans
- When Claude is likely to botch implementation

### Custom Planning Tools

For enterprise, build custom planning on SDK:
- Aligns with existing technical design format
- Enforces internal best practices
- Engineers "vibe plan" like senior architects

## Economic Considerations

### Cost Reality

- $500-$1000/month for heavy API usage
- Significantly less than human engineer
- AI doesn't need onboarding, vacations, or have bad days

### ROI Calculation

```
Senior engineer + AI effectively = optimal configuration
Human judgment for architecture + review
AI speed for implementation

Outperforms either alone.
```

### Cost Optimization

| Model | Use For | Cost |
|-------|---------|------|
| Haiku | Validation, simple tasks | Low |
| Sonnet | Routine coding | Medium |
| Opus | Complex planning | High |

- Use Sonnet until 50% usage, then switch (Claude default)
- Clear context to avoid compaction costs
- Batch similar tasks

## The Anti-Patterns to Avoid

### 1. Over-Documenting
```
Don't @-mention large files in CLAUDE.md
Don't try to be comprehensive
Start small, grow based on failures
```

### 2. Negative-Only Constraints
```
Don't say "never use X"
Provide alternatives: "use Y instead of X because..."
```

### 3. Block-at-Write Hooks
```
Let agent finish plan
Then validate at commit time
Blocking mid-plan confuses the agent
```

### 4. Long Slash Command Lists
```
If you need many complex commands
You failed to build intuitive CLAUDE.md
Slash commands = simple shortcuts only
```

### 5. Trusting Auto-Compaction
```
Manual context management is better
Use /clear for restarts
Document & Clear for complex tasks
```

## Quick Reference Table

| Feature | Best Practice | Avoid |
|---------|--------------|-------|
| CLAUDE.md | Short, guardrails | Comprehensive manual |
| /compact | Avoid | Rely on it |
| Subagents | Use Task/Explore | Custom specialized |
| Hooks | Block-at-submit | Block-at-write |
| MCP | Data gateways | API mirrors |
| Skills | Cross-platform | When CLI suffices |
| SDK | Batch processing | Over-engineering |
| Context | Manual management | Auto-compaction |
| Planning | Complex features | Small changes |

## The Mental Model Shift

### Before
```
I write code → I review code → I test code
```

### After
```
I think → I plan → AI writes code
         → I review → I test
         → I oversee multiple agents
         → I manage complexity
```

### The Job Transformed, Not Disappeared

What no longer happens:
- Opening 15 files to find what to edit
- Typing implementations character by character
- Manually chasing references for renames
- Writing boilerplate from scratch

What still happens:
- Learning new technologies
- Architecting systems
- Writing requirements
- Overseeing AI execution
- Reviewing every line
- Documenting decisions
- Managing complexity

The mechanical labor is gone. The thinking remains.

## Resources

- [Shrivu's Blog](https://blog.sshh.io/p/how-i-use-every-claude-code-feature)
- [Jökull's Blog](https://www.solberg.is/how-i-use-claude-code)
- [Ryan Charles](https://ryanxcharles.com/blog/2025-12-30-how-i-use-claude-code-to-do-my-entire-job)
- [Official Docs](https://docs.anthropic.com/en/docs/claude-code)
