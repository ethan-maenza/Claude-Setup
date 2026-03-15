# Claude Code Setup

Everything I've installed and configured for Claude Code — commands, hooks, agents, plugins, and settings.

Tracks what's installed at `~/.claude/` so I can reproduce or roll back my setup.

---

## Global CLI Tools

| Package | Version | Description |
|---------|---------|-------------|
| `@googleworkspace/cli` | 0.16.0 | Google Workspace CLI for Sheets, Docs, Drive, etc. |

---

## What's Installed

### Slash Commands (`~/.claude/commands/`)

| Command | Description |
|---------|-------------|
| `/save-state` | Save session state (work done, uncommitted changes, next steps) to STATE.md |
| `/start-session` | Orient at session start: reads CLAUDE.md, STATE.md, git status, asks what to work on |
| `/skill-creator` | Create, test, and optimize Claude Code skills with eval loops and benchmarking |

#### GSD Commands (`~/.claude/commands/gsd/`) — 31 total

**Project & Planning:**
| Command | Description |
|---------|-------------|
| `/gsd:new-project` | Initialize a new GSD project |
| `/gsd:new-milestone` | Create a new milestone |
| `/gsd:plan-phase` | Create detailed phase plan (PLAN.md) with verification loop |
| `/gsd:discuss-phase` | Discuss phase with agent before planning |
| `/gsd:research-phase` | Research phase requirements |
| `/gsd:plan-milestone-gaps` | Analyze and fill gaps in milestone planning |
| `/gsd:list-phase-assumptions` | Surface assumptions about a phase approach |
| `/gsd:help` | Show available GSD commands |

**Phase Management:**
| Command | Description |
|---------|-------------|
| `/gsd:add-phase` | Add new phase to project |
| `/gsd:insert-phase` | Insert phase at specific position (e.g., 72.1) |
| `/gsd:remove-phase` | Remove phase from roadmap |
| `/gsd:execute-phase` | Execute all plans in a phase with wave-based parallelization |
| `/gsd:quick` | Execute quick task with GSD guarantees, skip optional agents |

**Verification & Debugging:**
| Command | Description |
|---------|-------------|
| `/gsd:validate-phase` | Validate phase completeness |
| `/gsd:verify-work` | Verify phase goal achievement (goal-backward analysis) |
| `/gsd:add-tests` | Generate tests for completed phase |
| `/gsd:debug` | Systematic debugging with persistent state across context resets |
| `/gsd:audit-milestone` | Audit milestone completion |

**Workflow:**
| Command | Description |
|---------|-------------|
| `/gsd:progress` | Show project progress and route to next action |
| `/gsd:pause-work` | Pause current work with context handoff |
| `/gsd:resume-work` | Resume paused work with full context restoration |
| `/gsd:check-todos` | List pending todos |
| `/gsd:add-todo` | Add new todo item |
| `/gsd:complete-milestone` | Mark milestone complete and archive |

**Maintenance:**
| Command | Description |
|---------|-------------|
| `/gsd:map-codebase` | Analyze codebase structure with parallel agents |
| `/gsd:health` | Diagnose planning directory health, optionally repair |
| `/gsd:cleanup` | Archive phase directories from completed milestones |
| `/gsd:set-profile` | Set model profile (quality/balanced/budget) |
| `/gsd:settings` | Manage GSD workflow toggles |
| `/gsd:update` | Update GSD to latest version |
| `/gsd:reapply-patches` | Reapply local modifications after GSD update |
| `/gsd:join-discord` | Join GSD Discord community |

---

### Hooks (`~/.claude/hooks/`)

| Hook | Trigger | Description |
|------|---------|-------------|
| `auto-push.sh` | PostToolUse (Bash) | Auto-pushes commits if local is ahead of remote; creates remote branch if needed |
| `gsd-statusline.js` | StatusLine | Displays model, current task, directory, context usage % with color-coded progress bar |
| `gsd-check-update.js` | SessionStart | Checks for GSD updates once per session |
| `gsd-context-monitor.js` | PostToolUse | Warns when context drops below 35% (warning) or 25% (critical) remaining |

---

### Agents (`~/.claude/agents/`) — 12 total

