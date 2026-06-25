# Build

A Claude Code-first operating system for product and program managers. Templates, skills, workflows, and a worked case study for running real projects with Claude as a collaborator.

It's a harness you clone and shape to your own style, not a personal vault you inherit mine.

> [!NOTE]
> [The concept](#the-concept) · [Setup](#setup) · [What's in the box](#whats-in-the-box) · [Usage](#usage) · [Philosophy](#philosophy) · [References](#references)

## Why I built this

I'll be straight about what this actually is: an experiment in how far I can automate a PM role with Claude Code.

There are two levels to it. The floor is the boring stuff - the monkey-job parts of program management: status updates, meeting-note cleanup, decision logging, chasing context across a dozen docs. I wanted those off my plate. The ceiling is the part I was genuinely curious about: how far can this actually go if I commit to it? I don't know the answer yet. That's the experiment.

Going in, I had a few hypotheses:

- **Context is more controllable here than in a chat window.** In the desktop app or a chatbot, context is ephemeral - you re-paste, you re-explain, it drifts. In Claude Code, my source of truth lives in markdown files that *are* the project's context, and the skills and rules I've defined make Claude maintain that context instead of me babysitting it. The bet is that this gives me tighter control over what Claude knows and how it behaves.
- **I can offload more of the remembering to Claude.** Beyond controlling context, I wanted to see how much I can stop holding in my own head - and trust Claude to remember it instead. The markdown files plus auto-memory are the external brain; the test is whether I can lean on them hard enough to actually free up working memory, not just store things I still have to re-read myself.
- **A terminal-first, mouse-free workflow keeps me focused.** I notice how easily I get pulled off task by switching between apps, especially browser-based ones. Staying in the terminal fragments my attention less. And honestly, I find it fun - that counts for something when you want a habit to stick.

These are hypotheses, not conclusions. The case study in `examples/` is the first real-world test of whether they hold. If you're reading this, you're welcome to run the same experiment and see where it takes you.

## The concept

Most PM tooling is either too heavyweight (Notion wikis, Confluence labyrinths) or too personal to share. Build sits between the two: a Claude Code workspace you clone once, tailor to your style, and reuse across every project.

You're meant to tailor it, not just run it as-is:

- **`CLAUDE.md`** is where you fill in your operator context - role, background, how you like to work, and your core PM principles. These sections become Claude's standing instructions on every project.
- **`WORKSPACE.md`** explains the workspace pattern: launch Claude once from the repo root, hold every project in a single context.
- **`/pm-principles`** (optional, external) runs a ~20-minute interview that distills how you actually work into a `PRINCIPLES.md` other skills can reference. It ships in the author's [cc-tools](https://github.com/kintecus/cc-tools) plugin, not in this repo - see `examples/PRINCIPLES.md` for what it produces.

For each project, Claude works inside a structured folder of markdown files (`BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`, optional per-project `CLAUDE.md`) plus the raw inputs you drop in. The `/onboard-project` interview turns those inputs into living context that Claude reads before it acts.

The result is a collaborator who knows your style and has already read the docs, instead of an assistant starting cold every session.

## Setup

```bash
git clone https://github.com/kintecus/build ~/code/build
cd ~/code/build
```

Then fill in the placeholder sections of `CLAUDE.md` and onboard your first project. Full walkthrough (pre-commit hooks, optional integrations, shell alias) in [SETUP.md](SETUP.md).

## What's in the box

**Context layer**

- `CLAUDE.md` - workspace-level system prompt. Fill in your personal context and working style.
- `WORKSPACE.md` - explains the layered CLAUDE.md model (personal → workspace → per-project).

**Templates** (`templates/`)

- `BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`, `DECISION.md` - core project file templates.
- `PROJECT-CLAUDE.md` - per-project agent persona template.
- `STRATEGY.md`, `EXPERIMENT.md` - optional. Durable vision/roadmap, and thesis-under-test framing, for projects that need them.
- `CONTEXT-BRIDGE.md` - optional. Maps PM context here to a separate engineering repo when the code lives elsewhere.
- `FREELANCE-ENGAGEMENT.md` - optional. Living log for a billable engagement: scope, deliverables, time, invoices.

**Skills** (`.claude/skills/`)

- `onboard-project` - structured interview that creates a project context folder.
- `ideation` - turns brain dumps into structured specs or PRDs.
- `critique-idea` - steel-mans then pokes holes.
- `status-update` - generates stakeholder updates from project context.
- `review-prep` - fresh-context pre-review punch list (pulse, decisions waiting, stale flags, the 3 questions for the meeting).
- `process-document` - processes raw input materials into `.md` summaries.
- `tune-persona` - sets or updates the per-project agent persona.
- `git-week` - lightweight weekly git workflow helper (branches, commits, activity review).

**Reference docs** (`docs/`)

- `memory.md` - how to use Claude Code's auto-memory system across sessions.
- `obsidian-integration.md` - how to wire this workspace into an Obsidian vault.
- `plugin-ecosystem.md` - SessionStart hooks and plugins the author uses (swap freely).
- `skills-catalog.md` - full inventory of bundled and referenced skills.

**Worked example** (`examples/`)

- `adscale-case-study/` - a real 3-month ad tech consulting project, fully anonymized. Shows what the framework produces in practice: BRIEF, STATUS, decisions, specs, daily notes, stakeholder comms, retrospectives.

**Tooling**

- `.githooks/` - pre-commit hook that validates PlantUML and Mermaid diagrams.
- `.markdownlint.json` + `.pre-commit-config.yaml` - markdown linting.

## Usage

Projects live in `projects/` (gitignored). Start a new project:

```
/onboard-project {project-name}
```

To see what a fully developed project looks like, browse `examples/adscale-case-study/`.

## Philosophy

- Process as infrastructure, not ceremony.
- Match output weight to idea maturity.
- If something is repeatable, automate or template it.
- Quick capture, quick retrieval - your notes should help your future self, not impress anyone.

## References

- [CHANGELOG.md](CHANGELOG.md) - what changed, newest first.
- [SETUP.md](SETUP.md) - install and first-project walkthrough.
- MIT licensed - see [LICENSE](LICENSE).
