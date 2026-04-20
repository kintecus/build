---
date: 2026-03-17
date created: 2026-03-18
updated: 2026-03-18
---

# Versioned pair deploy plan agreed with AdScale

## Context

Since the Mar 4 decision to batch 1.2A + 1.2B and reorder as datasets-first/ZIPs-second, the team needed AdScale's formal agreement on the deploy sequence. Multiple PRs were open across service and ETL tracks, and the deploy order was discussed internally but never explicitly confirmed with AdScale stakeholders.

ENG-LEAD posted a structured pre-sync message on Mar 17 laying out the full plan: three version upgrades (v2, v3, v4), each deployed as a service PR + ETL PR pair, with auto-detection fallback so each version gracefully handles older data formats. Cleanup PR after all versions are live.

## Options considered

1. **Deploy all at once** - batch all service and ETL changes into a single release. Rejected: too risky, harder to isolate issues, and CLIENT-PRODUCT's QA gate makes incremental validation necessary.
2. **Deploy PRs individually** - merge each PR independently as reviewed. Rejected: service and ETL are coupled per version - deploying one without the other creates an inconsistent state.
3. **Deploy in versioned pairs** (selected) - service PR first, then ETL PR for each version. Auto-detection logic means the service handles data version transitions without downtime. Each pair can be validated independently before proceeding.

## Decision

Deploy in versioned pairs, service PR first then ETL PR:

| Version | What it adds | Service PR | ETL PR |
|---------|-------------|------------|--------|
| v2 | ML predicted win rates | #121 | #125 |
| v3 | ML predicted CTR/VTR/viewability | #122 | #127 |
| v4 | Geo ZIP codes | #126 | #129 |

After all versions deployed and validated in prod, a cleanup PR deprecates legacy version code paths and keeps only v4.

## Rationale

- **Incremental risk**: each pair is independently deployable and validatable. If v3 introduces issues, v2 remains stable in prod.
- **Auto-detection fallback**: service code detects which data version is available on S3 and routes accordingly. No downtime during migration. Each service PR supports all prior versions.
- **Aligns with CLIENT-PRODUCT's QA gate**: CLIENT-PRODUCT can validate each version step before proceeding, without blocking the entire pipeline.
- **Clean deprecation path**: legacy code removed only after all versions confirmed working in prod.

## Who was involved

- **ENG-LEAD** - proposed the plan in pre-sync Slack message
- **CLIENT-PRODUCT** - agreed immediately ("Ok for me"), announced release in AdScale's private channel
- **CLIENT-DS-LEAD** - agreed, took ownership of merge/deploy sequence, merged #121 same day
- **ENG-1, ENG-2** - tagged and aligned

## Outcome

v2 pair (#121 + #125) deployed to production Mar 17-18. First ML predictions integration shipped. CLIENT-DS-LEAD drove the merges proactively. CLIENT-PRODUCT handled user comms. Smoothest cross-team collaboration cycle in the engagement so far.
