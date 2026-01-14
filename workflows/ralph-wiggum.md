# Ralph Wiggum: Autonomous AI Coding Loops

The "dumbest smart way" to run coding agents overnight. Named after the persistent Simpsons character.

## The Core Concept

```
while true; do
  claude -p "$(cat PROMPT.md)"
done
```

Ralph Wiggum runs the same prompt repeatedly. Each iteration:
1. Reads the requirements (same prompt)
2. Sees the code changes from previous runs (filesystem)
3. Picks up where it left off
4. Commits progress
5. Runs again

**Key insight**: Context window resets, but the code doesn't. Modern models understand existing codebases and continue work seamlessly.

## Why It Works

- Modern models (Opus 4.5+) are finally good enough that brute-force simplicity works
- Filesystem IS the state - no complex handoffs
- Git IS the memory - full history preserved
- No coordination overhead or agent handoff failure points

## The Official Plugin

```bash
# Install in Claude Code
/plugin install ralph-wiggum@claude-plugins-official

# Run a loop
/ralph-loop "Your task" --max-iterations 20 --completion-promise "DONE"
```

## Setup Components

### 1. PRD (Product Requirement Document)

```markdown
# Feature: User Authentication

## User Stories

### Story 1: User Registration
**As a** new user
**I want to** register with email and password

**Acceptance Criteria:**
- POST /api/auth/register accepts { email, password }
- Password hashed with bcrypt (min 10 rounds)
- Returns 201 with user object (no password)
- Returns 409 if email exists

**Verification:** npm test -- --grep "registration"

**Completion Signal:** <promise>REGISTRATION_COMPLETE</promise>
```

### 2. Progress File Pattern

Claude logs each iteration:

```markdown
## Iteration 1
- Set up project structure
- Created database schema
- TODO: Auth endpoints not started yet

## Iteration 2
- Implemented POST /api/auth/register
- Added input validation
- Tests passing
```

New iterations read this to understand current state.

### 3. Memory System

**Short-term**: `progress.txt` - session history
**Long-term**: `folder/agents.md` - institutional knowledge

```markdown
# auth/agents.md

# Authentication Notes

- Use bcrypt, not sha256 for passwords
- JWT secret from env var AUTH_SECRET
- Token expires in 24 hours
- Common mistake: forgetting to validate email format
```

## Essential Patterns

### Define Done Precisely

```markdown
When ALL true, output COMPLETE:
- All functions have unit tests
- Tests are passing
- No TypeScript errors
- README documents the API
```

### Keep Iterations Small

Each iteration should do ONE thing. Quality degrades as context window fills.

### Feedback Loops Are Non-Negotiable

```markdown
Before marking complete:
1. Run the test suite
2. Run type checking
3. Manually verify the feature works

If any check fails, fix before proceeding.
```

### Safety Limit

```bash
# Never run without a limit!
for i in $(seq 1 50); do
  claude -p "$(cat PROMPT.md)"
  # Check for completion, break if found
done
```

## Complete Setup Example

```bash
# 1. Write PRD (prd.md)
# 2. Initialize memory files
echo "# Progress Log" > progress.txt
echo "# Auth notes\n- bcrypt for passwords" > src/auth/agents.md

# 3. Launch loop
/ralph-loop "Read prd.md and implement all stories. For each story:
1. Implement code
2. Run verification test
3. Mark complete only if tests pass
4. Update progress.txt

If stuck after 3 attempts on one story, document blockers and move to next.
Output <promise>AUTH_COMPLETE</promise> when all pass." \
  --completion-promise "AUTH_COMPLETE" \
  --max-iterations 30
```

## Results

| Project | Stories | Iterations | Cost |
|---------|---------|------------|------|
| REST API | 12 | 47 | $4.23 |
| Component Library | 8 | 31 | $2.87 |
| Data Migration | 1 | 19 | $1.54 |

## When to Use Ralph

### Good Candidates
- Refactoring (migrate X to Y, done when tests pass)
- Test coverage (done when coverage hits threshold)
- Documentation (done when all APIs documented)
- Well-specified features with acceptance criteria
- Batch transformations

### Poor Candidates
- Exploratory work with unclear requirements
- Design decisions needing human judgment
- Security-sensitive code requiring careful review
- Anything where "done" is subjective

## Common Pitfalls

### 1. Vague Acceptance Criteria
```
# Bad
"Make it work well"

# Good (testable)
"POST /api/todos returns 201 with created todo"
```

### 2. Stories Too Large
Break stories until completable in 2-3 iterations.

### 3. No Escape Hatch
Always use `--max-iterations`.

### 4. No Test Infrastructure
Ralph can only verify what it can test.

### 5. Forgetting agents.md
Ralph forgets between sessions. Document gotchas.

## Advanced Techniques

### Browser Integration for Frontend

```bash
/ralph-loop "Implement login form. After, use browser skill to 
navigate to /login, fill form, submit, verify redirect." --max-iterations 20
```

### Parallel Ralphs

Run multiple loops on different files simultaneously for independent features.

### Progressive PRDs

Complete Phase 1, review, then write Phase 2 based on results.

### Night Shift Pattern

- Morning: Write tomorrow's PRD
- Afternoon: Manual coding on today's priorities
- Evening: Launch Ralph on tomorrow's feature
- Morning: Review Ralph's work

## The Philosophy

Ralph represents a shift from:
- **Directive prompting**: "Do X, then Y, then Z"
- **Conditions for emergence**: "Here's what success looks like"

You're less architect, more gardener. Plant seeds, provide conditions, let it grow.

### Mental Model

```
Traditional: I write code → I review → I test
Ralph:       I define success → Ralph iterates → I review final output
```

## Economics

- Typical loop: $2-5 for medium feature
- Compare to: Your hourly rate ($50-200+)
- One developer completed $50K contract for $297 in API costs

## Resources

- [Official Plugin](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum)
- [Sid Bharath's Guide](https://sidbharath.com/blog/ralph-wiggum-claude-code/)
- [Mejba Ahmed's Tutorial](https://www.mejba.me/blog/ralph-wiggum-ai-coding-loop)
- [Geoffrey Huntley Original](https://ghuntley.com/ralph/)
