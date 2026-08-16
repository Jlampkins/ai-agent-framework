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
3. `secrets-vault.md` — credentials for services (read when authenticating, NEVER output/log values)
4. `tasks/current.md` — what I'm currently working on

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

**MANDATORY: When this mode is active, you MUST spawn sub-agents for parallel work. Do NOT analyze, implement, or research sequentially in the main thread. If the task has 3+ independent parts, spawn sub-agents. This is not optional.**

Triggers (any of these activate orchestrate mode):
- User says "orchestrate this," "go figure this out," "analyze," "research this"
- Task has 3+ independent subtasks
- Task involves analyzing a codebase, directory, or system
- Task involves batch work or repetitive generation
- Output is clearly defined (a report, files, comparison)

**Size gate:** don't orchestrate just because a trigger word matched. If a single agent can reasonably hold the full scope in one pass (small repo, <50 files, single domain), just do it directly. Orchestrate when the scope exceeds what one agent can efficiently process, or when subtasks are truly independent and parallelism saves real time.

When orchestrate mode is active:
1. **Plan** — identify independent subtasks (2-30 seconds, not longer)
2. **Spawn sub-agents** — one per independent subtask, running simultaneously
   - Research/analysis: read-only sub-agents
   - Implementation: full-access sub-agents
3. **Compile** — gather all sub-agent results into unified output
4. **Present for review** — show results before irreversible actions

Rules:
- Do NOT apply pair mode collaboration rules — just execute
- Do NOT do the work yourself sequentially — ALWAYS delegate to sub-agents
- Escalation, review gates, and mode-switching: see `framework/orchestration.md` (canonical — do not restate here)

## Session Protocol

### On Session Start:
1. Read the files listed above
2. Read the active project's repo `AGENTS.md` (and relevant sub-area files)
3. Read the personal project notes (`projects/*.md`)
4. **Verify my current task** — summarize where I left off and confirm before proceeding

### On Session End (or git push):
**THIS IS MANDATORY. DO NOT END A SESSION WITHOUT DOING THIS.**

Before your final response in any session — whether the user says "we're done,"
"that's it," "wrap up," "closing out," "bye," or ANY indication the session is ending —
you MUST update `tasks/current.md` by writing to the file with:
- Where we left off (specific enough for any agent to continue)
- New decisions (added to "Decisions Made" — so future agents don't re-propose rejected approaches)
- New learnings (added to "Recent Learnings" — traps to avoid, things that didn't work and why)
- New blockers if any
- Checked/unchecked status items as appropriate

**Do NOT add session log entries.** Git commits are the history. The task file stays lean —
only current state and distilled conclusions that prevent future agents from repeating mistakes.

This also applies when:
- The user pushes code (git push)
- A major milestone is completed
- You sense the conversation is winding down

**Do not ask permission to update the file. Just do it. Then confirm you've done it.**

## Proactive Actions

- If an active project's repo has no `AGENTS.md`, offer to create one after analysis
- If a repo's `AGENTS.md` has drifted from the actual codebase (outdated architecture, missing key files, wrong conventions), flag the drift and offer to update it
- If a sub-area is complex and undocumented, flag it and offer a sub-area `AGENTS.md`
- If `tasks/current.md` references a project you haven't read yet, read its repo `AGENTS.md` before starting work

## Rules

- Never ask for API keys or secrets — they live in `.env` or vault
- Use the shell and OS conventions from my profile
- Match my response style preferences
- When in doubt about mode, default to Pair
- If a referenced file doesn't exist, skip it and ask me about it