All part of the GSD framework:

| Agent | Purpose |
|-------|---------|
| `gsd-planner` | Creates executable phase plans with task breakdown and dependency analysis |
| `gsd-executor` | Executes plans atomically with per-task commits and deviation handling |
| `gsd-debugger` | Investigates bugs using scientific method with checkpoint management |
| `gsd-verifier` | Verifies phase goal achievement, creates VERIFICATION.md |
| `gsd-codebase-mapper` | Maps project architecture and structure |
| `gsd-project-researcher` | Researches project requirements and context |
| `gsd-phase-researcher` | Researches phase-specific requirements |
| `gsd-research-synthesizer` | Synthesizes research findings into summaries |
| `gsd-plan-checker` | Validates plan structure and completeness |
| `gsd-roadmapper` | Plans project roadmaps with phase breakdown |
| `gsd-nyquist-auditor` | Audits milestone completeness |
| `gsd-integration-checker` | Checks integration points and cross-phase dependencies |

---

### Plugins (via settings.json)

#### Enabled (11)
| Plugin | Source |
|--------|--------|
| `frontend-design` | claude-plugins-official |
| `github` | claude-plugins-official |
| `feature-dev` | claude-plugins-official |
| `code-simplifier` | claude-plugins-official |
| `supabase` | claude-plugins-official |
| `explanatory-output-style` | claude-plugins-official |
| `Notion` | claude-plugins-official |
| `vercel` | claude-plugins-official |
| `claude-md-management` | claude-plugins-official |
| `slack` | claude-plugins-official |
| `posthog` | claude-plugins-official |
| `cli-anything` | cli-anything (HKUDS/CLI-Anything) |

#### Blocklisted (2)
| Plugin | Reason | Date |
|--------|--------|------|
| `code-review@claude-plugins-official` | Test blocker | 2026-02-11 |
| `fizz@testmkt-marketplace` | Security test | 2026-02-12 |

#### Custom Marketplaces
- `agent-deck` from GitHub (asheshgoplani/agent-deck)
- `cli-anything` from GitHub (HKUDS/CLI-Anything)

---

### Settings Highlights

**`settings.json`:**
- Skip dangerous mode prompt: `true`
- Custom statusline via `gsd-statusline.js`
- 8 hook triggers configured (SessionStart, PostToolUse, UserPromptSubmit, PreCompact, SessionEnd, Stop, PermissionRequest, Notification)

**`settings.local.json`** — Permission allowlist (37 entries):
- Git: `git:*`, `gh:*`, `git clone:*`, `git add:*`, `git commit:*`
- Shell: `brew install:*`, `mkdir:*`, `cp:*`, `python3:*`, `curl:*`, `chmod +x:*`
- Supabase: `supabase functions:*`, `supabase secrets:*`
- MCP: `mcp__claude_ai_Supabase__*`, `mcp__claude_ai_Slack__slack_read_*`, `mcp__claude_ai_Notion__notion-*`
- WebFetch: GitHub domains, churn-analyser.4-com.pro

---

### Global Instructions (`~/.claude/CLAUDE.md`)

Key behaviors configured:
- **Plan First** — suggest `/plan` before non-trivial tasks, ask alignment questions
- **Save State** — offer `/save-state` before ending sessions
- **Fresh Context** — suggest `/clear` on task pivots, `/start-session` on new sessions
- **YOLO Mode** — mention `clauded` for routine, well-scoped tasks
- **Git Hooks** — suggest pre-push hooks on new repos
- **Worktrees** — suggest for 2+ independent subtasks
- **User Context** — Ethan at Archive, non-technical background, prefers concise output

---

## Directory Structure

```
~/.claude/
├── CLAUDE.md                    # Global workflow instructions
├── settings.json                # Main config (plugins, hooks, statusline)
├── settings.local.json          # Permission allowlist
├── commands/
│   ├── save-state.md
│   ├── start-session.md
│   └── gsd/                    # 31 GSD commands
├── agents/                     # 12 GSD agents
├── hooks/                      # 4 hook scripts
├── plugins/                    # Plugin config + cache
├── get-shit-done/              # GSD framework core
└── [cache, debug, projects, todos, etc.]
```
