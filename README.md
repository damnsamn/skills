# Sam's skills

Reusable agent skills for the way I work, installable with the [skills CLI](https://skills.sh).

## Install

Install globally and choose your skills and agents interactively:

```sh
npx skills add damnsamn/skills --global
```

Or select an individual skill:

```sh
npx skills add damnsamn/skills --global --skill note
npx skills add damnsamn/skills --global --skill commit-for-me
```

Omit `--global` for a project-local installation.

## Skills

### `note`

Give feedback mid-task without derailing the ongoing work:

> note: that border should be thicker

The agent folds actionable feedback into its work, acknowledges briefly or silently incorporates it, and continues through the original task and its checks. Tentative ideas stay tentative; explicit requests to stop are still honored.

[Read the skill](skills/note/SKILL.md)

### `commit-for-me`

Turn working changes into logical, reviewable commits:

> Commit the changes from this session for me.

The agent inspects the diffs, proposes commit groups, stages precise files or hunks, and reviews each staged diff. By default, only changes made during the current session are in scope; explicitly name a broader scope when needed.

[Read the skill](skills/commit-for-me/SKILL.md)

## Layout

Each skill lives in `skills/<name>/SKILL.md`, with a name and description in YAML frontmatter. Supporting files can live alongside it.
