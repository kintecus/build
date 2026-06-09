---
date: 2026-03-17
date created: 2026-03-17 10:12
updated: 2026-03-17 10:15
---

# Daily notes - Mar 17 (Tue)

## Context from yesterday

Escalation email still in draft. ENG-LEAD + PM aligned on framing (facts-only, propose parallel v1/vX deploy). DELIVERY-LEAD to finalize and send, then request a call with AdScale for today (Tue). CLIENT-PRODUCT's QA expanded to 20k scenarios - findings confirm the gap is data-driven (model predictions), not code. banner_320x50 absence is the clearest signal. CLIENT-PRODUCT said "let's proceed" but no formal unblock message came through today.

## GitHub PR status (as of morning)

| PR | Title | Author | Review state | AdScale review | Change since yesterday |
|----|-------|--------|-------------|--------------|------------------------|
| #121 | Win rates v2 (service) | ENG-LEAD | **APPROVED** (4 approvals) | CLIENT-DS-LEAD + CLIENT-PRODUCT-MGR approved | No change - ready to merge |
| #122 | CTR/VTR/viewability v3 (service) | ENG-LEAD | **APPROVED** (3 approvals) | CLIENT-DS-LEAD approved (Mar 13) | No change - ready to merge |
| #125 | Win rates v2 ETL | ENG-1 | **APPROVED** | **CLIENT-DS-LEAD approved (Mar 16)** | Was CHANGES_REQUESTED - now approved. v2 pair unblocked. |
| #126 | v4 geoip ZIP (service) | ENG-LEAD | **APPROVED** internally (ENG-2) | Pending AdScale review | No change |
| #127 | v3 ETL CTR/VTR/Viewability | ENG-1 | **APPROVED** internally | CLIENT-DS-LEAD commented (Mar 12), not approved | CLIENT-DS-LEAD's feedback: S3 env variable, dict arg for model paths, add unit tests + local test data |
| #128 | STAGING audience embeddings | ENG-2 | No reviews | Pending review | No change |
| #129 | v4 geoip ZIP (ETL) | ENG-1 | **APPROVED** internally | No AdScale review yet | No change |

**Key changes:**

- **#125 approved by CLIENT-DS-LEAD (Mar 16).** The v2 pair (#121 + #125) is now fully approved and ready to merge. This is the critical path unblock.
- #127 still has open feedback from CLIENT-DS-LEAD (Mar 12): wants S3 env variable (same as #125 fix), a dict arg for model CSV paths, and more unit tests with local test data. ENG-1 needs to address before AdScale will approve.
- #129 has no AdScale review activity yet.

**Deploy readiness:**

