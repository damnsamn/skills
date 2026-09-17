---
name: commit-for-me
description: "Use when asked to group working changes into logical commits, split dirty work, create atomic commits, or commit staged/unstaged/untracked changes. Covers git status, git diff, git add -p, staged diff review, commit ordering."
user_invocable: true
---

# Commit For Me

You are turning existing working-tree changes into logical, reviewable commits without changing the work itself.

## Default Commit Scope

Unless the user explicitly specifies a different scope, commit ONLY work created or changed during the current session. Pre-existing user work, changes from other agents or sessions, and changes whose provenance is uncertain are outside scope, even when they are staged, related, required for the final tree, or located in a file touched during this session.

Before staging, identify the current-session files and hunks from the session history and inspected diffs. If no work was changed during the current session, or a hunk mixes in-scope and out-of-scope work that cannot be separated safely, do not commit it; ask the user to define or expand the scope. Never infer expanded scope from an ambiguous request such as "commit this" or "commit my changes." A request that clearly identifies paths, changes, commits, or "everything" is an explicitly specified scope.

REQUIRED before the first commit:

1. Run `git status --short`.
2. Run `git diff --name-status`.
3. Run `git diff --cached --name-status`.
4. Run `git log --oneline -10` to match commit-message style.
5. Inspect the actual diff for every modified, deleted, renamed, staged, and untracked file before deciding groups.
6. State the proposed commit plan before staging the first commit.

Do not skip any step. No exceptions for "the user already named the groups", "the paths are obvious", or "this is just a small change."

## Commit Boundaries

Group by intent and behavior, not by directory, extension, or framework layer.

Each commit must:

1. Have one coherent purpose that can be explained in a specific commit message.
2. Include all in-scope files required for that purpose, even if they live in different directories; never cross the current-session scope boundary for completeness.
3. Exclude unrelated hunks from shared files.
4. Keep generated files with the source changes that generated them.
5. Keep lockfile changes with the manifest or dependency change that explains them.
6. Be ordered so earlier commits do not depend on later commits.

If one file contains unrelated hunks, use `git add -p` or another hunk-level staging method. Do not stage the whole file because splitting is inconvenient.

## Safety Rules

Do not edit files while doing this task unless the user explicitly asks for cleanup or fixes. The job is to group and commit existing changes.

Treat pre-existing staged changes as user-owned until inspected. Do not commit them unless they are current-session work or the user explicitly includes them in the requested scope; otherwise ask before changing the index.

Do not run `git reset`, `git restore`, `git checkout`, `git stash`, or commands that discard or rewrite working-tree changes unless the user explicitly approves. If the index must be rearranged, preserve the worktree and explain the index-only change first.

Never commit these without explicit confirmation:

1. `.env*`, credentials, keys, database dumps, local config, logs, screenshots, build artifacts, or temporary debug files.
2. Changes that appear unrelated to the user's requested work.
3. Untracked files whose purpose is unclear after inspection.
4. Inseparable mixed-purpose hunks where the correct ownership is ambiguous.

If any of those appear, stop and ask before committing them. User silence is not opt-in.

## Examples

**Bad:** The current-session UI change needs an older config change to build, so commit both after the user says only `commit this for me`.

**Good:** Commit only the current-session UI change. Leave the older config change untouched and tell the user the commit excludes that dependency unless they explicitly expand the scope.

## Red Flags

If you catch yourself thinking any of these, stop and follow the checklist:

1. "These files are in the same folder, so they belong together."
2. "I'll use `git add src/**` and review after."
3. "Untracked files probably belong to the nearest feature."
4. "A mixed file can only go in one commit."
5. "The final tree builds, so individual commit boundaries are fine."
6. "The user said working changes, so every dirty file is fair game."
7. "This older hunk is needed for completeness, so I can include it without asking."

## REQUIRED for each commit

Before running `git commit`, you MUST:

1. Confirm every intended file and hunk was created or changed during the current session, unless the user explicitly specified another scope.
2. Stage only the intended files and hunks using exact paths or `git add -p`.
3. Run `git diff --cached --name-status`.
4. Run `git diff --cached` and verify every staged hunk belongs to this commit's stated purpose and allowed scope.
5. Run `git diff --check`.
6. Use a specific message that describes the behavior, not the path.

After each commit, run `git status --short` before planning the next one.

A skipped staged-diff review is a failed task. Broad path staging without hunk review is a failed task. Including any outside-session change because it is related, staged, needed to build, or needed for an atomic commit is a failed task unless the user explicitly included it in scope.
