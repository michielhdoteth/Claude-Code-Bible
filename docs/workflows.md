# Claude Code Workflows

Practical, battle-tested workflows for using Claude Code effectively.

## The Eight-Step Workflow

A reliable process for any significant change:

### 1. Clarify Your Goal
Know exactly what you want to accomplish before typing anything.

### 2. Write Your Thoughts
Document what you're trying to accomplish in plain English. This forces precision.

### 3. AI Creates Execution Plan
Feed your thoughts to Claude and ask for a structured plan. This surfaces gaps.

### 4. Research and Refine
Explore the codebase, look things up, refine the plan based on findings.

### 5. Review the Plan
One or more passes to verify the plan is correct before writing any code.

### 6. Execute with Oversight
Claude works through the plan. Watch progress, course-correct when needed.

### 7. Document in docs/
Keep records of architectural decisions and progress.

### 8. Final Review
Confirm the plan was executed correctly.

## Multi-Agent Setup

Run 2-3 Claude Code agents in separate terminal tabs:

- **Agent 1**: Working on a planned feature or bugfix
- **Agent 2**: Creating or refining GitHub issues
- **Agent 3**: Waiting for CI or code review

### Tab Organization Strategy

Each agent has isolated context. The cognitive load is lower than it sounds because contexts don't bleed into each other.

## Context Management: /stash and /catchup

Claude Code's automatic compaction degrades agent quality. Use explicit checkpoints:

### The /stash Command

```markdown
# /stash

Document current progress and plan for continuation.

## Output Format
Save to PLAN.md with these sections:

# [Task Title] - Implementation Plan

## Next Up
> **Immediate next task**: [What to do next]
> **Context needed**: [What the agent needs to remember]

## Current State
[What I've learned, code snippets, file paths]

## What I've Tried
[Dead ends, test results, hypotheses ruled out]

## Key Decisions
- [Decision 1] because [reason]
- [Decision 2] because [reason]

## Remaining Steps
- [ ] Step 1
- [ ] Step 2
```

### The /catchup Command

```markdown
# /catchup

Read PLAN.md and continue from where we left off.

## Instructions
1. Read the PLAN.md file
2. Summarize the current state in 2-3 sentences
3. Pick up exactly where we left off
4. Begin working on "Remaining Steps"

## Note
This is a restart after context was cleared. Take time to re-establish context before proceeding.
```

### Context Control Options

When stashing, tell Claude what to preserve:
- "Note what you learned" - keeps debug findings
- "Note next steps" - prunes completed work, keeps the plan
- "Note both" - preserves everything

This beats automatic summarization because you know what matters.

## Document & Clear Pattern

For complex tasks that span multiple sessions:

### Before Clearing
```markdown
Create a comprehensive summary of:
1. What we've accomplished so far
2. What's left to do
3. Key files and their roles
4. Any workarounds or known issues

Save this to SESSION.md
```

### After Restarting
```markdown
Read SESSION.md and continue where we left off.
```

## Creating GitHub Issues From Claude Code

```markdown
# Create GitHub Issue

Create a GitHub issue for: $ARGUMENTS

## Instructions
1. Search existing issues for duplicates:
   `gh issue list --search "keywords"`
2. Audit the codebase to understand the scope
3. Verify the bug exists where I think it does
4. Create with `gh issue create`
5. Use actual file paths and line numbers
6. Add concrete reproduction steps

## Quality Standards
- Use concrete references (model names, component names) not fuzzy language
- Include actual file paths and line numbers
- Add specific reproduction steps
```

Example usage: `/create-issue checkout fails when cart has mixed currency items`

## The PR Feedback Loop

Automate your review process:

```markdown
# /pr-feedback

Process PR feedback until clean.

## Steps
1. Wait for CI to complete: `gh pr checks --watch`
2. Fetch automated review (Greptile/MCP)
3. Address each piece of feedback
4. Push fixes
5. Trigger re-review
6. Repeat until all checks pass
```

### What to Review
- CI/CD pipeline results
- Automated code review tools (Greptile, CodeRabbit)
- Human reviewer feedback
- Test coverage changes

## Specialized Agents

Create focused agents for repeated tasks:

### code-validator (Haiku)
```yaml
---
name: code-validator
description: Run validation tools and report issues
model: haiku
tools: Bash
---

Run validation and report issues only:
1. pnpm lint:fix
2. pnpm typecheck
3. Report any errors found

Do not try to fix anything. Just report.
```

**Why Haiku:** Validation doesn't need intelligence. Faster, cheaper, won't try to "improve" things.

### i18n-translator
```yaml
---
name: i18n-translator
description: Handle translations for 6 locales with ICU format
tools: Read, Edit, Bash, Grep
---

You handle our 6 locales using ICU message format.

Patterns:
- ICU message format: {count, plural, one {...} other {...}}
- jq patterns for bulk operations
- Locale files in /locales/{en,es,fr,de,ja,zh}/

When translating:
1. Identify the key needing translation
2. Apply ICU format if pluralization needed
3. Use jq for bulk locale operations
```

