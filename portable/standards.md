# Coding Standards

## Languages

### TypeScript
- **Style:** Strict mode, no `any`
- **Naming:** camelCase variables/functions, PascalCase classes/interfaces/types
- **Patterns:** Async/await over callbacks, explicit error handling
- **Avoid:** Unnecessary abstractions, over-engineering

### C#
- **Style:** Standard .NET conventions
- **Naming:** PascalCase methods/properties, camelCase locals
- **Patterns:** Standard .NET patterns

### Python
- **Style:** PEP 8
- **Naming:** snake_case functions/variables, PascalCase classes
- **Patterns:** Type hints where useful

## File & Directory Naming

- **Files:** kebab-case (TypeScript), PascalCase (C#), snake_case (Python)
- **Directories:** kebab-case
- **Tests:** Colocated with source — `*.test.ts` / `*.spec.ts`

## Testing

- **Philosophy:** Tests should be useful and kept up to date. No boilerplate for coverage numbers.
- **Write tests when:** Adding features, fixing bugs, complex logic that could regress
- **Don't write tests for:** Trivial wiring, simple getters, things that would just test the framework
- **Keep them honest:** If a test never fails, question whether it's testing anything real

## Git Workflow

- **Branch strategy:** Feature branches off main
- **On push:** Push to release branch
- **Commit format:** Clear, descriptive (not necessarily conventional commits — just make it understandable)
- **PRs:** Feature → release

## Dependencies

- **Philosophy:** Minimize. Avoid if you can build it reasonably yourself.
- **When you do use them:** Keep at latest versions
- **Justification needed:** Don't add a dep for something you could write in 50 lines
- **Lock files:** Yes, commit them

## Code Quality

- **Linting:** Use project-standard linter config
- **Type safety:** Strict — no `any`, no implicit any
- **Error handling:** Explicit, don't swallow errors silently
- **Logging:** Structured when in production code

## Documentation

- **Code comments:** When the WHY isn't obvious. Don't comment the WHAT.
- **READMEs:** Per repo minimum
- **API docs:** When there's a public interface
