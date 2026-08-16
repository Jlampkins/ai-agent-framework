# Work Modes & Orchestration

## The Core Principle

The mode is determined by **your role in the work**, not the task complexity:

- **Pair** = You participate in the thinking. You're in the loop.
- **Orchestrate** = You receive the output of the thinking. You review results.

Both modes handle simple and complex work. The question is: "Do I want to be in the conversation, or do I want a deliverable?"

---

## Pair Mode (default)

Single agent, conversational, real-time collaboration. You and the agent think together step by step.

### When to use:
- You want to *participate* in the exploration
- You need to react to findings in real time
- Decisions require your taste, judgment, or domain knowledge at each step
- You're learning and want to ask questions as you go
- The path forward is unclear and requires frequent course corrections
- Debugging where each clue changes what you look at next

### Agent behavior in Pair Mode:
- Work step by step, showing reasoning
- Ask before making significant decisions
- Present options when there are meaningful tradeoffs
- Explain what you're doing and why
- Wait for confirmation on irreversible actions

---

## Orchestrate Mode

Planner breaks work into subtasks. Workers execute independently (potentially in parallel). Results are compiled and presented for your review.

### When to use:
- Batch work: "do this N times with these variations"
- Scaffolding: "create these files following this pattern"
- Research/exploration when you want a *report*, not a *conversation*
- Design when you want *options presented*, not *co-creation*
- Codebase analysis: "explain how these subsystems work"
- Learning when you want a *summary/cheat sheet*, not a *tutorial*
- Any task where you'd tell a human "go figure this out and come back to me"

### Orchestrated Intellectual Work

Orchestration is not just for mechanical tasks. Complex thinking can be orchestrated when you want results delivered rather than co-created:

| Task | Pair (in the loop) | Orchestrate (get results) |
|------|-------------------|--------------------------|
| Understand a codebase | Walk through it together, ask questions | Workers analyze subsystems, deliver architecture report |
| Design a system | Back-and-forth on tradeoffs | Workers research N approaches, present comparison |
| Learn a new API | Agent teaches interactively | Workers summarize docs, build a cheat sheet |
| Evaluate options | Try each together, discuss | Workers benchmark independently, present table |
| Debug complex issue | Investigate together, react to clues | Workers check N hypotheses in parallel, report findings |
| Refactor a module | Discuss approach, apply incrementally | Workers refactor, present diff for review |

### Agent behavior in Orchestrate Mode:
- State the plan before executing (what will be done, how many subtasks)
- Execute without asking for input at every step
- Compile results into a clear, reviewable format
- Report what was done, what succeeded, what failed
- Present for review — don't assume approval

---

## Mode Selection

### Signals

| What you say / What's happening | → Mode |
|--------------------------------|--------|
| "I'm not sure how to..." and want to learn | Pair |
| "I'm not sure how to..." and want a recommendation | Orchestrate |
| "Why is this broken?" and want to watch the investigation | Pair |
| "Why is this broken?" and want an answer | Orchestrate |
| "Do this N times" | Orchestrate |
| "Let's try this and see" | Pair |
| "Figure out how X works and explain it to me" | Orchestrate |
| "Show me how X works" | Pair |
| "Implement these specs" | Orchestrate |
| "Let's build this feature" | Pair |
| Needs your taste/judgment at every step | Pair |
| Output is clearly defined or reviewable | Orchestrate |
| You'd walk away and check back later | Orchestrate |
| You're leaning in, watching, reacting | Pair |

### Explicit Triggers

The user can explicitly set the mode:
- "Orchestrate this" / "Go figure this out" / "Come back with results" → Orchestrate
- "Let's work through this" / "Walk me through it" / "Let's pair on this" → Pair
- "Checkpoint after N" → Orchestrate with review gate

### Default

If unclear, default to **Pair**. It's safer — you can always say "just go do it" to switch to Orchestrate mid-task.

---

## Escalation Rules (Orchestrate Mode)

When a worker encounters an error or unexpected result:

1. **First failure:** Attempt self-resolution. Try a reasonable fix or alternative approach.
2. **Second failure (same approach):** Step back. Diagnose the root cause. Try a fundamentally different approach.
3. **Third failure (different approach also fails):** Escalate to user with:
   - What was attempted (both approaches)
   - What failed and why
   - What information might be needed to proceed
   - Suggested next steps (if any)

### Rules:
- Never silently skip a failed subtask
- Never loop more than 3 times without escalating
- Never assume a failure is acceptable without user confirmation
- When escalating, provide enough context that the user can make a decision without re-investigating

---

## Review Gates

### Default: No Gate
Workers complete all subtasks and present final results. Use this when the work is low-risk and clearly defined.

### User-Flagged Gate
User specifies a checkpoint: "Do 2, show me, then continue if good."

The agent pauses after N items and presents results. User either approves continuation or provides corrections before the rest proceeds.

### Auto-Gate (always pause)
Automatically gate when the task involves:
- **Irreversible actions** — deleting files, dropping data, deploying, pushing to remote
- **Resource spend** — API calls with credit/token cost, generation quotas
- **Architectural decisions** — choices that affect other subsystems or are hard to undo
- **External side effects** — sending emails, posting to services, modifying shared state

### Gate Format
When pausing at a gate, present:
- What was completed so far
- Results/output of completed work
- What remains
- Explicit: "Continue with the remaining N items?"

---

## Resource Awareness

### Before orchestrating:
- State what will be consumed: "This will trigger ~N [API calls / generations / files / requests]"
- Do NOT estimate dollar cost (unreliable across services and plans)
- If the task approaches a known limit (rate limits, free tier caps, generation quotas), warn before starting

### After orchestrating:
- Report what was actually used: "Used N of [resource], M remaining"
- If usage was higher than estimated, note why

### Known limits to watch for:
- API rate limits (per-minute, per-hour)
- Generation quotas (monthly, per-plan)
- File system operations at scale (disk space, open file handles)
- Token/context limits of the agent itself

---

## Switching Modes Mid-Task

It's normal to switch modes during a session:

- **Pair → Orchestrate:** "Okay, I understand the approach now. Go implement the rest and come back."
- **Orchestrate → Pair:** "Wait, that result doesn't look right. Let's dig into this together."

The agent should handle transitions gracefully:
- When switching to Orchestrate: summarize what's been decided so far, state the plan
- When switching to Pair: pause, present current state, wait for direction
