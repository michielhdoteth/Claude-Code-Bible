# Hooks

Automated scripts that run at specific points in the development workflow.

## What are Hooks?

Hooks are deterministic automations that are guaranteed to run at specific trigger points. Unlike AI-generated responses which may vary, hooks execute the same code every time.

## Hook Types

| Hook | When It Runs |
|------|-------------|
| **PreToolUse** | Before any tool call (can block execution) |
| **PermissionRequest** | When permission dialog appears (can allow/deny) |
| **PostToolUse** | After any tool call completes |
| **UserPromptSubmit** | When user submits a prompt |
| **Notification** | When Claude sends notifications |
| **Stop** | When Claude finishes responding |
| **SubagentStop** | When subagent tasks complete |
| **PreCompact** | Before context compaction runs |
| **SessionStart** | When session starts or resumes |
| **SessionEnd** | When session ends |

## Getting Started

### Interactive Setup

Run the `/hooks` command in Claude Code for an interactive menu interface.

### Configuration File

Create `.claude/settings.json`:

```json
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
    }
  ]
}
```

### Matcher Patterns

```json
// Exact match
"matcher": "Edit"

// Multiple tools
"matcher": "Edit|Write"

// Regex pattern
"matcher": "Notebook.*"
```

## Environment Variables

Hooks receive these variables:

- `$CLAUDE_FILE_PATHS`: Files being edited
- `$CLAUDE_TOOL_NAME`: Current tool name
- `$CLAUDE_SESSION_ID`: Session identifier

## Example Hooks

### Auto-format on Edit

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

### TypeScript Validation

```json
{
  "matcher": "Edit",
  "hooks": [
    {
      "type": "command",
      "command": "if [[ \"$CLAUDE_FILE_PATHS\" =~ \\.(ts|tsx)$ ]]; then npx tsc --noEmit --skipLibCheck \"$CLAUDE_FILE_PATHS\" || echo 'TypeScript errors detected'; fi"
    }
  ]
}
```

### Date Context Generator

```python
#!/usr/bin/env python3
"""
Generate date context for Claude Code.
Outputs: today, tomorrow, week start/end dates
"""
import json
import sys
from datetime import datetime, timedelta

today = datetime.now()
tomorrow = today + timedelta(days=1)
week_start = today - timedelta(days=today.weekday())
week_end = week_start + timedelta(days=6)

print(json.dumps({
    "today": today.strftime("%Y-%m-%d"),
    "tomorrow": tomorrow.strftime("%Y-%m-%d"),
    "week_start": week_start.strftime("%Y-%m-%d"),
    "week_end": week_end.strftime("%Y-%m-%d")
}))
```

### Markdown Formatter Hook

```python
#!/usr/bin/env python3
"""
Markdown formatter for Claude Code output.
Fixes missing language tags and spacing issues.
"""
import json
import sys
import re

def detect_language(code):
    """Best-effort language detection from code content."""
    s = code.strip()
    # Python detection
    if re.search(r'^\s*def\s+\w+\s*\(', s, re.M) or \
       re.search(r'^\s*(import|from)\s+\w+', s, re.M):
        return 'python'
    # JavaScript detection
    if re.search(r'\b(function\s+\w+\s*\(|const\s+\w+\s*=)', s) or \
       re.search(r'=>|console\.(log|error)', s):
        return 'javascript'
    # Bash detection
    if re.search(r'^#!.*\b(bash|sh)\b', s, re.M) or \
       re.search(r'\b(if|then|fi|for|in|do|done)\b', s):
        return 'bash'
    # SQL detection
    if re.search(r'\b(SELECT|INSERT|UPDATE|DELETE|CREATE)\s+', s, re.I):
        return 'sql'
    return 'text'

def format_markdown(content):
    """Format markdown content with language detection."""
    def add_lang_to_fence(match):
        indent, info, body, closing = match.groups()
        if not info.strip():
            lang = detect_language(body)
            return f"{indent}```{lang}\n{body}{closing}\n"
        return match.group(0)

    fence_pattern = r'(?ms)^([ \t]{0,3})```([^\n]*)\n(.*?)\n(\1 ```)\s*$'
    content = re.sub(fence_pattern, add_lang_to_fence, content)
    content = re.sub(r'\n {3,}', '\n\n', content)
    return content.rstrip() + '\n'

try:
    input_data = json.load(sys.stdin)
    file_path = input_data.get('tool_input', {}).get('filePath', '')
    if file_path.endswith(('.md', '.mdx')) and os.path.exists(file_path):
        with open(file_path, 'r', encoding='utf-8') as f:
            content = f.read()
        formatted = format_markdown(content)
        if formatted != content:
            with open(file_path, 'w', encoding='utf-8') as f:
                f.write(formatted)
            print(f"Fixed markdown formatting in {file_path}")
except Exception as e:
    print(f"Error formatting markdown: {e}", file=sys.stderr)
    sys.exit(1)
```

### Security Scan Hook

```bash
#!/bin/bash
# .claude/hooks/security-scan.sh

echo "Running security scan..."

# Check for hardcoded secrets
if grep -rE "(api_key|apikey|secret|password)" --include="*.py" --include="*.js" . 2>/dev/null | grep -v ".env.example" | grep -v "test" | grep -v "# mock" > /dev/null; then
    echo "ERROR: Potential secrets found in code"
    exit 1
fi

echo "Security scan passed"
```

## Hook Directory Structure

```
.claude/
├── hooks/
│   ├── pre-format.sh
│   ├── post-validate.py
│   └── date-context.py
└── settings.json
```

## Best Practices

1. **Keep hooks fast** - Long-running hooks slow down workflow
2. **Fail gracefully** - Allow override for important commits
3. **Use proper shebangs** - Cross-platform compatibility
4. **Handle errors** - Provide meaningful error messages
5. **Document hooks** - Explain what each hook does
6. **Use SessionStart** for project initialization context

## Resources

- [Official Hooks Guide](https://docs.anthropic.com/en/docs/claude-code/hooks-guide)
- [Git Hooks Documentation](https://git-scm.com/docs/githooks)
