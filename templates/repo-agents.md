# [Project Name]

> This is the repo-level AI context file. Any AI agent opening this repo
> reads this to understand the project. It's committed and shared with the team.
>
> Personal notes (your API access, your current task) live in ~/.ai/projects/
> Sub-area AGENTS.md files handle complex directories (see Key Areas below).
>
> Delete this comment block after populating.

---

## Overview

<!-- One paragraph: what is this project, who is it for, what problem does it solve? -->

## Architecture

- **Framework/engine:**
- **Key patterns:** (ECS, MVC, microservices, event-driven, etc.)
- **Data flow:** (how data moves through the system)
- **State management:** (if relevant)

## Tech Stack

- **Language:** [version]
- **Runtime:** [Node, .NET, Python, etc.]
- **Key dependencies:**
  - [dep] — [what it does]
  - [dep] — [what it does]
- **Build tool:** [Vite, webpack, MSBuild, cargo, etc.]

## Project Structure

```
[root]/
├── src/            # [what's in here]
├── tests/          # [testing approach]
├── docs/           # [documentation]
├── infrastructure/ # [IaC, deployment]
└── [other]/        # [purpose]
```

## Key Areas

<!-- Link to sub-directory AGENTS.md files for complex areas.
     Agent reads these when working in that directory. -->

| Directory | AGENTS.md | Purpose |
|-----------|-----------|---------|
| [src/engine/] | [src/engine/AGENTS.md] | [ECS, game loop] |
| [src/api/] | [src/api/AGENTS.md] | [endpoints, middleware] |

## Conventions (project-specific)

<!-- Only things that differ from or add to the developer's global standards.
     Don't repeat general language conventions — just project-specific ones. -->

- [Pattern or convention specific to this project]
- [Naming rules unique to this codebase]
- [Architectural constraints]

## How to Build & Run

```bash
# Install dependencies
[command]

# Dev server / watch mode
[command]

# Build for production
[command]

# Run tests
[command]

# Lint / format
[command]
```

## Key Files

<!-- Help the agent find the important stuff fast -->

- **Entry point:** [path]
- **Config:** [path]
- **Main types/interfaces:** [path]
- **Routes / endpoints:** [path]
- **Database schema:** [path]

## Environment Variables

<!-- Names and purposes only — never values -->

| Variable | Purpose |
|----------|---------|
| [DATABASE_URL] | [Database connection] |
| [API_KEY_XYZ] | [External service auth] |

## Active Development Notes

<!-- Current architectural direction, recent big decisions, things
     a new developer (or agent) should know before making changes. -->
