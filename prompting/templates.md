# Claude Code Prompting Templates

Ready-to-use prompt templates for common development tasks with Claude Code.

## Code Generation Templates

### Basic Feature Implementation

```
Implement a [FEATURE_NAME] feature for [PROJECT_CONTEXT].

## Requirements
- [REQUIREMENT 1]
- [REQUIREMENT 2]
- [REQUIREMENT 3]

## Existing Code
[PASTE RELEVANT CODE]

## Constraints
- Use [LANGUAGE/FRAMEWORK] patterns
- Follow [STYLE_GUIDE]
- Include error handling
- Add unit tests

## Output Format
1. Implementation code
2. Test cases
3. Brief explanation
```

### API Endpoint Creation

```
Create a new REST API endpoint for [ENTITY] with the following:

## Endpoint Specification
- Method: [GET/POST/PUT/DELETE]
- Path: /api/v1/[RESOURCE]
- Request body/params: [SCHEMA]
- Response: [SCHEMA]

## Existing Code
[PASTE RELATED CODE]

## Requirements
- Use [FRAMEWORK] conventions
- Add input validation
- Include error responses
- Write integration tests
- Follow existing service patterns

## Related Endpoints
[REFERENCE ENDPOINTS]
```

## Code Review Templates

### Security Review

```
Perform a security review of [FILE/PATH]:

## Focus Areas
- SQL injection vulnerabilities
- XSS vulnerabilities
- Authentication/authorization issues
- Sensitive data exposure
- Dependency vulnerabilities

## Context
[PURPOSE OF CODE]

## Known Patterns
[SECURITY PATTERNS TO CHECK]

## Output Format
1. Critical issues (with fixes)
2. Medium issues (with fixes)
3. Low issues (with fixes)
4. Recommendations
```

## Debugging Templates

### Bug Investigation

```
Debug the following issue:

## Bug Description
[BUG DESCRIPTION]

## Error Message
[ERROR MESSAGE]

## Stack Trace
[STACK TRACE]

## Reproduction Steps
1. [STEP 1]
2. [STEP 2]

## Expected Behavior
[WHAT SHOULD HAPPEN]

## Actual Behavior
[WHAT'S HAPPENING]

## Investigation Steps
1. Analyze the error
2. Identify root cause
3. Propose fix with code
4. Suggest test to prevent regression
```

## Testing Templates

### Test-Driven Development

```
Following TDD, implement [FEATURE]:

## Requirements
[REQUIREMENTS]

## First: Write Failing Tests
Create tests that will pass once [FEATURE] is implemented:

```[LANGUAGE]
[TEST CODE - SHOULD FAIL]
```

## Second: Implement Feature
Write the minimum code to make tests pass:

```[LANGUAGE]
[IMPLEMENTATION]
```

## Third: Refactor
Improve code while keeping tests passing.

## Constraints
- Follow [TESTING FRAMEWORK]
- Use [MOCKING STRATEGY]
- Achieve [COVERAGE %] coverage
```

## Quick Reference

### Prompt Structure

| Element | Purpose | Example |
|---------|---------|---------|
| Role | Define expertise | "You are a senior backend engineer" |
| Context | Background | "Working on e-commerce API" |
| Task | Clear instruction | "Create auth endpoint" |
| Constraints | Boundaries | "Use JWT, 24h expiry" |
| Format | Output structure | "Return JSON" |
| Examples | Show desired output | "Example: {user: {...}}" |

### Effective Prompt Characteristics

1. Specific: Clear, detailed instructions
2. Contextual: Relevant background
3. Structured: Organized with sections
4. Constrained: Defined boundaries
5. Verifiable: Success criteria defined
6. Iterative: Room for refinement
