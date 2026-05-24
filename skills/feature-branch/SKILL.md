---
name: feature-branch
description: Use when starting any development, implementation, coding, or feature work. Sets up an isolated git worktree on a new feature branch, commits and pushes work incrementally during development, and submits a pull request when complete. Required for ALL development tasks — no direct commits to main/master. Triggers on requests like "implement X", "fix Y", "add feature Z", "refactor A", or any task that requires writing or changing code.
---

# Feature Branch

## Overview

All development work follows this lifecycle: isolated worktree → feature branch → incremental commits and pushes → pull request.

**Core principle:** Never commit to main. All work goes on a feature branch, gets pushed to remote, and merges via PR only.

## Phase 1: Setup

Find a skill to create the Git worktree and feature branch. Always pull the latest changes from the primary branch before starting a new feature branch.

**Branch naming — use descriptive kebab-case with type prefix:**

| Type          | Prefix     | Example              |
| ------------- | ---------- | -------------------- |
| New feature   | `feature/` | `feature/user-auth`  |
| Bug fix       | `fix/`     | `fix/login-redirect` |
| Maintenance   | `chore/`   | `chore/update-deps`  |
| Documentation | `docs/`    | `docs/api-reference` |

## Phase 2: Development

Work inside the worktree. Commit and push after each logical unit of work.

### Commit discipline

* One logical change per commit
* Conventional commit format: `<type>(<scope>): <description>`
* Examples: `feat(auth): add JWT validation`, `fix(api): handle null response`

### Push immediately after every commit

```bash
git push -u origin <branch-name>   # First push — sets upstream
git push                            # All subsequent pushes
```

Never leave committed work unpushed. Push is mandatory after each commit.

## Phase 3: Completion

When implementation is complete:

### 1. Verification

Check project-appropriate commands to test and verify your work.

Fix failures before proceeding. Do not create a PR with failing tests.

### 2. Confirm everything is pushed

```bash
git status           # Must show clean working tree
git log @{u}..HEAD   # Must show no unpushed commits
```

Push anything remaining before creating the PR.

### 3. Create pull request

```bash
gh pr create --title "<concise title under 70 chars>" --body "$(cat <<'EOF'
## Summary

- <what changed>
- <why it was needed>
EOF
)"
```

### 4. Clean up worktree

```bash
git worktree remove <worktree-path>
```

Report the PR URL to the user.

## Red Flags

**Never:**

* Commit directly to main or master
* Leave committed work unpushed
* Create a PR with failing tests
* Merge locally — always use a PR

**Always:**

* Create an isolated worktree for every development task
* Push after every commit
* Submit a PR when done, not a local merge
* Clean up the worktree after PR creation
