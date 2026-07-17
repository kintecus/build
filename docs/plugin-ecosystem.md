# Plugin ecosystem

> [!NOTE]
> This is the most Claude Code specific part of the framework. Nothing here is load-bearing - the workspace works without any of it. If you run a different agent, read this for the *pattern* and wire up whatever your tool offers. See [portability.md](portability.md).

Claude Code supports plugins that fire hooks at specific events (session start, tool use, post-tool-use, etc.). This doc lists the plugins the author runs and what each one does. They're not bundled here - swap freely.

## What plugins do in this workflow

Plugins show up as `SessionStart` hooks that print reminders into every session. The pattern: each plugin claims a small scope (branch naming, semver, diagram rendering, retrospectives) and reminds the agent of the relevant rules before work begins.

The effect: durable, cross-session consistency without needing to repeat the rules in every `AGENTS.md` or prompt.

The pattern generalizes even where the wiring doesn't - opencode has plugins, Codex has its own hook surface. Where your tool has no equivalent, the fallback is to put the rule in `AGENTS.md` and accept the context cost.

## Plugins the author uses

### semver

Enforces Semantic Versioning 2.0.0 on commits. Reminds the agent to evaluate each commit for the right version bump (MAJOR/MINOR/PATCH) and checks `.claude-plugin/semver.json` before committing.

Use when you want disciplined version tracking without manual bookkeeping.

### git-branch-naming

Enforces a branch naming convention: `<prefix>/<kebab-case-description>` with a controlled set of prefixes (`feature`, `bugfix`, `hotfix`, `release`, `docs`, `test`, `chore`, `refactor`).

Use when you want new branches to follow convention without having to think about it.

### plantuml

Renders PlantUML diagrams in markdown. Every PlantUML source block gets paired with an image link to `plantuml.com`; the plugin keeps source and image in sync via a pre-commit hook.

Use when you write docs with UML diagrams (sequence, activity, state, class, etc.) and want them visible in GitHub-rendered markdown.

### mermaid

Validates Mermaid syntax in markdown files. Mermaid renders natively in GitHub, Obsidian, and most IDEs - no image link needed. The plugin catches syntax errors before commit.

Use when you prefer Mermaid over PlantUML for simple flowcharts and sequences.

### retroscope

Generates retrospective summaries of agent sessions - daily, weekly, or on demand. Good for end-of-day reflection and for building an evidence base when you want to review how a session actually went vs. how it felt.

Use when you want a habit of closing out your work sessions with a short retro.

## Swap, disable, or roll your own

This list reflects one person's taste. The pattern is what matters, not the specific plugins.

- If you don't care about SemVer, drop the semver plugin.
- If your team uses a different branch convention, fork the branch-naming plugin and adjust the prefix list.
- If you don't draw diagrams, skip both diagram plugins.

Plugins install into `~/.claude/plugins/`. Check the plugin's own README for install instructions.

## The SessionStart hook pattern

If you want to build your own, the basic shape is:

1. Write a script that prints a short set of rules to stdout.
2. Register it as a `SessionStart` hook in your agent's settings (Claude Code's is `~/.claude/settings.json`).
3. The output lands in every session as a system reminder.

Keep the output short. Session-start reminders compete with `AGENTS.md` and user messages for context budget. A rule that fires 100 times per week should be tight.
