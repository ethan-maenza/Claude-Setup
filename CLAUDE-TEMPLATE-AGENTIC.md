# [Repo Name] - Agent Development Guidelines

This document defines how AI agents should work on this codebase.

---

## Core Principles

1. **Never commit directly to `main`** - All changes go through pull requests
2. **Test-Driven Development** - Write tests before implementation
3. **Small, focused PRs** - One feature/fix per PR for easy review
4. **Verify before closing** - Always test/verify your work before marking complete
5. **Never use `cd` commands** - Always use absolute paths to avoid breaking the shell session

---

## Git Safety Guidelines

**Safe Operations:**
- git status/diff/log are always safe - use freely for understanding state
- Destructive operations (reset --hard, clean -fd, force push) require explicit user request

**Commit Practices:**
- Never amend commits unless explicitly asked
- Check git status/diff before making edits (other agents may have changed files)
- For large diffs: Use `git --no-pager diff --color=never` for review

**Agentic Workflow:**
- Create feature branches as part of workflow (Feature Development steps)
- Commit and push as part of completing work
- Small, focused commits over large batches

---

## Branch Naming Convention

Use consistent branch names for easy tracking:

```
feature/{id}-{kebab-case-description}
fix/{id}-{kebab-case-description}
chore/{id}-{kebab-case-description}

Examples:
  feature/42-slack-bot-thread-support
  fix/35-supabase-deploy-timeout
  chore/50-cleanup-dead-code
```

---

## Critical Thinking Guidelines

**Problem Solving:**
- Fix root cause, not band-aids
- If unsure: read more code; if still stuck, ask user with short, clear options
- When conflicts arise: call out the issue and pick the safer path

**Multi-Agent Workflows:**
- Check git status/diff before editing (other agents may have changed files)
- If you see unrecognized changes: assume another agent made them, keep going, focus on your changes
- If unrecognized changes cause issues: stop and ask user

---

## Feature Development Workflow

### For Every Feature or Fix:

1. **Create a feature branch** from `main`:
   ```bash
   git checkout main && git pull
   git checkout -b feature/short-description
   ```

2. **Follow TDD** (see Test-Driven Development section below):
   - Write failing test (Red)
   - Write code to pass test (Green)
   - Refactor while keeping tests green

3. **Test locally** - CRITICAL: Always verify your changes work before pushing:
   - For Supabase Edge Functions: test with curl against local `supabase functions serve`
   - For web apps: Start dev server, test all modified functionality
   - For APIs: Use curl to test endpoints
   - **NEVER skip this step**

4. **Commit with clear messages:**
   ```bash
   git commit -m "Brief description of change"
   ```

5. **Push and create PR:**
   ```bash
   git push -u origin feature/short-description
   ```
   Then create PR with summary of changes.

6. **Wait for review** - User will review diff and approve

7. **After merge**, switch back to main:
   ```bash
   git checkout main && git pull
   ```

---

## Test-Driven Development (TDD)

**For EVERY new feature or fix:**

1. **Write test first (Red)**
   - Create test file: `tests/unit/my-feature.test.ts`
   - Write test that defines expected behavior
   - Run test - should fail

2. **Run test (should fail)**
   ```bash
   [test command for repo]
   ```

3. **Write implementation (Green)**
   - Write minimal code to pass test

4. **Run test (should pass)**
   ```bash
   [test command for repo]
   ```

5. **Refactor while tests pass**
   - Clean up code
   - Keep tests green

6. **Run full test suite**
   ```bash
   [test command for repo]
   ```

---

## Pre-commit Checks

Before every commit, ensure:

```bash
[repo-specific test command]      # Tests pass
[repo-specific lint command]      # Linting passes
[repo-specific format command]    # Formatting correct
```

---

## Claude Code Hooks (Enforcement)

This project uses Claude Code hooks to **enforce** the workflow rules automatically.

### What's Enforced:

| Rule | Enforcement |
|------|-------------|
| No direct commits to `main` | Blocked automatically |
| Tests must pass before commit | Blocked if tests fail |
| Linting must pass | Blocked if lint fails |

### How It Works:

Hooks are configured in `.claude/settings.json` and run automatically when Claude Code executes git commands.

**Location:** `.claude/hooks/pre-commit-checks.py`

### Hook Triggers:
- `git commit` → Runs tests + lint, blocks if on main
- `git push origin main` → Blocked (use PRs instead)

---

## Shell Commands (Critical)

**NEVER use `cd` in bash commands.** The shell session persists between commands, and changing directories breaks Claude Code hooks which expect to run from the project root.

### Bad:
```bash
cd supabase/functions/my-function && deno test
```

### Good:
```bash
deno test /absolute/path/to/repo/supabase/functions/my-function/test.ts
```

---

## Supabase Edge Function Patterns

When working with Supabase Edge Functions:

- Functions live in `supabase/functions/{function-name}/index.ts`
- Shared code goes in `supabase/functions/_shared/`
- Use `Deno.serve()` pattern for request handling
- Webhook endpoints (Slack, etc.) skip JWT verification — authenticate via service-specific signature verification
- Environment variables are set in Supabase dashboard, accessed via `Deno.env.get()`
- Test locally with `supabase functions serve` before deploying

---

## PR Description Template

```markdown
## Summary
Brief description of what this PR does.

## Changes
- List of specific changes made

## Test plan
- [ ] How to verify this works

## Related
- Links to relevant issues/docs
```

---

## [REPO-SPECIFIC SECTIONS BELOW]

[Each repo adds its own sections here — stack details, deployment, env vars, etc.]