- v2 (#121 + #125): **Ready to merge.** Both approved. ML prediction files confirmed on prod S3 (CLIENT-DS-LEAD transferred Mar 16, ENG-1 verified).
- v3 (#122 + #127): Service approved. ETL needs ENG-1 to address CLIENT-DS-LEAD's feedback, then re-review.
- v4 (#126 + #129): Both pending AdScale review.

## Slack updates (Mar 17)

### CLIENT-PRODUCT's QA - proceeded but inconsistencies remain

CLIENT-PRODUCT ran another analysis comparing STAGING vs PROD vs actual data with aligned margins. Shared `QA_STAGING_vs_PROD_vs_Actual.xlsx`. Conclusion: "There are still some inconsistencies but they are already present in prod so let's proceed tomorrow morning."

ENG-LEAD confirmed with CLIENT-PRODUCT: V2 win rates deploying first, then V3 (CTR/VTR/viewability) right after. CLIENT-PRODUCT will create a warning message for users. CLIENT-PRODUCT also wants to "review when we are saturating."

**Status: CLIENT-PRODUCT has conditionally unblocked prod deploy for v2.** The inconsistencies exist in both old and new versions - not a regression. This is a significant shift from "not ready for production."

### Win rate CPM bin granularity

ENG-LEAD flagged that the current 0.5 CPM step in win rates CSV is too coarse - cheap inventory collapses into one bin. Proposed finer discretization (0.05 step at low end, 0.1-0.5 mid-range). CLIENT-PRODUCT agreed: "Good call! Yes for future iteration it would be great to have a 0.1 - 0.5 - 1 step."

**Confirmed: future iteration, not a blocker for current deploy.** Need to verify on daily.

### Pre-deploy: ML prediction files on prod S3

ENG-1 posted pre-deployment conditions - ML prediction CSVs need to be on prod S3 with specific filenames. CLIENT-DS-LEAD confirmed transfer done. ENG-1 verified. **This dependency is resolved.**

File mapping:

- `predictions_file_impression_rate.csv` -> `predicted_win_rate.csv`
- `predictions_file_ctr.csv` -> `predicted_ctr.csv`
- `predictions_file_vcr.csv` -> `predicted_vtr.csv`
- `predictions_file_viewability.csv` -> `predicted_viewability.csv`

### CLIENT-DS-LEAD reviewing PRs + ZIP SQL questions

CLIENT-DS-LEAD asked for the ClickHouse queries that the DAG will execute with ZIP codes. ENG-1 responded with detailed SQL for all 4 stages (history build, history unload, audience build, audience unload). No approval signal yet from CLIENT-DS-LEAD on #127 or #129.

### Audience ZIP data - old audiences lack geo data

CLIENT-PERSONA-LEAD clarified:

- Old audiences (H1 2025) have empty `geoip_zip` - generated before full geo data was available
- Recent audiences (2026 onwards) are fine
- Audiences are scored at creation, then refreshed hourly when activated by a line item
- **New gap identified**: CLIENT-PRODUCT M recommended including `geoip_state` alongside `geoip_zip` to avoid edge cases with repeating ZIP codes across regions. Need to check if service/ETL PRs account for this.

ENG-LEAD's mixed-mode logic (built Saturday) handles partially-populated audiences. CLIENT-PRODUCT M clarified that the issue isn't "transition period" but rather that not all contexts are scored at ZIP level - fallback scores exist at (country, bundle, state) and (country, bundle). Recommended: join first at ZIP level, then without ZIP, coalesce giving priority to ZIP-level scores.

**Action needed:** Confirm with ENG-LEAD/ENG-1 whether `geoip_state` is in the current PRs (#126, #129). If not, this is a scope addition for v4.

## Today's priorities

1. **Escalation email** - finalize with DELIVERY-LEAD. CLIENT-PRODUCT's conditional unblock changes the tone - we can now lead with "v2 deploying to prod" rather than pure escalation. The email shifts from "we're blocked" to "deploying v2, here's what's needed for v3/v4."
2. **v2 prod deploy coordination** - CLIENT-PRODUCT said "proceed tomorrow morning." Who triggers the merge? Who runs the DAG? Coordinate with ENG-LEAD and ENG-1.
3. **#127 feedback** - ENG-1 needs to address CLIENT-DS-LEAD's comments (S3 env variable, dict arg, unit tests). This is the bottleneck for v3 deploy.
4. **geoip_state gap** - check with ENG-LEAD/ENG-1 if PRs #126/#129 include `geoip_state`. CLIENT-PRODUCT M's recommendation.
5. **Daily AdScale ping** - PRs + Phase 2 (per DELIVERY-LEAD's Mar 13 request).
6. **Jira session with ENG-LEAD** - deferred twice already. Getting stale.
7. **CPM bin granularity** - confirm with team this is backlog, not pre-deploy.

## DELIVERY-LEAD sync (quick call)

See [[2026-03-17-delivery-lead-quick-sync]] for full notes.

DELIVERY-LEAD didn't send the escalation email yesterday. Reason: "too verbose and detailed, was too tired to fix it." PM sent the draft mid-day Monday. DELIVERY-LEAD wants three short paragraphs only: (1) process change info, (2) parallel API workaround proposal, (3) team continuity pressure (deallocation notices went out).

Key revelation: DELIVERY-LEAD spoke with CLIENT-DECISION-1 (AdScale) this morning. **CLIENT-DECISION-1 was completely unaware** of any process changes or missed deadlines. He believed CLIENT-PRODUCT's replacement calls and Phase 2 discussions had already started. DELIVERY-LEAD corrected him. CLIENT-DECISION-1 wants the email today and will push internally.

CLIENT-PRODUCT's replacement confirmed as VP of Product (CLIENT-PRODUCT's manager). Open question whether he'll be hands-on like CLIENT-PRODUCT or managerial - affects PM's role scope.

No other projects in Northwind pipeline. If AdScale doesn't extend this week, engagement pauses.

## Deliverables

- **Phase 2 roadmap proposal** (`docs/phase-2-roadmap-proposal.csv`) - 25 items across 4 tiers for ENG-LEAD to share with AdScale. Expanded from initial 16 after reviewing Confluence PDF exports. Covers stabilization, quick wins, platform enhancements, and strategic items. T-shirt sized, dependencies mapped.
- **Processed 5 Confluence PDFs** into `docs/archive/`: MPT interface PRD, V2 PRD, forecast/bid POC model doc, V1 test cases, V2 test cases. Surfaced allow lists, margin management, plan-to-campaign conversion, and media mix table details that fed back into the roadmap.

## (internal DSP) / (third-party DSP) / InStream-CTV analysis

Research into how these concepts relate for the Phase 2 roadmap. Key findings:

**AdScale has two supply paths:**

- **AdScale SSP** (own ad server) - all Phase 1 data comes from here. `delivery_monitoring.full_delivery`, win rate models, prediction CSVs from CLIENT-DS-LEAD. Display + Outstream Video.
- **(internal DSP)** (AdScale's managed DSP, built on (third-party DSP)) - separate demand path for managed campaigns. InStream Video and CTV run exclusively through (internal DSP). (third-party DSP) Curate is the programmatic counterpart.

**What "(internal DSP) integration" means for MPT:**

- Different data source - (internal DSP) has its own delivery data, bid prices, win rates. None flows into the current forecasting pipeline. The `delivery_monitoring` tables are SSP-only.
- No win rate models exist for (internal DSP) inventory. CLIENT-DS-LEAD's models trained on SSP data only.
- Throttling complicates prediction - CLIENT-PRODUCT flagged this (Mar 5). (internal DSP) sits behind a throttler, so raw request counts don't translate to available inventory. The formula `Impressions = Requests x Win_Rate(CPM)` breaks when requests are throttled.
- New filtering dimensions - InStream/CTV adds video length, aspect ratio, skippable/non-skippable, pre/mid/post-roll, device type (CTV/Mobile/Tablet/Desktop). Don't exist in SSP schema.

**Gaps in our knowledge:**

1. CLIENT-PRODUCT promised a (internal DSP) technical document (Mar 5 action item) - never delivered before his departure.
2. Unclear whether Northwind needs direct (third-party DSP) API access or if AdScale has an abstraction layer.
3. The API already has `campaignActivation` (Managed/Programmatic) and margin config handles both - but underlying data is SSP-only for both paths. (internal DSP) integration means actually having different prediction data for the Managed path.
4. Scope unclear: just ETL (new pipeline, same forecasting logic) or fundamentally different forecasting approach for throttled inventory?

**Conclusion:** Discovery phase is genuinely necessary. Dependencies heavier than initially stated - not just "(third-party DSP) API access" but new prediction models, understanding throttler impact, potentially different forecasting approach for managed inventory. InStream/CTV correctly folded into (internal DSP) since those formats only exist on that path.

## Daily sync outcomes

See [[2026-03-17-daily-sync]] for full processed transcript.

### Decisions made

1. **v2/v3 deploy**: CLIENT-DS-LEAD handles S3 file placement + deploy. Sequence: v2 today, v3 tomorrow. CLIENT-PRODUCT's user warning message still needed.
2. **ZIP geoip_state**: ~1 day service + ~1 day ETL to add. Team will flag to AdScale as new input, propose deferring to Phase 2 if Mexico-class countries aren't a priority. Decision burden on AdScale.
3. **ZIP audience partial coverage**: No filtering - dump all data as-is. Three-level join (zip, state, country) with coalesce. ENG-LEAD's pre-filter cascade keeps it performant.
4. **ZIP testing environment**: Propose prod-based testing to CLIENT-PRODUCT (separate endpoint + folder, not connected to live users). STAGING can't handle it.
5. **Escalation email**: Use DELIVERY-LEAD's simpler version (not PM/ENG-LEAD's detailed one). Recipient is CLIENT-DECISION-1 - needs plain language, not technical detail. Three topics: process transparency, missed milestones, Phase 2 intent.
6. **Phase 2 framing**: All new data team inputs (state, partial audiences) = Phase 2 scope per DELIVERY-LEAD. Current Phase 1 sticks to functional DoD. Northwind to prepare Phase 2 proposal for when CLIENT-DECISION-1 confirms intent.

### New information from sync

- ENG-LEAD confirmed PRs #126/#129 do NOT include `geoip_state` - it's a scope addition.
- ENG-2 needs to account for three-level audience scoring in recommender work.
- CLIENT-VP-DS confirmed as not relevant to escalation path - CLIENT-DECISION-1 is the decision-maker.
- DELIVERY-LEAD sending email to CLIENT-DECISION-1 only (not CLIENT-VP-DS/CLIENT-PRODUCT - they're already in context).

## Carry-overs

- [ ] **DELIVERY-LEAD** - send escalation email to CLIENT-DECISION-1 today (simple version). Push for Phase 2 decision this week.
- [ ] **PM** - daily ping to AdScale on PRs and Phase 2 (per DELIVERY-LEAD)
- [ ] **PM** - schedule 1.3 alignment sync (recommendation UX + frontend path)
- [ ] **PM** - Jira session with ENG-LEAD (deferred from Mon - third time)
- [ ] **PM** - prepare Phase 2 scope document for when CLIENT-DECISION-1 confirms intent
- [x] **PM** - get formal DoD v4.2 sign-off from CLIENT-PRODUCT
- [ ] **ENG-LEAD** - communicate ZIP state issue + partial audience coverage to CLIENT-PRODUCT. Propose prod-based testing.
- [ ] **ENG-LEAD** - coordinate v2 deploy with ENG-1. Confirm CLIENT-PRODUCT's user warning message sent.
- [ ] **ENG-1** - v2 deploy (S3 files + merge). Address CLIENT-DS-LEAD's #127 feedback in parallel.
- [ ] **ENG-1/ENG-LEAD** - geoip_state addition (~1d service + ~1d ETL) - pending AdScale decision.
- [ ] **ENG-2** - account for three-level audience scoring (zip/state/country) in recommender.
- [ ] **ENG-2 + ENG-1** - express data analysis (still pending?)
- [ ] **ENG-LEAD** - resolve app list HTTP 500 on STAGING