### software-architect-analyzer (Opus)
```yaml
---
name: software-architect-analyzer
description: Plan complex features with thorough analysis
model: opus
---

You are a senior software architect.

For any feature proposal:
1. Analyze the current codebase structure
2. Identify dependencies and integration points
3. Consider at least 3 alternative approaches
4. Evaluate tradeoffs of each approach
5. Recommend a specific approach with reasoning

Think through this ultrathoroughly before proposing anything.
```

## Planning Mode

Use built-in planning mode for complex changes:

1. Press Shift+Tab to enter analysis-only mode
2. Define what to build and how
3. Set "inspection checkpoints" where Claude must show work
4. Exit planning mode to execute

This builds intuition for what minimal context is needed.

## Parallel Execution with Bash Scripts

For large-scale refactors, use the SDK in scripts:

```bash
#!/bin/bash
# Run multiple Claude instances in parallel

for path in /pathA /pathB /pathC; do
  claude -p "Change all refs from foo to bar in $path" &
done

wait
echo "All done"
```

This is more scalable than managing subagents manually.

## Git Worktree Isolation

Run multiple agents on separate PRs without interference:

### Setup
```bash
# Create worktree for feature
git worktree add .conductor/feature-1 feature-branch

# Run Claude in worktree
cd .conductor/feature-1
claude
```

### Validation Hook
```bash
#!/bin/bash
# .claude/scripts/validate-worktree-dir.sh
# Blocks navigation to parent directories when in worktree
```

This prevents agents from accidentally modifying the wrong branch.

## Visual Tab Indicators (Kitty)

Highlight tabs that need attention:

### Enable Kitty Remote Control
```bash
# In kitty.conf
allow_remote_control yes
listen_on unix:/tmp/kitty-{kitty_pid}
```

### Hook Configuration
```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "command",
        "command": "~/.claude/hooks/stop.sh"
      }]
    }],
    "SessionStart": [{
      "hooks": [{
        "type": "command",
        "command": "~/.claude/hooks/start.sh"
      }]
    }]
  }
}
```

### Scripts
```bash
# start.sh - Clear highlight
kitty @ --to "unix:/tmp/kitty-$KITTY_PID" set-tab-color inactive_bg=none

# stop.sh - Yellow highlight
kitty @ --to "unix:/tmp/kitty-$KITTY_PID" set-tab-color inactive_bg=#b58900
```

Yellow tabs need attention. Clear tabs are working fine.

## Copy/Paste Best Practice

On macOS, use `pbcopy` and `pbpaste`:

```bash
# Instead of printing output
claude -p "explain this code" | pbcopy

# Paste from clipboard
pbpaste | claude -p "improve this code"
```

This avoids unwanted linebreaks from terminal copy.

## What Works Best

### Languages with Strong Tooling
- **Rust**: Type system + Clippy + compiler catch everything
- **TypeScript**: Prettier + ESLint + tests = feedback loop

The tooling acts as guardrails. AI writes code; tools verify it.

### Feedback Loop Pattern
1. AI writes code
2. Format (Prettier)
3. Lint (ESLint)
4. Build (TypeScript compiler)
5. Test (Jest/Vitest)
6. Repeat until clean

Each pass constrains the AI's freedom to introduce errors.

## Common Anti-Patterns

### Don't Over-Document
If you have extensive docs elsewhere, don't @-mention them in CLAUDE.md. This bloats context. Instead, mention the path and when to read it:

```markdown
For complex FooBar usage or if you encounter a FooBarError,
see path/to/docs.md for advanced troubleshooting.
```

### Don't Use Negative-Only Constraints
```markdown
# Bad
Never use --foo-bar flag

# Good
Use --foo-baz instead of --foo-bar for X reason
```

### Don't Block at Write Time
Let the agent finish its plan, then validate at commit time. Blocking mid-plan confuses the agent.

### Don't Create Long Slash Command Lists
If you need many complex commands, you've failed to build an intuitive CLAUDE.md.

## Quick Reference

| Workflow | Command | When |
|----------|---------|------|
| Quick restart | `/clear` + `/catchup` | Simple reboots |
| Complex session | "Document & Clear" | Multi-day tasks |
| Create issue | `/create-issue [title]` | Filing bugs/features |
| PR review | `/pr-feedback` | Automated review loop |
| Checkpoint | `/stash` | Pause and preserve |
| Resume | `/catchup` | Continue from checkpoint |

## Cost Optimization

- Use **Sonnet** for routine tasks (cheaper)
- Use **Opus** for complex planning and architecture
- Clear context regularly to avoid expensive compaction
- Batch similar tasks together

## The Mental Model

It's not "AI writes my code." It's:

- Claude Code is a slow, thorough tool that you orchestrate
- Multiple agents let you parallelize waiting (CI, reviews)
- Explicit context management beats automatic summarization
- Specialized agents beat general prompts for repeated tasks
- Planning catches mistakes before they become code
- The feedback loop (lint → test → review) catches real bugs

The setup takes investment. But the payoff is a workflow where you're rarely blocked waiting for one thing to finish.
