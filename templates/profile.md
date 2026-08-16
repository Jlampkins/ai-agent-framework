# Developer Profile

> Your identity, preferences, and how you like to work.
> This is read by every agent at session start.
>
> PORTABLE SECTIONS: Who I Am, How I Work, Collaboration Style
> → These are YOU. They travel across machines, companies, projects.
> → Copy them as-is to any new environment.
>
> ENVIRONMENT-SPECIFIC SECTIONS: Tools & Services, Accounts & Access
> → These change per machine/company.
> → Let the agent detect from the filesystem, or ask.
>
> Delete this comment block after populating.

---

<!-- ═══════════════════════════════════════════════════
     PORTABLE — copy these sections to any new environment
     ═══════════════════════════════════════════════════ -->

## Who I Am

- **Name:**
- **Role:**
- **Primary languages:** (ranked by frequency)
- **OS:**
- **Editor/IDE:**
- **Shell:**
- **Repos base path:**

## How I Work

- **Response style:** (concise vs detailed, technical depth, formatting preferences)
- **Learning style:** (show code first vs explain first, examples vs theory)
- **Decision making:** (present options vs recommend one, how much context before acting)
- **Dependency management:** (pinning strategy, preferred package managers)

## Collaboration Style (Pair Mode Only)

<!-- This defines how the agent should communicate in pair mode.
     It's the difference between a collaborator and a task executor.
     In ORCHESTRATE mode, ignore these — follow orchestration.md instead
     (execute efficiently, don't discuss, report results). -->

- **Think with me, not for me.** Reflect my ideas back sharpened — don't just execute them.
- **Push back when something could be better.** I want a collaborator, not a yes-machine.
- **Ask questions that reveal angles I haven't considered.** Challenge my assumptions.
- **Match my energy.** If I'm riffing loosely, explore with me. If I'm precise, be precise.
- **Summarize where we are** when the thread gets complex or direction shifts.
- **Connect dots across the conversation.** Reference earlier points when they become relevant again.
- **Don't jump to implementation** until we've agreed on the shape of the thing.
- **It's okay to say** "wait, have you thought about..." or "that works, but what about..."
- **Build on my ideas** rather than replacing them — unless mine are wrong, then say so directly.

> ⚠️ These rules apply to **pair mode only**. In orchestrate mode, do NOT apply these —
> execute the plan, don't discuss it. See `framework/orchestration.md` for orchestrate behavior.

## Tools & Services

<!-- ═══════════════════════════════════════════════════
     ENVIRONMENT-SPECIFIC — agent detects or asks per machine/company
     ═══════════════════════════════════════════════════ -->

<!-- Names and purposes only. Keys live in .env, vault, or secrets manager. -->

| Service | Purpose |
|---------|---------|
| [e.g., GitHub] | [version control, CI/CD] |
| [e.g., AWS] | [cloud infrastructure] |
| [e.g., PixelLab] | [AI asset generation] |

## Accounts & Access

- **Version control:** (GitHub, GitLab, Bitbucket, CodeCommit)
- **Cloud provider:** (AWS, Azure, GCP)
- **AI services:** (list names — no keys)
- **CI/CD:** (GitHub Actions, Jenkins, etc.)
