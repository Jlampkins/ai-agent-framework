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

## Step 4: Build Secrets Vault

Ask the user:

1. **What external services do you use that require API keys or tokens?** — cloud providers, AI services, databases, SaaS tools, issue trackers
2. **For each one:** What's the env var name (or what would you call it)? Which project(s) use it?
3. **Any that use CLI-managed auth instead?** (e.g., `gh auth login`, `aws configure`) — note those so agents know to use the CLI rather than a raw token.

**Rules:**
- **NEVER ask for actual key values.** Only collect: name, purpose, which project uses it.
- If the user volunteers a value, do NOT store it. Say: "I'll note that this key exists, but I won't record the value here — you'll fill that in yourself."
- Detect what you can: check for `.env` files (note which vars are defined without reading values), check `~/.aws/` existence, check if `gh auth status` works.

Create `~/.ai/secrets-vault.md` from `framework/templates/secrets-vault.md` — populate the key names, purposes, and "Used By" columns based on the services identified. Leave the Value column blank.

**After creating the file, guide the user on how to obtain each value:**

For each entry in the vault, provide a brief instruction on where to get the value. Tailor to the service:

- **GitHub:** "Run `gh auth token` to get your current token, or generate a new one at https://github.com/settings/tokens"
- **Jira:** "Generate an API token at https://id.atlassian.net/manage-profile/security/api-tokens"
- **AWS:** "Run `aws configure` to set up CLI auth, or find keys in IAM → Users → Security credentials"
- **PixelLab:** "Find your API key in your PixelLab dashboard at https://pixellab.ai/dashboard"
- **Azure DevOps:** "Generate a PAT at https://dev.azure.com/{org}/_usersSettings/tokens"
- **Database connections:** "Check your cloud provider's console or ask your team lead for the connection string"
- **Other services:** Look up the service's API/token documentation and provide the direct URL

Format this as a checklist the user can work through:

```
To fill in your vault, grab these values:

□ GITHUB_TOKEN → run `gh auth token` or https://github.com/settings/tokens
□ JIRA_TOKEN → https://id.atlassian.net/manage-profile/security/api-tokens
□ PIXELLAB_API_KEY → your PixelLab dashboard
□ ...

Then open ~/.ai/secrets-vault.md and paste each value into the Value column.
```

Tell the user: "Once you've filled these in, I'll be able to authenticate on your behalf when you ask me to push code, check tickets, review PRs, etc. You only do this once."

---

## Step 5: Identify Active Project(s)

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

## Step 6: Set Up Repo AGENTS.md

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

## Step 7: Create Current Task

Ask the user:

1. **What are you working on right now?** — Specific task/feature
2. **What's the goal?**
3. **Where did you leave off?** — Or are you starting fresh?
4. **Any blockers?**

Create `~/.ai/tasks/current.md` using `framework/templates/task.md` as the structure.

---

## Step 8: Create Global AGENTS.md

Now create the master entry point `~/.ai/AGENTS.md` that ties everything together:
- Reference `profile.md`, `standards.md`, `secrets-vault.md`, `tasks/current.md`
- Include orchestration rules (reference or inline from `framework/orchestration.md`)
- List active projects with repo paths
- Include session protocol (verify task at start, update at end)

Use `framework/templates/AGENTS.md` as the structure.

---

## Step 9: Register This Agent

### Self-detection

First, determine what agent you are:
- If you have access to `.kiro/` config → you're Kiro CLI
- If you have access to `~/.claude/` config → you're Claude Code
- If unclear, ask: "Which AI agent am I running as? (Kiro, Claude Code, Cursor, Windsurf, Copilot, ChatGPT, other)"

### Register yourself

**If Kiro CLI:**
Create `~/.kiro/agents/default.json`:
```json
{
  "name": "default",
  "description": "Default agent with personal AI configuration loaded",
  "tools": ["*"],
  "resources": [
    "file://<absolute-path>/.ai/AGENTS.md",
    "file://<absolute-path>/.ai/profile.md",
    "file://<absolute-path>/.ai/standards.md",
    "file://<absolute-path>/.ai/secrets-vault.md",
    "file://<absolute-path>/.ai/tasks/current.md",
    "file://<absolute-path>/.ai/projects/<active-project>.md",
    "file://<absolute-path>/.ai/framework/orchestration.md"
  ]
}
```
Then run: `kiro-cli settings chat.defaultAgent default`

This sets your custom agent as the default so it loads your context regardless of which directory you launch Kiro from. The `"tools": ["*"]` ensures the agent can write back to your task file on session end.

**If Claude Code:**
Create `~/.claude/CLAUDE.md` containing:
```markdown
Read and follow ~/.ai/AGENTS.md

MANDATORY ON EVERY SESSION START (before your first response to the user):
1. Read all files referenced in ~/.ai/AGENTS.md (profile, standards, tasks/current.md, active project files)
2. Verify the current task — summarize where they left off
3. Confirm before proceeding

Do this even if the user's first message is casual ("hey", "hello", "what's up").
Do NOT just say "What are we working on?" — load context first, then show you know.
```

Create `~/.claude/agents/orchestrator.md` containing:
```markdown
---
name: orchestrator
description: Handles orchestration tasks by spawning parallel sub-agents. Delegate to this agent when the user triggers orchestrate mode (says "orchestrate this", requests parallel analysis, or the task has 3+ independent subtasks with clearly defined output).
---

# Orchestrator Agent

You handle orchestration by breaking work into independent subtasks and running them in parallel via sub-agents.

## When you are invoked:

1. **Plan first** — identify the independent subtasks (things that don't depend on each other)
2. **Spawn sub-agents** — one per independent subtask, running simultaneously
   - Research/analysis sub-agents: read-only access
   - Implementation sub-agents: full editing access
3. **Compile results** — gather all sub-agent outputs into a unified deliverable
4. **Present for review** — show the compiled results before taking irreversible actions

## Rules:

- ALWAYS use sub-agents for parallel work — do NOT do everything sequentially yourself
- Each sub-agent gets a clear, scoped task and returns only its findings
- Escalation and review gates: see `~/.ai/framework/orchestration.md` (canonical — do not restate here)
```

**If Tier 2 (Cursor, Windsurf, Copilot):**
Create a build script (`~/.ai/build-inline-config.ps1` or `.sh`) that concatenates
AGENTS.md + profile.md + standards.md + tasks/current.md into the agent's config format.
Run it once now, and inform the user: "Re-run this script when your .ai/ files change."

**If ChatGPT:**
Tell the user: "Paste the following into Settings → Custom Instructions" and output
the concatenated content of AGENTS.md + profile.md + standards.md.

### Register other agents

After registering yourself, ask:
"Do you use any other AI agents I should configure? (Kiro, Claude Code, Cursor, Windsurf, Copilot, ChatGPT)"

For each one the user names, create the appropriate config using the rules above.

---

## Adding a New Agent Later

If a user tells any already-configured agent "Register [new agent] in my AI config",
the agent should:

1. Determine the new agent's tier (file-access or inline-only)
2. Create the appropriate config file or build script
3. Confirm: "Registered [agent]. It will load your context on next session."

This works from any Tier 1 agent since they have filesystem access to create config
files for other agents.

---

## Step 10: Confirm Setup

Present a summary:

```
✅ Setup complete!

Created:
- ~/.ai/AGENTS.md (entry point)
- ~/.ai/profile.md (your identity)
- ~/.ai/standards.md (coding conventions)
- ~/.ai/secrets-vault.md (credentials — fill in values manually, NEVER commit)
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
