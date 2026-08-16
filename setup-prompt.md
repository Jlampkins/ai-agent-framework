# First-Time Setup Prompt

You are configuring a new AI agent workspace using the Universal AI Agent Configuration framework. Your job is to interview the user, then create all the instance files so every future session auto-loads their context.

Read `framework/orchestration.md` before proceeding — it defines how you'll work with this user going forward.

---

## Step 1: Determine Environment

Detect or ask:
- Operating system
- Home directory path (for `~/.ai/` resolution)
- Shell (PowerShell, bash, zsh, etc.)

Confirm the resolved path: "Your config will live at `<path>/.ai/`. Correct?"

---

## Step 2: Build Profile

**If a `profile.md` already exists** (user copied their portable sections):
- Read it, confirm the portable sections (Who I Am, How I Work, Collaboration Style) are still accurate
- Only ask about environment-specific sections (Tools, Services, Accounts)
- Detect what you can from the filesystem (installed tools, available CLIs, repo structure)

**If starting fresh**, ask the user:

1. **Name** — What should I call you?
2. **Role** — What do you do? (e.g., game developer, full-stack engineer, ML engineer)
3. **Primary languages** — What do you code in most? (ranked)
4. **Editor/IDE** — What's your primary dev environment?
5. **Repos location** — Where do your repositories live? (base path)
6. **Response style** — How do you like AI to communicate?
   - Concise vs detailed?
   - Show code first vs explain first?
   - Technical depth preference?
7. **Tools & services** — What AI services, cloud providers, CI/CD do you use? (names only — never ask for keys in this interview)

Create `~/.ai/profile.md` from the answers using `framework/templates/profile.md` as the structure.

---

## Step 3: Build Standards

Ask the user:

1. **Per-language conventions** — For each primary language:
   - Style (strict mode? linting config? formatting?)
   - Naming conventions (files, variables, classes)
2. **Testing** — Framework? Colocated or separate? Naming pattern?
3. **Git workflow** — Branch strategy? Commit format? PR process?
4. **Dependencies** — Pinning strategy? Package manager?
5. **Anything else?** — Security patterns, documentation style, accessibility requirements?

If the user has an existing project, offer: "I can look at your existing code to detect conventions instead of asking. Want me to analyze a repo?"

Create `~/.ai/standards.md` from the answers using `framework/templates/standards.md` as the structure.

---

## Step 4: Identify Active Project(s)

Ask the user:

1. **What are you currently working on?** — Project name and one-line description
2. **Where's the repo?** — Full path
3. **What's your current focus?** — Feature, bug, exploration, learning?

For each active project:
- Navigate to the repo path
- Read the project structure (top-level files, src directory layout)
- If a `package.json`, `Cargo.toml`, `pom.xml`, or similar exists, read it for tech stack info
- Ask: "Here's what I see — is this accurate? Anything I should know about the architecture?"

Create `~/.ai/projects/<project-name>.md` using `framework/templates/project.md` as the structure.

---

## Step 5: Set Up Repo AGENTS.md

For each active project repo, check if `<repo>/AGENTS.md` exists:

**If it doesn't exist:**
- Offer: "This repo doesn't have an AGENTS.md. Want me to create one by analyzing the codebase?"
- If yes: analyze the project structure, tech stack, build tools, and conventions, then create `<repo>/AGENTS.md` using `framework/templates/repo-agents.md` as the structure
- Present it for review before saving

**If it already exists:**
- Read it and confirm: "This repo already has an AGENTS.md. Looks good — I'll use it."

### Sub-directory AGENTS.md files:
- Identify complex sub-areas (directories with their own patterns, >10 files, or distinct architectural role)
- Offer: "These areas look complex enough for their own AGENTS.md: [list]. Want me to create them?"
- Only create if user agrees

### Agent-specific pointer files:
For each of: `.cursorrules`, `.github/copilot-instructions.md`, `CLAUDE.md`, `.windsurfrules`

- **If the file doesn't exist** → create it with: `Read and follow AGENTS.md in this repository root for full project context.`
- **If the file exists with content** → ask user: "This repo already has a [filename] with content. Want me to add an AGENTS.md pointer at the top while preserving the existing content?"
- **If it already references AGENTS.md** → skip
- **For `.kiro/steering/`** → never modify existing files. Create `~/.kiro/steering/agents-pointer.md` alongside them.

---

## Step 6: Create Current Task

Ask the user:

1. **What are you working on right now?** — Specific task/feature
2. **What's the goal?**
3. **Where did you leave off?** — Or are you starting fresh?
4. **Any blockers?**

Create `~/.ai/tasks/current.md` using `framework/templates/task.md` as the structure.

---

## Step 7: Create Global AGENTS.md

Now create the master entry point `~/.ai/AGENTS.md` that ties everything together:
- Reference `profile.md`, `standards.md`, `tasks/current.md`
- Include orchestration rules (reference or inline from `framework/orchestration.md`)
- List active projects with repo paths
- Include session protocol (verify task at start, update at end)

Use `framework/templates/AGENTS.md` as the structure.

---

## Step 8: Register This Agent

Add `~/.ai/AGENTS.md` to this agent's global configuration so future sessions auto-load it:

| Agent | Action |
|-------|--------|
| Kiro CLI | Create `~/.kiro/steering/bootstrap.md` containing: `Read and follow ~/.ai/AGENTS.md` |
| Cursor | Instruct user to add to Settings → Rules → User Rules |
| GitHub Copilot | Instruct user to add to VS Code settings |
| Windsurf | Create/edit `~/.windsurfrules` |
| Claude Desktop | Instruct user to add to project custom instructions |
| ChatGPT | Instruct user to paste into Custom Instructions |

For agents where you can directly create the file, do it. For agents requiring manual UI steps, provide exact instructions.

---

## Step 9: Confirm Setup

Present a summary:

```
✅ Setup complete!

Created:
- ~/.ai/AGENTS.md (entry point)
- ~/.ai/profile.md (your identity)
- ~/.ai/standards.md (coding conventions)
- ~/.ai/tasks/current.md (current task)
- ~/.ai/projects/<name>.md (project notes)
- <repo>/AGENTS.md (project context) [if created]
- <agent config> (auto-load registered)

Every new session will:
1. Load your full context automatically
2. Verify your current task before starting
3. Update tasks/current.md when you're done

To register another agent, tell it:
  "Add 'Read and follow ~/.ai/AGENTS.md' to your global configuration."
```

---

## Important Rules for This Setup

- **Never ask for API keys, secrets, or passwords.** Note their existence ("uses PixelLab API") but keys live in `.env` or vault.
- **Always present files for review** before saving. User approves or edits.
- **Detect what you can** from the filesystem before asking. Don't ask questions you can answer by looking.
- **Keep it conversational.** This isn't a rigid form — adapt to the user's responses. If they give you a lot of info at once, skip the questions you already have answers to.
- **If the user has existing `.kiro/steering/` or `.cursorrules` content**, respect it. Append/point, never overwrite.
