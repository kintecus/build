# Skills catalog

Skills are the reusable behaviors your agent can invoke on its own, or that you trigger with `/skill-name`. They follow the [Agent Skills](https://agentskills.io) open standard, so they aren't tied to any one tool. This catalog covers what ships in this repo and what patterns the author uses elsewhere.

## Bundled in this repo

Skills live under `.claude/skills/`, with `.agents/skills` symlinked to the same folder so both discovery conventions find them. See [portability.md](portability.md) for why.

| Skill | Purpose |
|---|---|
| `onboard-project` | Structured interview that creates `projects/{name}/` with BRIEF, STATUS, WAYS-OF-WORKING filled in from input docs. |
| `ideation` | Turns brain dumps and half-formed ideas into structured one-pagers, specs, or PRDs. Chooses output weight based on idea maturity. |
| `critique-idea` | Steel-mans an idea first, then identifies gaps, risks, and alternatives. Use when you want honest pushback. |
| `status-update` | Generates stakeholder status updates from the BRIEF + STATUS + recent comms in a project folder. |
| `review-prep` | Dispatches a fresh-context agent over STATUS + docs + decisions to produce a pre-review punch list: pulse, decisions waiting on you, decisions recently closed, stale/at-risk flags, and the 3 questions worth the meeting. The cold read catches drift and cross-artifact risks your own working memory misses. |
| `process-document` | Processes a raw file (PDF, transcript, CSV export) from `raw/` into a structured markdown summary in `docs/`. |
| `tune-persona` | Sets or updates the per-project agent persona (the project-level `AGENTS.md`). Archetype, document standards, decision rigor, communication style. |
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

A skill is a folder containing a `SKILL.md` with YAML frontmatter. Per the [specification](https://agentskills.io/specification), only two fields are required:

- `name` - lowercase, hyphens, max 64 chars, and it must match the folder name.
- `description` - max 1024 chars. This is what the agent reads at startup to decide whether the skill is relevant.

The `description` does the real work. A precise one that names specific triggers ("use when user says X or is doing Y") beats a vague one. See any bundled skill for the shape.

Optional fields (`license`, `compatibility`, `metadata`, `allowed-tools`) are in the spec but rarely needed. Anything outside the spec is a vendor extension and other tools will ignore it - keep to `name` + `description` and your skill runs everywhere.

**Avoid `disable-model-invocation: true`** - a Claude Code flag, not part of the spec. It blocks programmatic invocation via the `Skill` tool, not just auto-triggering. Use a tight description instead.

Validate before committing:

```bash
skills-ref validate ./my-skill
```

## Personal vs. shared skills

There are two good places to put skills:

- Personal, global - applies to every repo you work in. Path depends on your agent: `~/.claude/skills/`, `~/.agents/skills/`, or `~/.config/opencode/skills/`.
- Workspace, shared - belongs with a specific project or set of projects. Here that's `<workspace>/.claude/skills/`, reachable at `.agents/skills` too.

The bundled skills above are workspace-shared. The referenced-but-not-bundled skills are typical candidates for personal-global.

## Finding more skills

- The Agent Skills spec and client list: <https://agentskills.io>
- The author's GitHub org (Tribe Coding) publishes a public skill marketplace: <https://github.com/tribe-coding>
- Anthropic's skill documentation and examples: <https://code.claude.com/docs/en/skills>
- OpenAI's skills catalog: <https://github.com/openai/skills>
