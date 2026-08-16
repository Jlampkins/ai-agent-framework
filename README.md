# AI Agent Configuration Framework

## What This Is

A portable system for configuring any AI coding assistant (Claude, Cursor, Copilot, Kiro, Windsurf, ChatGPT, etc.) with persistent context that carries across sessions, machines, and agents.

**The problem it solves:** Every time you start a new session, you re-explain who you are, what you're working on, and how you want things done. Every time you switch agents, you start from zero.

**The solution:** A set of markdown files that any AI agent can read, structured so the agent automatically knows your identity, preferences, project context, and current task — then picks up where your last session left off.

---

## How It Works

```
You open any AI agent → it reads ~/.ai/AGENTS.md → it knows everything → you just work
```

### The AGENTS.md Pattern

One file is the entry point. It routes to everything else:

```
~/.ai/AGENTS.md (global — about YOU)
  → profile.md         "Who I am"
  → standards.md       "How I code"
  → tasks/current.md   "What I'm doing now" → routes to active project(s)
  → orchestration      "How to work with me"

<repo>/AGENTS.md (per-project — about THE WORK)
  → Architecture, conventions, how to build
  → Sub-area AGENTS.md files for complex directories
```

Same pattern at both levels: an entry point that routes to detail files.

---

## Setup (from scratch on any machine)

1. Copy this `framework/` folder to `~/.ai/framework/`
2. Open any AI agent
3. Say: `Read and follow ~/.ai/framework/setup-prompt.md`
4. Answer the agent's questions
5. The agent creates all your instance files (`profile.md`, `standards.md`, etc.)
6. The agent registers itself (adds `~/.ai/AGENTS.md` to its global config)
7. Every future session auto-loads your context

### After setup, register additional agents:

Tell each new agent:
```
Add "Read and follow ~/.ai/AGENTS.md" to your global configuration.
```

The agent creates its own config entry. One time, permanent.

---

## File Structure (what gets created)

```
~/.ai/
├── AGENTS.md              # Entry point — agents read this first
├── profile.md             # Who you are, how you work
├── standards.md           # Coding standards & conventions
├── tasks/
│   └── current.md         # Active task — drives project routing
├── projects/
│   └── <project>.md       # Personal notes per project (private)
│
└── framework/             # THIS FOLDER — the portable factory
    ├── README.md          # You're reading it
    ├── setup-prompt.md    # The setup interview prompt
    ├── orchestration.md   # Work mode rules
    └── templates/         # Blank templates
        ├── AGENTS.md
        ├── profile.md
        ├── standards.md
        ├── task.md
        ├── project.md
        ├── repo-agents.md
        └── repo-agents-sub.md
```

---

## Core Concepts

### Three Layers

| Layer | What | Changes | Where |
|-------|------|---------|-------|
| Identity | Who you are, preferences, tools | Rarely | `~/.ai/profile.md`, `~/.ai/standards.md` |
| Project | Architecture, conventions, APIs | Per project | Repo `AGENTS.md` + `~/.ai/projects/` |
| Task | Current work, status, blockers | Per session | `~/.ai/tasks/current.md` |

### Session Protocol

**Start:** Agent reads context → verifies your current task → confirms before proceeding.

**End:** Agent updates `tasks/current.md` with what was accomplished, where you left off, and any new blockers. The next session (any agent, any machine) picks up exactly there.

### Work Modes

**Pair (default):** Conversational, you're in the loop, thinking together. For exploration, debugging, design decisions, learning interactively.

**Orchestrate:** Agent works independently (or delegates to sub-agents), delivers results for your review. For batch work, scaffolding, research reports, codebase analysis, parallel tasks.

The distinction: "Let's figure this out together" = pair. "Go figure this out and come back to me" = orchestrate.

See `orchestration.md` for full rules.

### Repo Integration

Each codebase gets its own `AGENTS.md` at the root:
- Describes architecture, tech stack, conventions, how to build/test
- Links to sub-directory `AGENTS.md` files for complex areas
- Agent-specific pointer files (`.cursorrules`, `CLAUDE.md`, etc.) are one-liners that say "Read AGENTS.md"

This makes repos self-describing — anyone opening the project with any agent gets context, regardless of whether they have the global setup.

---

## Implementing for a Team / Company

1. Copy this `framework/` folder into a shared location (internal repo, wiki, etc.)
2. Each developer runs the setup on their machine
3. Agree on repo-level `AGENTS.md` standards for your projects
4. Commit `AGENTS.md` + pointer files to shared repos

### What's personal (not shared):
- `~/.ai/profile.md` — individual preferences
- `~/.ai/tasks/current.md` — what each person is doing
- `~/.ai/projects/*.md` — personal notes, API access

### What's shared (committed to repos):
- `<repo>/AGENTS.md` — project context for everyone
- `<repo>/src/<area>/AGENTS.md` — sub-area context
- Pointer files (`.cursorrules`, `CLAUDE.md`, `.windsurfrules`, `.github/copilot-instructions.md`)

---

## Agent Compatibility

| Agent | Global config | Repo config |
|-------|--------------|-------------|
| Kiro CLI | `~/.kiro/steering/bootstrap.md` | Reads `AGENTS.md` directly |
| Cursor | Settings → Rules → User Rules | `.cursorrules` → AGENTS.md |
| GitHub Copilot | VS Code setting | `.github/copilot-instructions.md` → AGENTS.md |
| Windsurf | `~/.windsurfrules` | `.windsurfrules` → AGENTS.md |
| Claude Code | `~/.claude/` config | `CLAUDE.md` → AGENTS.md |
| Claude Desktop | Project custom instructions | Upload/paste AGENTS.md |
| ChatGPT | Custom Instructions | Paste content (no file access) |
| Any API | System message | Include AGENTS.md in request |

---

## Maintenance

| File | Frequency | Who updates |
|------|-----------|-------------|
| `profile.md` | Quarterly or on tool changes | You |
| `standards.md` | When conventions change | You |
| `tasks/current.md` | Every session end | Agent (automatic) |
| `projects/*.md` | When project context changes | You or agent |
| Repo `AGENTS.md` | When architecture changes | Team (committed) |
| This framework | When you refine the system | You |
