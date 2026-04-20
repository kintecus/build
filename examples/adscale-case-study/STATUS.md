---
date created: 2026-02-18 20:59
updated: 2026-03-31
---

# AdScale - Status

> Last updated: 2026-03-31. **Project complete.** Engagement ended.

## Final state

**Phase 1 is done.** All three deliverables deployed to production and validated by AdScale's product owner. Engagement ended on schedule (Mar 31, 2026). No Northwind action items remain.

| Phase | Status | Production version | Deployed |
| ------------------------------ | ------ | ------------------ | --------- |
| 1.1 Discovery | Done | - | Jan 15-31 |
| 1.2B Model integration | Done | v0.23.0-v0.24.2 | Mar 17-19 |
| 1.2A Geo-enabled forecasts | Done | v0.25.0 + #126 | Mar 23-24 |
| 1.3 Recommendation engine | Done | v0.26.0-v0.26.5 | Mar 26-30 |
| RS-to-CH migration (unplanned) | Done | v0.22.8 | Mar 9 |

**Phase 2 frozen.** No budget approved. Internal champion (CLIENT-PRODUCT) transitioning out of AdScale. No one else at AdScale owns the project.

## Production releases

| Version | Date | What shipped |
|---|---|---|
| v0.22.8 | Mar 9 | RS-to-CH migration |
| v0.23.0 | Mar 17 | v2 win rate predictions (service) |
| v0.23.1 | Mar 17 | v2 win rate predictions (ETL) |
| v0.24.0 | Mar 18 | v3 CTR/VTR/viewability (service) |
| v0.24.1 | Mar 18 | v3 CTR/VTR/viewability (ETL) |
| v0.24.2 | Mar 19 | Naming hotfix |
| v0.25.0 | Mar 23 | v4 ZIP-level ETL pipeline |
| v0.26.0 | Mar 26 | Persona embedding pipeline |
| v0.26.1 | Mar 26 | v4 late-aggregation hotfix |
| v0.26.2 | Mar 30 | Recommender logic (v3/forecast endpoint) |
| v0.26.3 | Mar 30 | API scaled to 4 vCPUs |
| v0.26.4 | Mar 30 | API adjusted to 3 vCPUs (prod fit) |
| v0.26.5 | Mar 30 | Recommender weight rebalance |
| v0.26.6 | Mar 31 | Env args refactoring |

## Handoff items (AdScale-owned)

- V1 -> V2 CMA switch (CLIENT-PRODUCT to coordinate, no contract change needed)
- US latency optimization (15s with 3 vCPUs, target <10s)
- Recommender UI integration (Q2 scope, different AdScale team)
- Docstrings/comments (CLIENT-DS-LEAD's request, partially addressed in #138)
- Jira cleanup (~7 issues still in stale status)

## Retrospective documents

- [[adscale-phase-1-retrospective]] - Private (PM only). Full analysis.
- [[adscale-northwind-retro]] - Northwind internal. Delivery scorecard, team performance, lessons.
- [[adscale-build-framework-retro]] - Public (blog/LinkedIn). Anonymized method retrospective.
