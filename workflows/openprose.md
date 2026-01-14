# OpenProse: A Language for AI Sessions

*A programming language for long-running AI sessions. Named because "prose" is what AI generates.*

## What Is OpenProse?

OpenProse is a language specification that runs inside AI agent sessions. The session itself becomes the runtime - it's both interpreter and execution environment.

Think of it as a new layer between you and Claude Code:
- Traditional: You → Claude Code → Files
- OpenProse: You → OpenProse Program → Claude Code → Files

The key insight: **A long-running AI session is a Turing-complete computer.** OpenProse is a programming language for it.

## Key Concepts

### The Session as IoC Container

Traditional frameworks (LangChain, CrewAI) orchestrate agents from **outside**. OpenProse runs **inside** the agent session - the session IS the container.

```
Other frameworks:  Coordinator → Agent 1, Agent 2
OpenProse:         Session (understands + executes)
```

### The Fourth Wall (`**...**`)

When you need AI judgment instead of strict execution:

```
loop until **the code is production ready**:
  session "Review and improve"
```

The `**...**` syntax lets you speak directly to the AI. It evaluates semantically - deciding what "production ready" means based on context.

### Structure + Flexibility

- **Structure**: Control flow, agent definitions, variables
- **Flexibility**: Natural language for conditions, context passing

## Installation

### Claude Code

```bash
claude plugin marketplace add https://github.com/openprose/prose.git
claude plugin install open-prose@prose
```

### OpenCode

```bash
git clone https://github.com/openprose/prose.git ~/.config/opencode/skill/open-prose
```

### Amp

```bash
git clone https://github.com/openprose/prose.git ~/.config/agents/skills/open-prose
```

Then try: `"run example prose program and teach me how it works"`

## Language Features

### Agents

```prose
agent researcher:
  model: sonnet
  skills: ["web-search"]

agent writer:
  model: opus
```

### Sessions

```prose
session "prompt"              # Simple session
session: researcher           # Use defined agent
session: agent_name
  prompt: "Research X"
  context: [research, notes]
```

### Parallel Execution

```prose
parallel:
  research = session: researcher
    prompt: "Research quantum computing"
  competitive = session: researcher
    prompt: "Analyze competitors"
```

### Variables

```prose
let draft = session "Write first draft"
let revised = session "Refine the draft"
  context: { draft }
```

### Control Flow

```prose
# Fixed loops
repeat 3:
  session "Improve this"

# Unbounded loops
loop until **the draft meets publication standards**:
  session "Refine until done"
  max: 5  # Safety limit

# Conditionals
if **this needs more work**:
  session "Continue editing"
```

### Error Handling

```prose
try:
  session "Risky operation"
catch:
  session "Handle failure"
finally:
  session "Cleanup"
```

### Pipelines

```prose
items | map: session "Process item"
```

### Choice

```prose
choice **best approach**:
  session "Option A"
  session "Option B"
  session "Option C"
```

## Complete Example

```prose
# Research and write workflow
agent researcher:
  model: sonnet
  skills: ["web-search"]

agent writer:
  model: opus

parallel:
  research = session: researcher
    prompt: "Research quantum computing breakthroughs"
  competitive = session: researcher
    prompt: "Analyze competitor landscape"

loop until **the draft meets publication standards** (max: 3):
  session: writer
    prompt: "Write and refine the article"
    context: { research, competitive }
```

## How It Works

### The OpenProse VM

LLMs are simulators. When given a detailed system description, they simulate it - they don't just describe it, they become it.

The OpenProse specification describes a virtual machine with enough fidelity that a Prose Complete system reading it becomes that VM.

```
Traditional:  Code → Compiler → Execution
OpenProse:    Prose → AI Session → Emergent Execution
```

### VM Semantics

| Aspect | Behavior |
|--------|----------|
| Execution order | Strict - follows program exactly |
| Session creation | Strict - creates what specified |
| Parallel coordination | Strict - executes as specified |
| Context passing | Intelligent - summarizes/transforms |
| Condition evaluation | Intelligent - interprets `**...**` |
| Completion detection | Intelligent - determines when "done" |

## Documentation Files

| File | Purpose | When to Read |
|------|---------|--------------|
| `prose.md` | VM semantics | Always, for running programs |
| `docs.md` | Language spec | Compilation/validation questions |

## Examples

The plugin ships with 28 examples:

| Range | Category |
|-------|----------|
| 01-08 | Basics (hello world, research, code review) |
| 09-12 | Agents and skills |
| 13-15 | Variables and composition |
| 16-19 | Parallel execution |
| 20 | Fixed loops |
| 21 | Pipeline operations |
| 22-23 | Error handling |
| 24-27 | Advanced (choice, conditionals) |
| 28 | Orchestration (Gas Town multi-agent system) |

## Why Not Just Plain English?

You CAN use `**...**` for natural language. But complex workflows need unambiguous structure for control flow - the AI shouldn't guess whether you want sequential or parallel execution.

## Why Not Rigid Frameworks?

They're inflexible. OpenProse gives structure where it matters (control flow) and flexibility where you want it (conditions, context).

## Portability

OpenProse runs on any **Prose Complete** system - model + harness capable of inducing the VM:

- Claude Code + Opus
- OpenCode + Opus
- Amp + Opus

It's a language specification, not a library. Your `.prose` files work everywhere.

## Use Cases

### Multi-Agent Orchestration
```prose
agent planner:
  model: opus
agent coder:
  model: sonnet
agent tester:
  model: haiku

sequence:
  planner = session: planner "Plan feature X"
  coder = session: coder
    context: { planner }
    prompt: "Implement based on plan"
  tester = session: tester
    context: { coder }
    prompt: "Test the implementation"
```

### Research Pipeline
```prose
agent researcher:
  model: sonnet

let topics = ["AI", "blockchain", "quantum"]
parallel:
  topics | map: session: researcher
    prompt: "Research this topic"
```

### Iterative Refinement
```prose
agent editor:
  model: opus

let draft = session "Write first draft"
loop until **publishable quality**:
  let revised = session: editor
    prompt: "Improve this draft"
    context: { draft }
  draft = revised
  max: 10
```

## Resources

- [GitHub](https://github.com/openprose/prose)
- [Language Spec](/openprose/prose/blob/main/skills/open-prose/docs.md)
- [Examples](https://github.com/openprose/prose/tree/main/examples)
