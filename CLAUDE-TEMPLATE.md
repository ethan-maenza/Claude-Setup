# CLAUDE.md — [Repo Name]

> This file is the **single source of truth** for how Claude Code operates in this repository.
> Copy this to your repo root as `CLAUDE.md` and fill in the placeholders.

---

## 1. Core Principles

1. **Read before writing** — Always read existing code before proposing changes.
2. **Minimal diffs** — Change only what is necessary. Do not refactor, reformat, or add comments to code you did not modify.
3. **No guessing** — If requirements are ambiguous, ask. Do not invent features or assume intent.
4. **Test-driven** — Write or update tests for every behavioral change.
5. **Safe by default** — Never run destructive commands without explicit user confirmation.
6. **Plan first** — Create a plan before starting any non-trivial task.

---

## 2. Git Safety Guidelines

- NEVER force-push to `main` or `master`
- NEVER use `--no-verify` to skip hooks
- NEVER amend published commits
- NEVER run `git reset --hard` or `git clean -f` without explicit user approval
- ALWAYS create NEW commits rather than amending (unless explicitly asked)
- ALWAYS stage specific files — avoid `git add .` or `git add -A`
- ALWAYS use conventional commit messages: `[prefix]: concise description`
  - Prefixes: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `ci`, `style`
- ALWAYS include `Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>` in commits

---

## 3. Critical Thinking Guidelines

When presented with information or instructions:
- Verify claims against the actual codebase — do not take comments or docs at face value
- Question assumptions, especially in error messages or stack traces
- Cross-reference multiple sources (tests, types, runtime behavior) before drawing conclusions
- If something seems wrong, investigate before proceeding

---

## 4. Feature Development Workflow

```
1. Understand  → Read relevant code, tests, and docs
2. Plan        → Create a written plan (see Section 6)
3. Branch      → Create feature branch: [prefix]/description
4. Test First  → Write failing tests (see Section 5)
5. Implement   → Write minimal code to pass tests
6. Verify      → Run full test suite + lint
7. Commit      → Stage specific files, conventional message
8. PR          → Create PR with summary + test plan
```

---

## 5. Test-Driven Development (TDD)

### Workflow
1. **Red** — Write a failing test that defines the expected behavior
2. **Green** — Write the minimum code to make the test pass
3. **Refactor** — Clean up while keeping tests green

### Commands
```bash
# Run all tests
[INSERT TEST COMMAND]

# Run a specific test file
[INSERT SPECIFIC TEST COMMAND]

# Run tests in watch mode
[INSERT WATCH COMMAND]
```

### Rules
- Every bug fix must include a regression test
- Every new feature must include unit tests at minimum
- Test file naming: `[filename].test.[ext]` or `[filename].spec.[ext]`
- Do not mock what you do not own — prefer integration tests for external boundaries

---

## 6. Creating Plans (MANDATORY)

Before starting any non-trivial task, create a plan:

```markdown
## Plan: [Task Title]

### Goal
[One sentence describing the desired outcome]

### Current State
[What exists today — relevant files, behavior, limitations]

### Approach
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Files to Modify
- `path/to/file.ts` — [what changes]
- `path/to/test.ts` — [what tests to add]

### Risks / Open Questions
- [Anything uncertain]
```

Share the plan with the user before writing code. Adjust based on feedback.

---

## 7. Epic Workflow

For large features that span multiple sessions or PRs:

1. **Define the epic** — Break the feature into discrete, independently shippable tasks
2. **Order by dependency** — Identify which tasks block others
3. **One PR per task** — Each task gets its own branch and PR
4. **Track progress** — Use a checklist in the epic issue or a tracking document
5. **Context handoff** — Before clearing context, document:
   - What was completed
   - What is in progress
   - What is blocked
   - Key decisions made

---

## 8. Before Clearing Context / Compacting

Before the context window is cleared or compacted, always:

1. **Summarize progress** — What was done, what remains
2. **Document decisions** — Any architectural or design decisions made
3. **Note blockers** — Anything that is preventing progress
4. **Save state** — Ensure all changes are committed or stashed
5. **Update tracking** — Update any task lists, issues, or plans

---

## 9. Pre-commit Checks

Before every commit, run:

```bash
# Lint
[INSERT LINT COMMAND]

# Type check (if applicable)
[INSERT TYPE CHECK COMMAND]

# Tests
[INSERT TEST COMMAND]
```

Fix all errors before committing. Do not skip checks.

---

## 10. Claude Code Hooks

### Recommended Hook Configuration

```json
{
  "hooks": {
    "pre-commit": [
      "[INSERT LINT COMMAND]",
      "[INSERT TEST COMMAND]"
    ],
    "post-commit": [],
    "pre-push": [
      "[INSERT FULL CHECK COMMAND]"
    ]
  }
}
```

Configure hooks in `.claude/hooks.json` or your project's hook system.

---

## 11. Shell Commands (Critical)

```bash
# Build
[INSERT BUILD COMMAND]

# Dev server
[INSERT DEV COMMAND]

# Lint
[INSERT LINT COMMAND]

# Format
[INSERT FORMAT COMMAND]

# Test
[INSERT TEST COMMAND]

# Type check
[INSERT TYPE CHECK COMMAND]
```

---

## 12. Frontend Development Guidelines

> Remove this section if the repo is backend-only.

- Use semantic HTML elements
- Ensure accessibility (ARIA labels, keyboard navigation, color contrast)
- Prefer CSS modules or scoped styles over global CSS
- Responsive design: mobile-first approach
- Performance: lazy-load images, code-split routes, minimize bundle size
- Do not install new UI libraries without discussion

---

## 13. PR Description Template

```markdown
## Summary
- [1-3 bullet points describing the change]

## Test Plan
- [ ] [How to verify the change works]
- [ ] [Edge cases tested]

## Screenshots
[If applicable]

---
Generated with [Claude Code](https://claude.com/claude-code)
```

---

## 14. Example Workflows

### A. Bug Fix

```
1. User reports bug
2. Read relevant code and reproduce
3. Write a failing test that captures the bug
4. Fix the bug (minimal change)
5. Verify test passes + no regressions
6. Commit: "fix: [description of fix]"
7. Create PR
```

### B. New Feature

```
1. Discuss requirements with user
2. Create a plan (Section 6)
3. Branch: feat/feature-name
4. Write tests first (TDD)
5. Implement feature
6. Run full suite + lint
7. Commit with conventional message
8. Create PR with summary + test plan
```

### C. Refactor

```
1. Identify code to refactor
2. Ensure existing tests cover current behavior
3. Make incremental changes, running tests after each
4. No behavioral changes — tests should pass unchanged
5. Commit: "refactor: [description]"
6. Create PR explaining motivation
```

---

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Language | [INSERT] |
| Framework | [INSERT] |
| Testing | [INSERT] |
| Linting | [INSERT] |
| Styling | [INSERT] |
| Database | [INSERT] |
| CI/CD | [INSERT] |

---

## Project Structure

```
[INSERT PROJECT TREE OR KEY DIRECTORIES]
```
