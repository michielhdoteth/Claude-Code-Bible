# CLAUDE.md Templates and Examples

This document provides ready-to-use CLAUDE.md templates for various project types.

## Basic Template

```markdown
# Project Overview

This is a [PROJECT TYPE] built with [STACK]. The project follows [SPECIFIC PATTERNS/GUIDELINES].

## Tech Stack

- **Language**: [LANGUAGE] (v[VERSION])
- **Framework**: [FRAMEWORK] (v[VERSION])
- **Package Manager**: [npm/yarn/pnpm/uv]
- **Key Dependencies**:
  - [DEPS]
  - [DEPS]

## Project Structure

```
[SOURCE_DIR]/
├── [SUBDIR]/          # [DESCRIPTION]
├── [SUBDIR]/          # [DESCRIPTION]
├── [SUBDIR]/          # [DESCRIPTION]
└── [CONFIG_FILES]
```

## Key Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run test` | Run tests |
| `npm run lint` | Run linter |
| `npm run typecheck` | Run type checker |

## Code Style

- [STYLE GUIDE RULES]
- [LINTING RULES]
- [FORMATTING RULES]

## Testing

- Framework: [JEST/VITEST/PYTEST]
- Coverage threshold: [X]%
- Location: `tests/` or `[TEST_DIR]`

## Git Workflow

- Branch naming: `[type]/[ticket]-[short-description]`
- Commit format: `[type]: [description]`
- PR requirements: [REQUIREMENTS]
```

## TypeScript/Node.js Template

```markdown
# TypeScript Node.js Project

## Tech Stack

- TypeScript 5.x
- Node.js 20+
- ESLint + Prettier
- Jest or Vitest

## Code Style

- ESLint config: `eslint.config.js`
- Prettier config: `.prettierrc`
- Import order: built-ins → external → relative
- Use absolute imports (`@/` for `src/`)

## TypeScript Rules

- `strict: true` in `tsconfig.json`
- No `any` - use `unknown` instead
- Prefer interfaces over type aliases for objects
- Use explicit return types for exported functions

## Testing

- Jest for unit tests
- Location: `__tests__/` or `tests/`
- Pattern: `*.test.ts` or `*.spec.ts`
```

## Python Project Template

```markdown
# Python Project

## Tech Stack

- Python 3.11+
- [FRAMEWORK: FastAPI/Django/Flask]
- Poetry or uv for package management
- Pytest for testing

## Code Style

- Black for formatting
- isort for import sorting
- ruff for linting
- Type hints required

## Project Structure

```
src/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── api/
│   ├── models/
│   ├── schemas/
│   └── services/
tests/
├── __init__.py
├── conftest.py
└── [TEST_FILES]
```

## Key Commands

| Command | Description |
|---------|-------------|
| `uv run dev` | Development server |
| `uv run test` | Run tests |
| `uv run lint` | Run linters |
| `uv run typecheck` | Type checking |

## Database

- ORM: [SQLAlchemy/Django ORM]
- Migrations: [Alembic/Django migrations]
```

## React/Next.js Template

```markdown
# React/Next.js Project

## Tech Stack

- React 18+ with TypeScript
- Next.js 14+ (App Router)
- Tailwind CSS
- Jest + React Testing Library

## Code Style

- ESLint + Prettier
- Functional components only
- Use hooks for state/logic
- Component file structure:
  - `ComponentName.tsx` - main component
  - `ComponentName.test.tsx` - tests
  - `ComponentName.types.ts` - types
  - `ComponentName.styles.ts` - styles (if needed)

## Directory Structure

```
src/
├── app/              # Next.js App Router
├── components/
│   ├── ui/           # Base UI components
│   ├── features/     # Feature-specific components
│   └── layout/       # Layout components
├── lib/              # Utilities
├── hooks/            # Custom hooks
├── types/            # TypeScript types
└── styles/           # Global styles
```

## Testing

- Jest + React Testing Library
- Vitest for unit tests
- Playwright for E2E tests
- Location: `__tests__/` or alongside components
```

## Full-Stack Web App Template

```markdown
# Full-Stack Web Application

## Architecture

- **Frontend**: [React/Vue/Svelte] with TypeScript
- **Backend**: [Node.js/Python/Go] API
- **Database**: [PostgreSQL/MongoDB/MySQL]
- **API Style**: REST or GraphQL

## Project Structure

```
client/              # Frontend
├── src/
│   ├── components/
│   ├── pages/
│   ├── hooks/
│   └── utils/
server/              # Backend
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   └── utils/
shared/              # Shared types/schemas
```

## Development

- Frontend: `cd client && npm run dev`
- Backend: `cd server && npm run dev`
- Both: `docker-compose up`

## API Conventions

- Base URL: `/api/v1`
- Authentication: Bearer token
- Error format: `{ "error": "message", "code": "ERROR_CODE" }`
```

## DevOps/Infrastructure Template

```markdown
# DevOps/Infrastructure Project

## Tech Stack

- [Terraform/Pulumi] for IaC
- [Kubernetes] for orchestration
- [Docker] for containerization
- [GitHub Actions/GitLab CI] for CI/CD

## Structure

```
├── terraform/        # Terraform modules
│   ├── modules/
│   ├── environments/
│   └── main.tf
├── kubernetes/       # K8s manifests
│   ├── base/
│   ├── overlays/
│   └── helm/
├── scripts/          # Automation scripts
└── .github/
    └── workflows/    # CI/CD pipelines
```

## Key Commands

| Command | Description |
|---------|-------------|
| `terraform plan` | Preview changes |
| `terraform apply` | Apply changes |
| `kubectl apply` | Deploy to K8s |
| `make deploy` | Full deployment |

## Environments

- `dev`: Development environment
- `staging`: Staging environment
- `prod`: Production environment
```

## Quick Reference

### Essential Sections to Include

| Section | Description |
|---------|-------------|
| `# Project Overview` | What this project does |
| `## Tech Stack` | Technologies used |
| `## Project Structure` | Directory layout |
| `## Key Commands` | Common CLI commands |
| `## Code Style` | Linting, formatting rules |
| `## Testing` | Test framework and location |

### Tips for Effective CLAUDE.md

1. **Keep it concise** - Claude reads this every session
2. **Focus on specifics** - Don't repeat common knowledge
3. **Include commands** - Document frequently used scripts
4. **Add anti-patterns** - What NOT to do
5. **Update regularly** - Keep it in sync with the codebase

### Common Mistakes to Avoid

- Too verbose - Context is limited
- Too generic - Generic advice isn't helpful
- Outdated - Don't let it drift from reality
- Missing commands - Common tasks should be documented
- No examples - Code examples help Claude understand
```
