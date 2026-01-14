# Claude Code Workflows

Advanced autonomous and multi-agent workflows for Claude Code.

## Contents

### [Ralph Wiggum](ralph-wiggum.md)
The autonomous AI coding loop that builds features while you sleep. Run Claude Code for hours (or overnight) with clear completion criteria.

**Key concepts:**
- Simple bash loop pattern
- Progress file memory
- Safety limits
- When to use / when to avoid

### [OpenProse](openprose.md)
A programming language for long-running AI sessions. Declarative multi-agent orchestration inside the agent session.

**Key concepts:**
- Session as IoC container
- The Fourth Wall (`**...**`)
- Structure + flexibility
- Portable across AI platforms

## Choosing a Workflow

| Workflow | Use Case | Complexity |
|----------|----------|------------|
| Ralph Wiggum | Overnight feature development | Simple |
| OpenProse | Complex multi-agent orchestration | Advanced |
| Standard Claude Code | Interactive development | Basic |

## Quick Start

### Ralph Wiggum (Simple)

```bash
# Install plugin
/plugin install ralph-wiggum@claude-plugins-official

# Launch overnight loop
/ralph-loop "Implement feature from prd.md" --max-iterations 30 --completion-promise "DONE"
```

### OpenProse (Advanced)

```bash
# Install
claude plugin marketplace add https://github.com/openprose/prose.git
claude plugin install open-prose@prose

# Try examples
"run example prose program and teach me how it works"
```

## Resources

- [Ralph Wiggum by Geoffrey Huntley](https://ghuntley.com/ralph/)
- [OpenProse GitHub](https://github.com/openprose/prose)
- [Sid Bharath's Ralph Guide](https://sidbharath.com/blog/ralph-wiggum-claude-code/)
- [Mejba Ahmed's Tutorial](https://www.mejba.me/blog/ralph-wiggum-ai-coding-loop)
