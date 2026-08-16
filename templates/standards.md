# Coding Standards

> Your conventions across all projects. Project-specific overrides
> go in the repo's AGENTS.md, not here.
>
> Delete this comment block after populating.

---

## Languages

### [Primary Language]
- **Style:** (strict mode, linting config, formatter)
- **Naming:** (variables, functions, classes, interfaces)
- **Patterns:** (error handling, async, state management)
- **Avoid:** (anti-patterns specific to this language)

### [Secondary Language]
- **Style:**
- **Naming:**
- **Patterns:**
- **Avoid:**

<!-- Repeat for each language you regularly use -->

## File & Directory Naming

- **Files:** (kebab-case, PascalCase, camelCase, snake_case)
- **Directories:** (convention)
- **Components/modules:** (naming pattern)
- **Tests:** (*.test.ts, *.spec.ts, __tests__/, etc.)

## Testing

- **Framework:** (Jest, Vitest, xUnit, pytest, etc.)
- **Location:** (colocated with source / separate `tests/` directory)
- **Naming:** (describe blocks, test function names)
- **Coverage:** (expectations, what must be tested)
- **Approach:** (unit first? integration? TDD?)

## Git Workflow

- **Branch strategy:** (trunk-based, gitflow, feature branches)
- **Branch naming:** (feature/xxx, fix/xxx, etc.)
- **Commit format:** (conventional commits, freeform, squash policy)
- **PR/MR process:** (required reviews, CI checks, merge strategy)
- **Protected branches:** (main, develop, release/*)

## Dependencies

- **Pinning:** (exact versions, lock files, ranges policy)
- **Package manager:** (npm, pnpm, yarn, pip, cargo, etc.)
- **Adding deps:** (evaluation criteria, approval needed?)
- **Updating deps:** (frequency, automation, breaking change policy)

## Code Quality

- **Linting:** (tool and config file)
- **Formatting:** (tool, run on save, pre-commit hook)
- **Type safety:** (strict mode, no any, no implicit any)
- **Error handling:** (pattern — Result types, try/catch, error boundaries)
- **Logging:** (structured? levels? library?)

## Documentation

- **Code comments:** (when to comment, JSDoc/TSDoc, inline vs block)
- **README expectations:** (per repo, per package, per module)
- **API docs:** (generated, handwritten, OpenAPI)
- **ADRs:** (architecture decision records — yes/no, where)

## Security Defaults

- **Input validation:** (where, how)
- **Auth patterns:** (JWT, session, API keys)
- **Secrets:** (never in code, env vars, vault references)
- **Dependencies:** (audit frequency, known-vuln policy)
