# Build

An operating system for product and program managers, built on open agent standards. Templates, skills, workflows, and a worked case study for running real projects with a coding agent as a collaborator.

It runs on any agent that supports the [Agent Skills](https://agentskills.io) and [AGENTS.md](https://agents.md) standards - Claude Code, Codex, opencode, Cursor, Gemini CLI, and ~40 others. It was built and is used daily in Claude Code, so that's the best-trodden path, not the only one.

It's a harness you clone and shape to your own style, not a personal vault you inherit mine.

> [!NOTE]
> [The concept](#the-concept) · [Setup](#setup) · [Which agents this runs on](#which-agents-this-runs-on) · [What's in the box](#whats-in-the-box) · [Usage](#usage) · [Philosophy](#philosophy) · [References](#references)

## Why I built this

I'll be straight about what this actually is: an experiment in how far I can automate a PM role with an agentic coding tool. I run it in Claude Code, which is why the narrative below is written from that seat - but nothing in the framework is bound to it.

There are two levels to it. The floor is the boring stuff - the monkey-job parts of program management: status updates, meeting-note cleanup, decision logging, chasing context across a dozen docs. I wanted those off my plate. The ceiling is the part I was genuinely curious about: how far can this actually go if I commit to it? I don't know the answer yet. That's the experiment.

Going in, I had a few hypotheses:

- **Context is more controllable here than in a chat window.** In a chatbot, context is ephemeral - you re-paste, you re-explain, it drifts. In an agentic tool, my source of truth lives in markdown files that *are* the project's context, and the skills and rules I've defined make the agent maintain that context instead of me babysitting it. The bet is that this gives me tighter control over what the agent knows and how it behaves.
- **I can offload more of the remembering to the agent.** Beyond controlling context, I wanted to see how much I can stop holding in my own head - and trust the tool to remember it instead. The markdown files plus auto-memory are the external brain; the test is whether I can lean on them hard enough to actually free up working memory, not just store things I still have to re-read myself.
- **A terminal-first, mouse-free workflow keeps me focused.** I notice how easily I get pulled off task by switching between apps, especially browser-based ones. Staying in the terminal fragments my attention less. And honestly, I find it fun - that counts for something when you want a habit to stick.

These are hypotheses, not conclusions. The case study in `examples/` is the first real-world test of whether they hold. If you're reading this, you're welcome to run the same experiment and see where it takes you.

## The concept

Most PM tooling is either too heavyweight (Notion wikis, Confluence labyrinths) or too personal to share. Build sits between the two: an agent workspace you clone once, tailor to your style, and reuse across every project.

You're meant to tailor it, not just run it as-is:

- **`AGENTS.md`** is where you fill in your operator context - role, background, how you like to work, and your core PM principles. These sections become the agent's standing instructions on every project. (`CLAUDE.md` is a symlink to it, so Claude Code reads the same file.)
- **`WORKSPACE.md`** explains the workspace pattern: launch the agent once from the repo root, hold every project in a single context.
- **`/pm-principles`** (optional, external) runs a ~20-minute interview that distills how you actually work into a `PRINCIPLES.md` other skills can reference. It ships in the author's [pm-principles skill](https://github.com/kintecus/cc-tools/blob/main/commands/pm-principles/SKILL.md), not in this repo - see `examples/PRINCIPLES.md` for what it produces.

For each project, the agent works inside a structured folder of markdown files (`BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`, optional per-project `AGENTS.md`) plus the raw inputs you drop in. The `/onboard-project` interview turns those inputs into living context the agent reads before it acts.

The result is a collaborator who knows your style and has already read the docs, instead of an assistant starting cold every session.

## Setup

```bash
git clone https://github.com/kintecus/build ~/code/build
cd ~/code/build
```

Then fill in the placeholder sections of `AGENTS.md` and onboard your first project. Full walkthrough (pre-commit hooks, optional integrations, shell alias) in [SETUP.md](SETUP.md).

## Which agents this runs on

Build is layered so the portable parts stay portable. Most of its value is plain markdown, which any agent can read:

| Layer | What it is | Runs on |
|---|---|---|
| **Conventions** | `BRIEF`/`STATUS`/`WAYS-OF-WORKING`/`DECISION` templates, the `projects/` folder structure | Anything that reads markdown - including Claude Cowork and chat-only tools |
| **Skills** | The 8 bundled skills, [Agent Skills](https://agentskills.io) standard (`name` + `description` frontmatter, nothing vendor-specific) | Claude Code, Codex, opencode, Cursor, Gemini CLI, Goose, Copilot, and [~40 more](https://agentskills.io/clients) |
| **Instructions** | `AGENTS.md` (canonical) + `CLAUDE.md` symlink | The [AGENTS.md](https://agents.md) readers, plus Claude Code |
| **Extras** | Auto-memory conventions, plugins/`SessionStart` hooks, `.githooks/` | Claude Code specific - see [docs/portability.md](docs/portability.md) for what to swap |

Skills are checked in at `.claude/skills/`, with `.agents/skills` symlinked to it, so both discovery conventions resolve to the same files.

**What's actually been tested:** Claude Code (daily, for months) and opencode (all 8 skills load unmodified, verified with `opencode debug skill`). Codex, Cursor, and the rest follow the same published spec and should work, but I haven't personally run a full engagement in them. Corrections welcome via issue.

For the per-tool detail - what breaks, what to swap, and why Cowork can run the document skills but not `git-week` - see [docs/portability.md](docs/portability.md).

## What's in the box

**Context layer**

- `AGENTS.md` - workspace-level system prompt. Fill in your personal context and working style. `CLAUDE.md` symlinks to it.
- `WORKSPACE.md` - explains the layered instruction-file model (personal → workspace → per-project).

**Templates** (`templates/`)

- `BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`, `DECISION.md` - core project file templates.
- `PROJECT-AGENTS.md` - per-project agent persona template.
- `STRATEGY.md`, `EXPERIMENT.md` - optional. Durable vision/roadmap, and thesis-under-test framing, for projects that need them.
- `CONTEXT-BRIDGE.md` - optional. Maps PM context here to a separate engineering repo when the code lives elsewhere.
- `FREELANCE-ENGAGEMENT.md` - optional. Living log for a billable engagement: scope, deliverables, time, invoices.

**Skills** (`.claude/skills/`, also reachable at `.agents/skills/`)

- `onboard-project` - structured interview that creates a project context folder.
- `ideation` - turns brain dumps into structured specs or PRDs.
- `critique-idea` - steel-mans then pokes holes.
- `status-update` - generates stakeholder updates from project context.
- `review-prep` - fresh-context pre-review punch list (pulse, decisions waiting, stale flags, the 3 questions for the meeting).
- `process-document` - processes raw input materials into `.md` summaries.
- `tune-persona` - sets or updates the per-project agent persona.
- `git-week` - lightweight weekly git workflow helper (branches, commits, activity review).

**Reference docs** (`docs/`)

- `portability.md` - what runs where, per-tool setup, and which parts are Claude Code specific.
- `memory.md` - how to use an auto-memory system across sessions (Claude Code's, or an equivalent).
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
