---
date created: 2026-02-19 15:27
updated: 2026-02-20 11:31
---

# Feature spec: Audience broadening (Recommender)

> Source: raw/04-in-progress/2026-02-18-spec-audience-broadening.md
> Processed: 2026-02-18
> Branch: `003-audience-broadening`
> Status: Draft

## TL;DR

The core anti-downselling feature for Phase 1.3. When a forecast detects a narrow audience (CPM too high vs. country benchmark, or max achievable budget < 50% of requested), it recommends persona combinations using OR logic to expand reach. Recommendation is informational only - actual persona creation happens outside the forecasting module. Four user stories covering detection, benchmark config, persona metadata, and hybrid ranking.

## Key points

### Narrow audience detection (FR-001, FR-002)

Triggers when EITHER:

- Recommended CPM > country benchmark CPM × 1.5 (configurable)
- Max achievable budget < 50% of requested budget (configurable)

Both conditions checked independently - OR logic triggers the warning.

### Recommendation strategy by persona type (FR-004, FR-005)

**Basic persona (or purely OR-based combined):**

- Returns up to 3 ranked alternatives
- Each = current persona OR one candidate basic persona
- Exhaustive evaluation of all eligible candidates, return top 3 by hybrid score

**Combined persona with AND clauses:**

- Returns single recommendation - a flattened OR expression
- Decomposes at top-level AND boundaries:
 - Sub-expressions with OR → include as-is
 - Single-persona sub-expressions → expand with best-fit candidate via OR
 - Each branch picks its candidate independently
- Examples:
 - `A AND B` → `A OR candidate1 OR B OR candidate2`
 - `(A OR B) AND (C OR D)` → `A OR B OR C OR D`
 - `A AND (B OR C) AND D` → `A OR candidate1 OR B OR C OR D OR candidate2`

### Candidate eligibility (FR-006)

- Must be basic personas (not combined)
- Must belong to same Brand/Account as current persona
- For combined personas spanning multiple Brand/Accounts: candidate must be available across ALL of them
- Personas with missing/incomplete metadata excluded from pool (FR-017)

### Hybrid ranking score (FR-013, User Story 4)

Two components:

1. **Audience proximity** - cosine similarity of persona manifesto/summary embeddings
2. **Eligible request uplift** - actual request overlap data (pre-computed persona-pair overlap matrix)

Combined inventory formula: `inventory(A) + inventory(B) - overlap(A,B)`

Weights between proximity and uplift: TBD during learn/exploration phase. Must be configurable without code changes.

### Latency

- Narrow-audience cases: up to 2× standard forecast latency acceptable
- Proximity scoring requires embedding service - if unavailable: omit recommendations (do NOT fall back to uplift-only ranking)

### Out of scope

- Automatic creation of combined personas (downstream integration, separate project)
- Impression breakdown by filtering stage (that's the Transparency Funnel spec)

## Dependencies

- Persona metadata (name, Brand/Account, expression, manifesto/summary) staged in data pipeline alongside persona scores (User Story 3)
- Persona-pair overlap matrix pre-computed in pipeline (pre-computation required, not on-the-fly)
- Country benchmark CPM config file (CSV, no-redeploy updates) (User Story 2)
- Embedding/similarity service for proximity scoring
- CLIENT-PERSONA-LEAD (Persona team) for embeddings access - discussion targeting ~Feb 24

## Action items / requirements

- [ ] Market benchmark CPM CSV config (one entry per country + global default)
- [ ] Persona metadata pipeline alongside persona scores
- [ ] Persona-pair overlap matrix pre-computation
- [ ] Narrowness detection logic (CPM + budget thresholds)
- [ ] AND-decomposition logic for combined personas
- [ ] Hybrid ranking score with configurable weights
- [ ] Monitoring metrics: narrow detection count, recommendation count, excluded personas count (FR-019)
- [ ] Calibrate ranking weights as part of feature delivery

## Success criteria

- SC-001: ≥80% of narrow audience detections with persona result in a recommendation with higher estimated inventory
- SC-002: ≥30% of planners who receive recommendations act on them *(requires downstream persona creation integration - cannot measure at launch)*
- SC-003: Campaigns sold >1.5× country benchmark CPM decreases by ≥25% within 3 months *(business outcome, cannot gate launch)*
- SC-004: 100% of recommendations scoped to correct Brand/Account - zero cross-brand leaks
- SC-005: Benchmark config can be updated and takes effect on next request without restart
- SC-006: Full response (including warning + recommendations) returned within 2× standard forecast latency

## Open questions / ambiguities

- Ranking weights between proximity and uplift: not yet defined - learn/exploration phase needed
- Effectiveness of recommendations for CPM-triggered narrowness (price issue vs. volume issue) - to be validated in testing
- ~~"Sufficient volume" threshold: the budget threshold (50%) is proposed as default but not yet confirmed with CLIENT-PRODUCT~~ **Confirmed on Feb 18 weekly sync**: 50% budget threshold and 1.5× CPM multiplier both agreed.
- CLIENT-PRODUCT's recommender Confluence doc (expected EOW Feb 18) may change or clarify scope of this spec
- Transparency funnel is a separate spec (002) - confirmed out of scope for audience broadening (003)
