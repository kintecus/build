---
name: git-week
description: Lightweight git workflow helper for the PM workspace. Handles weekly branch creation, commit message drafting, and activity log queries. Use when the user wants to start a new week's branch, commit work, or review what they did today/this week.
---

# Git week

Help the operator with a lightweight weekly git workflow: start a branch per project per week, draft commits for markdown changes, and review recent activity.

This is an opinionated default. If your operator prefers committing directly to main, or uses a different branching convention, swap this skill out.

## Triggers

Invoke when the user says:

- "start this week's branch"
- "what did i do today/this week"
- "help me commit"
- "weekly summary"
- "new branch for [project]"

## Convention

- **Branch format**: `{project}/week-{YYYY-WW}` where `YYYY-WW` is the ISO 8601 week (`date +%G-W%V`).
- **Merge cadence**: weekly, on Fridays (adjust to your own rhythm).
- **Active projects**: check the workspace `AGENTS.md` "Active projects" section for the current list.

If the workspace has a `GIT-WORKFLOW.md` at the repo root, prefer its conventions over the defaults above.

## Subtasks

### Start a weekly branch

1. Run `git branch --list "{project}/week-*"` to check if a branch already exists for this week.
2. If not, run: `git checkout main && git pull && git checkout -b {project}/week-{YYYY-WW}`.
3. Confirm branch created. Remind user to switch back when working on a different project.

If the user says "start all branches", repeat for each active project.

### Draft commit messages

1. Run `git status` to see what's staged or modified.
2. For each changed file, infer the action from the path and filename:
   - `docs/YYYY-MM-DD-*` → `add {date} {description}`
   - `STATUS.md` → `update {project} status`
   - `comms/` → `draft {filename without date and extension}`
   - `decisions/` → `add decision: {title}`
3. Present one commit command per logical group of changes. Don't batch unrelated files.
4. Ask user to confirm before running.

### Weekly activity summary

1. Run: `git log --since="last monday" --format="%ad  %s" --date=short --all`.
2. Group output by date.
3. Present as a clean bullet list, one item per commit, grouped by day.
4. Optionally: offer to turn this into a status update (trigger the `status-update` skill).

### Today's activity

1. Run: `git log --since="today" --format="%s" --all`.
2. Present as a bullet list.
3. If empty, check for uncommitted changes with `git status`.

## Tone

Be direct. Show the commands you're running. Don't narrate.
