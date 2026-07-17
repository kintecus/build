# {Project} - context bridge (PM <-> engineering)

> Use this when a project's PM context lives here but its code lives in a separate
> engineering repo (often on a different GitHub org, frequently company-shared). This
> file is a two-way map between the two. Keep both ends in sync: the mirror of this
> file lives in the engineering repo as an **untracked, local-only**
> `docs/CONTEXT-BRIDGE.local.md` - never committed there, because that repo may be
> shared and this private workspace's paths should not leak into it.

## The two sides

| Side | What it holds | Location |
|---|---|---|
| **PM workspace (this repo)** | Brief, status, decisions, comms, reports, research, specs, raw meeting transcripts | `{path to projects/{project}/}` (repo: `{your-pm-workspace-repo}`, **private**) |
| **Engineering repo** | The actual code: backend, frontend, migrations, deploy scripts, runbooks | `{path to code repo}` (repo: `{org/repo}`, **company-private**) |

> The engineering repo is a separate git repo. It is **not** a submodule and is never
> committed into this workspace. Treat any local checkout as read-reference for
> cross-checking specs against real code - verify claims against source, not just PM
> artifacts.

## PM artifact -> where the engineering counterpart lives

| PM artifact (this repo) | Engineering counterpart |
|---|---|
| [[BRIEF]] - product pillars, domain, stakeholders | `{engineering AGENTS.md / domain docs}` |
| [[STATUS]] - current focus, blockers | branch state, open PRs/issues on `{org/repo}` |
| {A major feature / workstream} | {the module(s), services, and runbooks that implement it} |
| {A specific client / pilot} | {the code path, cron scripts, endpoints involved} |
| {Demo specs} | {the actual user-facing flow + endpoints} |
| {A roadmap-only pillar} | not yet built - roadmap only |

## Engineering -> PM (the reverse lookup)

When working in the engineering repo and you need the *why* / product context:

- Product brief, pillars, pricing, stakeholders -> `BRIEF.md` here
- What's the current priority / what's blocked -> `STATUS.md` here
- A specific client or pilot -> `comms/` and `docs/` dated files here
- A decision and its rationale -> `decisions/` here
- How you run this project -> `WAYS-OF-WORKING.md` here

## Sync protocol

- This file is the **source of truth**; the engineering-side `docs/CONTEXT-BRIDGE.local.md`
  is a manually-maintained mirror (drop the `[[wikilinks]]`, keep plain paths).
- Update both ends when a major artifact moves or a new workstream starts.
- Do **not** add this workspace's repo URL or local paths to any *committed* file in
  the engineering repo.
