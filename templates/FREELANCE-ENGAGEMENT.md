# {Client / Brand} - Engagement log

> Living record of a freelance engagement: what was agreed, what shipped, and how
> much billable time it took. If you track time from your Claude Code session logs
> (see the workflow at the bottom), the time numbers come from there - not from a
> manual stopwatch.
>
> Lean by design. One file per engagement. Update at the end of a billing period,
> not after every session.

## Engagement header

| Field | Value |
|---|---|
| Client | {name or "the client"} |
| Brand / URL | {Brand} - <https://example.com> |
| Project slug | `{slug}` (must match the per-project filter your time tool uses) |
| Start date | {YYYY-MM-DD} |
| Engagement type | {hourly / fixed-price / hybrid / TBD} |
| Rate | {e.g. EUR X/hr, or fixed EUR Y, or TBD} |
| Billing cadence | {weekly / per-milestone / on-completion / TBD} |
| Status | {discovery / active / paused / wrapping up / closed} |

### Scope summary

{One paragraph: what this engagement covers and where the line is. Name what is
explicitly OUT of scope so "could you also..." has somewhere to bounce off.}

### Workstreams

- **A - {name}**: {one line}
- **B - {name}**: {one line, if any}

---

## Deliverable log

The "what they're paying for" record. One row per shipped artifact (research doc,
mockups, deployed feature, handoff doc). Dated, so it lines up with the time log.

| Date | Workstream | Deliverable | Link / location | Notes |
|---|---|---|---|---|
| {YYYY-MM-DD} | A | {e.g. UX teardown report} | {path or URL} | {client-visible? draft?} |

---

## Time log

If you derive billable hours from your Claude Code session logs, **that tool is the
source of truth, not this table.** A session-log timesheet computes a *conservative
floor* on hands-on time (idle gaps capped at a few minutes). It does not count
off-keyboard work - calls, reviewing on the phone, thinking away from the desk. If
those hours are billable, add them as separate, clearly-labelled manual rows.

> **Optional tooling:** this template was dogfooded with a `horizon-timesheet` helper
> (a session-log timesheet generator) that lives outside this framework. Any tool that
> reads Claude Code session transcripts and emits per-project active hours works the
> same way. If you don't use one, just fill the time log manually - the structure below
> is the point, not any specific tool.

Paste the active-hours figure per period below. The "active_h" floor is the billable
number; a "union_h" sanity check (if your tool provides one) confirms overlapping
sessions of the same project were not double-counted.

| Period | active_h | Off-keyboard h | Billable h | Notes |
|---|---|---|---|---|
| {YYYY-Www or range} | {0.0} | {0.0} | {sum} | {what got worked on} |

> **Honesty note on concurrency:** if you context-switch between clients within the
> same minute, that minute can appear under both projects. Never bill two clients for
> the same minute. When in doubt, attribute it to one and say so.

---

## Invoice line items

Built from the deliverable log + time log for one billing period. Copy a block per invoice.

**Invoice {N} - {period}**

| Line | Hours | Rate | Amount |
|---|---|---|---|
| {Workstream A work, period} | {h} | {rate} | {amount} |
| {Workstream B work, period} | {h} | {rate} | {amount} |
| **Total** | **{h}** | | **{total}** |

Deliverables shipped this period: {list, mirrors the deliverable log rows for the period}

---

## Session-to-bill workflow

1. **Work in Claude Code** as normal, in the engagement's code repo and/or this PM folder. No manual timer - the session logs are the timer.
2. **At the end of a billing period**, generate per-project active hours from your session logs (or fill the time log by hand). Add any off-keyboard billable hours manually.
3. **Paste** the active hours into the time log, log the period's deliverables, fill an invoice block, send it.

That's the whole loop. The point is that billable time falls out of the work
automatically, with a defensible audit trail, instead of being reconstructed from memory.
