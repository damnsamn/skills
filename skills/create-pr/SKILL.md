---
name: create-pr
description: Use when asked to open, create, or prepare a pull request from local work, especially when other features are also uncommitted, or when finishing work in a Git worktree.
user_invocable: true
---

# Create PR

You are publishing one requested piece of work as a pull request while preserving the user's other local work.

**Default to the existing working tree.** Multiple unrelated features may share it, including staged changes and mixed-purpose files. Do not create a worktree just to isolate a PR. Use one only when the shared checkout cannot safely support the task or the user requests one, and plan its cleanup before creating it. A dirty file differing between branches does **not** by itself establish a conflict or justify a permission prompt. If a worktree is already being used, finish by cleaning it up as described below.

**Worktree transfers move work; they do not duplicate it.** Once the transfer is verified, remove the transferred changes from the source checkout before doing further work or opening the PR. This cleanup is part of the transfer, not an optional end-of-PR task. Preserve unrelated changes, including their staged/unstaged status.

## Required workflow

1. Identify the requested PR scope and inspect `git status --short`, the current branch, remotes, and the base branch. Inspect staged, unstaged, and untracked changes; distinguish requested work from other features and note which unrelated changes were staged. If the scope or ownership of a hunk is unclear, ask before including it. Check whether the current branch already has commits that would enter the PR.
2. Fetch the remote. By default, bring local `main` up to date with its upstream by fast-forward before creating the feature branch. First try the ordinary `git switch main` with the dirty checkout; Git carries safe changes across and refuses unsafe overwrites. Do not pause just because `git diff main HEAD` shows that a dirty path differs between branches. If Git refuses the switch (or the fast-forward) solely to protect local changes, follow the reversible transfer below instead of immediately proposing a worktree. If `main` has diverged, stop and ask rather than reset it or silently base the PR on stale `main`. Honor a different base or explicit instruction to skip syncing when supplied.
   - Record the current branch, `git status --short`, staged/unstaged diffs, and untracked files. Use `git stash push --include-untracked -m "create-pr temporary transfer"` to park the working changes, record the resulting stash commit ID, then switch to and fast-forward `main` without forcing anything. Apply with `git stash apply --index <stash-id>`; do **not** use `git stash pop`. Compare staged, unstaged, and untracked changes with the inventory before dropping that exact stash entry. If apply conflicts or the inventory cannot be reproduced, keep the stash and stop to resolve the actual conflict; never apply it twice or drop the only copy. Use the same reversible transfer if returning to `main` after committing the PR work requires it. This permission to temporarily park and restore changes is part of the requested PR workflow, not permission to discard or commit unrelated changes.
3. Create or use the feature branch for this PR. If transferring uncommitted work into a worktree, complete the worktree transfer checklist below first. If there is uncommitted PR work, load `commit-for-me` to select, stage, inspect, and commit **only** the requested work. Explicitly identify the PR's feature as its commit scope; an unspecified "make a PR" does not grant scope over every dirty file. Follow that skill's index and hunk safety rules, including its approval requirements before rearranging pre-existing staged changes. This PR request authorizes ordinary, safe branch switches for the workflow and the verified, scoped worktree move below, not discarding changes or otherwise overriding that skill's index protections. Do not include unrelated commits already on the branch.
4. Compare the branch against the PR base (commits **and** diff) to confirm it contains only the intended work. Read the repository's PR template, if present. Push the feature branch, create the PR against the intended base, and verify its URL, target, and changed files. Use the project's existing checks where applicable; don't claim checks passed unless they did.
5. Return to `main` with the remaining uncommitted changes intact, including their staged/unstaged status where possible. If using a temporary worktree, return to the primary checkout first and safely relocate only any remaining uncommitted work; do not reapply the original transferred PR changes. Try `git switch main` first; if Git refuses to carry the changes, use the reversible transfer from step 2. Compare the final status and diffs against the initial inventory: the PR work should be committed and absent from the primary checkout's dirty changes, and unrelated work should still be present. Never force a checkout or discard changes to achieve this. Ask only if a transfer actually conflicts or cannot preserve the work; identify the concrete blocker.
6. If a worktree was used for this work (whether you created it or started in it), remove it **after** the PR is created and all remaining changes have been safely retained in the desired checkout. Use `git worktree list` to identify the exact worktree and `git worktree remove <path>` only once it is clean; verify it is gone with `git worktree list`. If it contains changes or is the checkout holding the user's other work, safely relocate that work first or ask how to proceed. Never force-remove a dirty worktree. A PR is not fully wrapped up while its temporary worktree is still hanging around.

