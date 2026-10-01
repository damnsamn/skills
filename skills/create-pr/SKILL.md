---
name: create-pr
description: Use when asked to open, create, or prepare a pull request from local work, especially when other features are also uncommitted, or when finishing work in a Git worktree.
user_invocable: true
---

# Create PR

You are publishing one requested piece of work as a pull request while preserving the user's other local work.

**Default to the existing working tree.** Multiple unrelated features may share it, including staged changes and mixed-purpose files. Do not create a worktree just to isolate a PR. Use one only when the shared checkout cannot safely support the task or the user requests one, and plan its cleanup before creating it. If a worktree is already being used, finish by cleaning it up as described below.

## Required workflow

1. Identify the requested PR scope and inspect `git status --short`, the current branch, remotes, and the base branch. Inspect staged, unstaged, and untracked changes; distinguish requested work from other features and note which unrelated changes were staged. If the scope or ownership of a hunk is unclear, ask before including it. Check whether the current branch already has commits that would enter the PR.
2. Fetch the remote. By default, bring local `main` up to date with its upstream by fast-forward before creating the feature branch. Preserve the dirty worktree and index while switching branches or updating `main`; Git may refuse either operation if changes overlap. If `main` has diverged or a safe fast-forward/switch is blocked, stop and ask rather than force, reset, stash, or silently base the PR on stale `main`. Honor a different base or explicit instruction to skip syncing when supplied.
3. Create or use the feature branch for this PR. If there is uncommitted PR work, load `commit-for-me` to select, stage, inspect, and commit **only** the requested work. Explicitly identify the PR's feature as its commit scope; an unspecified "make a PR" does not grant scope over every dirty file. Follow that skill's index and hunk safety rules, including its approval requirements before rearranging pre-existing staged changes. This PR request authorizes ordinary, safe branch switches for the workflow, not discarding changes or overriding that skill's index protections. Do not include unrelated commits already on the branch.
4. Compare the branch against the PR base (commits **and** diff) to confirm it contains only the intended work. Read the repository's PR template, if present. Push the feature branch, create the PR against the intended base, and verify its URL, target, and changed files. Use the project's existing checks where applicable; don't claim checks passed unless they did.
5. Return to `main` with the remaining uncommitted changes intact, including their staged/unstaged status where possible. Compare the final status and diffs against the initial inventory: the PR work should be committed, and unrelated work should still be present. Never force a checkout or discard changes to achieve this. If switching back is blocked, leave the work safely where it is and explain the blocker.
6. If a worktree was used for this work (whether you created it or started in it), remove it **after** the PR is created and all remaining changes have been safely retained in the desired checkout. Use `git worktree list` to identify the exact worktree and `git worktree remove <path>` only once it is clean; verify it is gone with `git worktree list`. If it contains changes or is the checkout holding the user's other work, safely relocate that work first or ask how to proceed. Never force-remove a dirty worktree. A PR is not fully wrapped up while its temporary worktree is still hanging around.

**Bad:** Create a fresh worktree for every PR, commit all staged changes, push, then leave the user on the feature branch or leave a stale worktree behind.

**Good:** Sync `main` safely, branch in the current checkout, commit just the named feature via `commit-for-me`, create and verify the PR, then switch back to `main` with the other features still uncommitted.

Before reporting completion, confirm the PR URL, base and branch, final checkout, preservation of unrelated changes, and (if applicable) worktree removal. No exceptions for "the other changes look related" or "the worktree might be useful later."
