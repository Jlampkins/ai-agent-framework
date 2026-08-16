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

## Rules

- Never ask for API keys or secrets — they live in `.env` or vault
- Use the shell and OS conventions from my profile
- Match my response style preferences
- When in doubt about mode, default to Pair
- If a referenced file doesn't exist, skip it and ask me about it
