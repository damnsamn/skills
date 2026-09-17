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
