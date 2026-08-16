# AGENTS.md Template (Global Entry Point)

> This is the file every AI agent reads first. It routes to your personal
> configuration files and defines how agents should work with you.
> 
> Delete this comment block after populating.

---

# Agent Configuration

## Context Files

Read these files in order to understand who I am and how to work with me:

1. `profile.md` — my identity, preferences, tools
2. `standards.md` — coding conventions and workflow
3. `tasks/current.md` — what I'm currently working on

## Active Projects

<!-- The agent reads tasks/current.md to determine which projects are active,
     then reads the corresponding project context files. -->

| Project | Repo Path | Context Files |
|---------|-----------|---------------|
| [name] | [path to repo] | Repo: `<repo>/AGENTS.md` • Personal: `projects/[name].md` |

## Work Modes

<!-- Reference or inline from framework/orchestration.md -->

### Default: Pair Mode
- Work conversationally, step by step
- Ask before making significant decisions
- Present options when there are meaningful tradeoffs

### Orchestrate Mode (when triggered)
- Break work into subtasks, execute independently
- Present compiled results for review
- Escalation: self-resolve 2x → pivot approach → escalate to me
- Review gates: respect user-flagged checkpoints and auto-gate on irreversible/costly actions

See `framework/orchestration.md` for full rules.

## Session Protocol

### On Session Start:
1. Read the files listed above
2. Read the active project's repo `AGENTS.md` (and relevant sub-area files)
3. Read the personal project notes (`projects/*.md`)
4. **Verify my current task** — summarize where I left off and confirm before proceeding

### On Session End (or git push):
Update `tasks/current.md` with:
- What was accomplished
- Where I left off (specific enough for any agent to continue)
- New blockers or decisions
- Session log entry with date

## Rules

- Never ask for API keys or secrets — they live in `.env` or vault
- Use the shell and OS conventions from my profile
- Match my response style preferences
- When in doubt about mode, default to Pair
- If a referenced file doesn't exist, skip it and ask me about it
