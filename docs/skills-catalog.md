# Skills catalog

Skills are the reusable behaviors Claude can invoke with the `Skill` tool (or the user via `/skill-name`). This catalog covers what ships in this repo and what patterns the author uses elsewhere.

## Bundled in this repo

Skills under `.claude/skills/`:

| Skill | Purpose |
|---|---|
| `onboard-project` | Structured interview that creates `projects/{name}/` with BRIEF, STATUS, WAYS-OF-WORKING filled in from input docs. |
| `ideation` | Turns brain dumps and half-formed ideas into structured one-pagers, specs, or PRDs. Chooses output weight based on idea maturity. |
| `critique-idea` | Steel-mans an idea first, then identifies gaps, risks, and alternatives. Use when you want honest pushback. |
| `status-update` | Generates stakeholder status updates from the BRIEF + STATUS + recent comms in a project folder. |
| `process-document` | Processes a raw file (PDF, transcript, CSV export) from `raw/` into a structured markdown summary in `docs/`. |
| `tune-persona` | Sets or updates the per-project agent persona (the project-level `CLAUDE.md`). Archetype, document standards, decision rigor, communication style. |
| `git-week` | Lightweight weekly git workflow: start a week branch, draft commit messages, review activity. |

## Referenced but not bundled

The author's personal setup includes several skills that depend on individual habits (Obsidian vault, PR workflow, end-of-day reflection). They're worth replicating rather than copying wholesale. A non-exhaustive list:

| Skill | What it does | Why not bundled |
|---|---|---|
| `daily-note` | Read/append to an Obsidian daily note | Assumes an Obsidian vault at a specific path |
| `obsidian-vault` | Navigate and search the Obsidian vault | Same |
| `clippings-digest` | Summarize unreviewed web clippings | Assumes a `Clippings/` folder structure |
| `pr` | Create PRs with the author's PR template | Opinionated about PR format |
| `commit` | Stage related changes and draft conventional commit messages | Opinionated about commit style |
| `reflect` | End-of-day review against the daily-note plan | Depends on `daily-note` |
| `prose-deslop` | Polish AI-generated text into the author's writing voice | Personal voice profile |
| `amazon-writing` | Apply Amazon 6-pager writing rigor to a doc | Personal standard |
| `research` | Research a technology/API via web search | Generic; use what you prefer |
| `tune-persona` (bundled above) | Sets per-project agent persona | Bundled - generally useful |

## How to add your own skills

Skills live in `.claude/skills/<skill-name>/` with a `SKILL.md` that follows the Claude Code skill format. The key is the `description` frontmatter - it's what Claude uses to decide whether to invoke the skill.

A precise description that names specific triggers ("use when user says X or is doing Y") works better than a vague description. See any bundled skill for the format.

**Avoid `disable-model-invocation: true`** - it blocks programmatic invocation via the `Skill` tool, not just auto-triggering. Use a tight description instead.

## Personal vs. shared skills

There are two good places to put skills:

- `~/.claude/skills/` (personal, global) - skills that apply to every repo you work in.
- `<workspace>/.claude/skills/` (workspace, shared) - skills that belong with a specific project or set of projects.

The bundled skills above are workspace-shared. The referenced-but-not-bundled skills are typical candidates for personal-global.

## Finding more skills

- The author's GitHub org (Tribe Coding) publishes a public skill marketplace: <https://github.com/tribe-coding>
- Anthropic's own skill documentation and examples: <https://docs.claude.com/en/docs/build-with-claude/agent-skills>
