# Workspace pattern

This document explains the design choice that shapes everything else in this repo: **your agent runs from a single workspace root, not from individual project folders**.

This pattern is tool-agnostic - it works in any agent that reads an instruction file and walks a directory tree. Examples below use Claude Code because that's where it was built; see [docs/portability.md](docs/portability.md) for the equivalents elsewhere.

## Why one workspace, not one repo per project

Most coding setups give the agent one project at a time. That works for engineering. It breaks for product and program management work, where you routinely:

- Switch between 3-7 active engagements in a day
- Hold cross-project context in your head (this client's Slack thread relates to that other client's vendor decision)
- Reuse the same skills, templates, and habits across projects
- Need a single source of truth for "what am I working on right now"

If each project lives in its own session, you pay the context-loading tax every time you switch. Worse, the agent has no view across projects - it can't help you spot patterns, prioritize across portfolios, or notice when a problem in one engagement has a solution in another.

The workspace pattern fixes this. You launch the agent once, from `~/code/build/` (or wherever you check out this repo), and it has the whole picture: every project's `BRIEF`, `STATUS`, `WAYS-OF-WORKING`, plus shared templates and skills.

## The layered instruction-file model

Three layers, loaded in order. Each layer can override or extend the one above it.

```
~/.claude/CLAUDE.md                # Personal global. Applies to every session, every repo.
~/code/build/AGENTS.md             # Workspace. Loaded when the agent runs from this directory.
~/code/build/projects/X/AGENTS.md  # Per-project. Loaded when the agent reads/writes inside projects/X/.
```

Paths are per tool. The workspace and per-project files are `AGENTS.md`, each with a `CLAUDE.md` symlink beside it, because Claude Code reads only `CLAUDE.md` - including for nested files. The personal-global layer is wherever your tool keeps user-level instructions (`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`, and so on). See [docs/portability.md](docs/portability.md).

What goes where:

| Layer | What belongs | Examples |
|---|---|---|
| **Personal global** | Things true everywhere: tone, formatting preferences, shell aliases, your name | "no emdashes", "use eza flags for ls" |
| **Workspace** | How this collection of projects is organized | "active projects list", "navigation order", "core PM principles" |
| **Per-project** | Agent persona for one engagement | "act as Amazon TPM", "audience is mixed exec/eng", "project-specific decision rules" |

The per-project layer is optional. Many projects don't need it - the workspace defaults are enough. Add a project `AGENTS.md` only when this engagement needs the agent to behave noticeably differently from the workspace default.

See `templates/PROJECT-AGENTS.md` for the per-project template.

## Launching from the workspace

A shell alias keeps this consistent:

```bash
# in ~/.zshrc or ~/.bashrc
alias build='cd ~/code/build && claude'   # or: codex, opencode, cursor-agent, ...
```

Then `build` from anywhere drops you into the workspace with full context loaded. Adapt the alias name to taste (`work`, `pm`, your initials, whatever), and point it at whichever agent you run.

## When projects need their own session

Some projects are genuinely standalone repos with their own instruction file - a SaaS codebase, an open-source library, a personal site. Those keep their own session. The workspace pattern is for *consulting/PM work where the artifacts are markdown and the value is cross-project context*, not for codebases that should be isolated.

Rule of thumb: if the project produces commits to a code repo, it probably wants its own session. If the project produces decisions, status updates, and stakeholder docs, it belongs in the workspace.

## What you give up

Honesty: this pattern has trade-offs.

- **Workspace context window**: the agent loads more on every turn. With many active projects, you may want to prune the "Active projects" list in workspace `AGENTS.md` rather than list every dormant one.
- **Tooling assumes one repo**: some agent features (e.g., Claude Code's per-project memory) treat the workspace as the unit. That's fine for this pattern but worth knowing.
- **Privacy boundary**: every project in the workspace is visible to every session. Don't put a confidential client and a job-search project in the same workspace if you'd rather they not bleed into each other.
