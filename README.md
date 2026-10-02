# Sam's skills

```sh
npx skills add damnsamn/skills --global
```

## `note`

Give feedback mid-task without derailing the ongoing work:

> note: that border should be thicker

The agent folds actionable feedback into its work, acknowledges briefly or silently incorporates it, and continues through the original task and its checks. Tentative ideas stay tentative; explicit requests to stop are still honored.

[Read the skill](skills/note/SKILL.md)

## `commit-for-me`

Turn working changes into logical, reviewable commits:

> Commit the changes from this session for me.

The agent inspects the diffs, proposes commit groups, stages precise files or hunks, and reviews each staged diff. By default, only changes made during the current session are in scope; explicitly name a broader scope when needed.

[Read the skill](skills/commit-for-me/SKILL.md)

## `create-pr`

Create a PR for one feature while other work remains in the same checkout:

> Create a PR for the project pins work.

The agent syncs `main`, branches, uses `commit-for-me` to commit only the requested work, pushes and opens the PR, then returns to `main` with unrelated changes intact. Transferring work into a worktree moves it: after verifying the transfer, the agent removes those changes from the original checkout before continuing, preserving unrelated work. It removes the temporary worktree after publishing the PR and preserving any remaining work.

[Read the skill](skills/create-pr/SKILL.md)
