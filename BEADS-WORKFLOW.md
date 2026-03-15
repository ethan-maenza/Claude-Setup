# Beads Workflow Guide

> Beads (`bd`) is a lightweight workflow tool for tracking progress, decisions, and context across Claude Code sessions.

---

## What Are Beads?

Beads are small, structured markdown files that capture **snapshots of work** — what was done, what was decided, and what comes next. They form a chain (like beads on a string) that gives any future session the context it needs to pick up where the last one left off.

---

## Getting Started

### Initialize

```bash
bd init
```

This creates a `.beads/` directory in your repo root with the necessary structure.

### Directory Structure

```
.beads/
  config.yml          # Beads configuration
  chain/              # The bead chain (ordered snapshots)
    001-initial.md
    002-feature-auth.md
    ...
  templates/          # Bead templates
    default.md
```

---

## Creating a Bead

### At the Start of a Session

```bash
bd new "Brief description of what you're working on"
```

This creates a new bead from the template with:
- Timestamp
- Session goal
- Starting context

### Bead Structure

```markdown
# Bead: [Title]

**Created:** [timestamp]
**Session:** [session identifier]

## Goal
[What this session aims to accomplish]

## Context
[Relevant state from previous beads — auto-populated]

## Progress
- [ ] [Task 1]
- [ ] [Task 2]

## Decisions
- [Decision 1]: [Rationale]

## Blockers
- [Anything preventing progress]

## Handoff
[Summary for the next session — fill this in before ending]
```

---

## Core Commands

| Command | Description |
|---------|-------------|
| `bd init` | Initialize beads in a repo |
| `bd new "title"` | Create a new bead |
| `bd list` | List all beads in the chain |
| `bd show` | Show the current (latest) bead |
| `bd show [id]` | Show a specific bead |
| `bd edit` | Edit the current bead |
| `bd close` | Mark the current bead as complete |
| `bd status` | Show chain status and summary |

---

## Workflow Integration

### With Claude Code Sessions

1. **Session start:** Run `bd new` or `bd show` to load context
2. **During work:** Update the bead with progress, decisions, blockers
3. **Before context clear:** Run `bd edit` to fill in the Handoff section
4. **Session end:** Run `bd close` to seal the bead

### With Git

- Beads are committed alongside code changes
- Include bead updates in your commits: `chore: update bead 003`
- The `.beads/` directory should be tracked in version control

### With Epics

For multi-session features:

1. Create a bead per session within the epic
2. Reference the epic issue/PR in each bead
3. Use the Handoff section to maintain continuity
4. The bead chain becomes a natural log of the epic's development

---

## Best Practices

### Do

- Create a bead at the **start** of every session
- Fill in the Handoff section **before** clearing context
- Keep bead titles descriptive but concise
- Reference file paths and line numbers in Progress notes
- Record **why** decisions were made, not just what

### Don't

- Don't skip the Handoff section — it's the most important part
- Don't create beads for trivial, single-command tasks
- Don't put large code blocks in beads — reference files instead
- Don't edit closed beads — create a new one with corrections

---

## Reading the Chain

To get up to speed on a project:

```bash
bd list          # See the full chain
bd show          # Read the latest bead for current state
bd status        # Get a summary
```

The chain gives you:
- **Timeline** of what was done and when
- **Decision log** of why things are the way they are
- **Context** for picking up any in-progress work

---

## Configuration

### `.beads/config.yml`

```yaml
# Bead chain configuration
version: 1
prefix: "[prefix]"        # Bead numbering prefix
template: "default"        # Default template name
auto_context: true         # Auto-populate context from previous bead
```

Customize the config per repo. The `prefix` should match your project's conventional prefix.

---

## Tips

- **Short sessions?** A bead can be just a Goal + Handoff — keep it lightweight.
- **Long sessions?** Update Progress incrementally so nothing is lost if context compacts.
- **Multiple contributors?** Each person creates their own beads. The chain is shared.
- **Lost context?** `bd show` always gets you back on track.