## Required worktree transfer

Complete these steps at worktree creation, before further implementation, commits, or PR creation:

1. Record the source checkout path, branch, `git status --short`, `git diff --binary`, `git diff --cached --binary`, and the contents of in-scope untracked files. Identify the exact files and hunks being moved; unrelated work stays in the source. Retain a recoverable backup outside both checkouts until the transfer and source cleanup are verified.
2. Transfer only the identified work into the destination worktree. Verify its staged and unstaged diffs and untracked file contents against the inventory; preserve staging state. A successful copy command alone is not verification. If anything is missing or conflicts, retain the backup and stop to resolve it before removing source changes.
3. Remove exactly the verified transferred changes from the source index and working tree. For mixed-purpose files, build separate scoped reverse patches for HEAD → index (`git diff --cached --binary`) and HEAD → working tree (`git diff HEAD --binary`); the unstaged diff alone does not capture staged edits in the working tree. Run `git apply --reverse --check` before `git apply --reverse`, using `--cached` for index-only patches. Remove an in-scope untracked source file only after verifying its destination copy and confirming the source has not changed. Never use whole-file restore on a mixed-purpose file, `git reset --hard`, or broad `git clean`. If the source has newer edits or cleanup cannot safely separate hunks, retain all recoverable copies and resolve the concrete blocker rather than guessing.
4. Run `git status --short`, `git diff`, and `git diff --cached` in both checkouts. Confirm the destination owns all transferred work, the source has none of those dirty changes, and unrelated source work and staging match the inventory. The source need not be completely clean. Only after these checks may you release the transfer backup and continue in the worktree.

Moving verified work out of the source is authorized by this workflow; do not ask for separate permission merely because the source is dirty. This does not authorize moving unrelated work or rearranging its index. If starting in a worktree that was already populated, inspect the primary checkout for leftover copies of the transferred work and perform the same verified, scoped cleanup before continuing; if ownership is unclear, ask.

**Bad:** Copy feature A into a worktree, publish its PR, remove the worktree, and leave the original feature A changes dirty in the primary checkout—or defer source cleanup until the PR succeeds.

**Good:** Inventory A and unrelated B, transfer and verify A, remove only A's source hunks and verified untracked files, confirm B's contents and staging are intact, then continue A in the worktree. After publishing the PR, return to the primary checkout and remove the clean temporary worktree.

## Safe branch switching

**Bad:** See that `service-line-selector.tsx` differs between a feature branch and `main`, assume an uncommitted change to the same file will clash, and ask to make a worktree before trying a safe switch or reversible transfer.

**Good:** Try the safe switch; if Git rejects it because of a dirty file, stash with an ID, switch, apply and verify the work before dropping the stash. Sync `main`, commit just the named feature via `commit-for-me`, create and verify the PR, then return to `main` with other features still uncommitted.

## Before reporting completion

1. Confirm the PR URL, base and branch, and final checkout.
2. Confirm unrelated changes and their staging are preserved.
3. If work moved to a worktree, confirm the primary checkout has no leftover dirty copies of that work.
4. If a temporary worktree was used, confirm its removal with `git worktree list`.

Do not skip source cleanup or defer it until PR completion. No exceptions for "the original changes are a useful backup", "the other changes look related", or "the worktree might be useful later."
