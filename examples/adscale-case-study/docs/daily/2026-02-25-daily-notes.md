---
date created: 2026-02-25 09:00
updated: 2026-02-25 12:58
---

# Daily notes - Feb 25, 2026

## TL;DR

Weekly sync day with AdScale. DoD v3 will not be shared or discussed today - DELIVERY-LEAD and ENG-LEAD decided against it in the daily sync. Agreed sync strategy (ENG-LEAD): raise open questions, and diplomatically surface that PR review rigor is causing the timeline shift - asking AdScale to confirm whether that quality bar is justified for Phase 1. This flips the frame from Northwind accommodating feedback to Northwind asking for alignment on expectations.

## Flags

**DoD v3 - not sharing today (resolved)**

DELIVERY-LEAD and ENG-LEAD decided in the daily sync: we don't share or discuss the DoD with AdScale today. Team will review it separately. Open question: why? Their reasoning wasn't explicit. Worth understanding - is it timing, content concerns, or something about the audience? See open questions below.

**ENG-1's PR status - unclear**

As of evening Feb 24, ENG-1 was still working on his PR. Need to confirm in daily sync: is this the RS->CH migration PR or the ZIP PR? This matters for what we can report in the weekly sync.

## CLIENT-VP-DS's Slack message - analysis

> "Please note that this project takes more time to my team than expected so far, so we would be all happy to regain some time by streamlining this process."

**Frustration level**: moderate, controlled. This is a yellow flag - constructive feedback wrapped in a diplomatic warning. Not escalation, but a signal that the goodwill budget is being drawn down.

**What he's actually saying**:

- PR reviews are costing his team more hours than budgeted
- He sees this as a shared problem, not purely our fault (acknowledged both sides)
- Code reviews are non-negotiable - they're his team's investment against future maintenance
- Cursor bot is adding noise; his team is open to us disabling it
- He's offering a concrete fix: quick call with 2-3 engineers after dev is done, before PR

**What we should do**:

- Acknowledge the time cost directly - don't minimize or defend
- Adopt his suggestion: short call before PR submission to align on approach
- Decide on cursor bot (team should vote - likely disable)
- Frame it as process learning, not failure: first big PR (migration) was always going to be painful, and we don't expect many more of that scale

**What we should NOT do**:

- Get defensive about PR review duration
- Promise faster turnaround without a concrete mechanism
- Ignore this message - it needs acknowledgment in the sync

## Weekly sync prep

Ref: `comms/archive/2026-02-24-sync-questions-slack.md`

**What's our goal for this sync?**

Best case:

- Report ZIP work done (or nearly done)
- Walk through DoD v3 for alignment
- Get gate decisions on embedding service, overlap matrix, recommendation UX
- Acknowledge CLIENT-VP-DS's PR feedback and propose process improvement

Realistic case:

- ZIP status uncertain - may only be able to report migration PR done
- DoD not on the table today (team decision)
- Gate decisions likely deferred again (pattern: CLIENT-PRODUCT needs time)
- PR review framing is the most strategic move available today

**Talking points** (from `comms/archive/2026-02-24-sync-questions-slack.md` + additions):

1. ENG-LEAD's point: confirm evaluate endpoint is out of scope for Phase 1 (RS->CH migration context)
2. Embedding service: confirmed Northwind owns it (CLIENT-VP-DS's Feb 23 Slack). What's the expected timeline and hosting approach? (ENG-2's questions 1-3)
3. Overlap matrix ownership - still open
4. Recommendation UX spec - still waiting from CLIENT-PRODUCT (was due Feb 21)
5. **PR review quality alignment** (ENG-LEAD's reframe): PR review depth is causing timeline shift. We need AdScale to confirm: is this level of quality appropriate for Phase 1? If yes, we align timelines accordingly. If not, we calibrate the bar. Process improvements (pre-PR calls, cursor bot off) are supporting measures either way.
6. Fallback reminder: anything open past Feb 28, we ship committed scope only

## Internal daily sync agenda (before weekly sync)

1. **ENG-1** - which PR are you on? Migration merged? ZIP started?
2. **ENG-2** - migration PR review done? Tagged CLIENT-DS-LEAD?
3. **DoD v3** - has anyone read it? Need feedback now or we go to sync without it.
4. **CLIENT-VP-DS's message** - share with team. Align on response. Cursor bot: keep or disable?
5. **Sync goals** - agree on what we present and who presents what
6. **ENG-LEAD** - evaluate endpoint point for sync agenda

## Action items - Feb 25

- [x] **PM** - run internal daily sync (before weekly sync)
- [x] **Team** - decide on DoD v3 for today's sync (decided: not sharing today)
- [x] **PM** - draft and share internally the [[2026-02-25-weekly-sync]] notes
- [ ] **ENG-1/DELIVERY-LEAD** - prepare response to CLIENT-VP-DS's PR review feedback
- [ ] **ENG-LEAD** - run/attend weekly sync with AdScale
- [ ] **PM/DELIVERY-LEAD** - start updating Miro board (carried from Feb 24, pushed by DELIVERY-LEAD to Feb 26)
- [ ] **ENG-1** - confirm PR status: migration done? ZIP in progress?
- [ ] **ENG-2** - confirm migration PR review status
- [ ] **Team** - decide on cursor bot (disable or keep)

## DELIVERY-LEAD challenge prep

DELIVERY-LEAD's pattern: deflect accountability by framing himself as waiting on someone else. Expect some version of "the ball is not on my side" today. Likely angles and responses:

**"I'm waiting on the DoD to share with AdScale"**

- Reality: DoD v3 was shared with the team yesterday. DELIVERY-LEAD hasn't reviewed it. The ball is on his side - he needs to read it and give feedback, not wait for it to be hand-delivered.
- Response: "The doc was shared yesterday. Did you read it? I need your feedback before I can post it externally - that's the process we agreed on."

**"I'm chasing CLIENT-DECISION-1/CLIENT-VP-DS on Phase 2 extension"**

- Reality: DELIVERY-LEAD raised this on Feb 24 as something he and ENG-LEAD "need to start preparing." No concrete action taken yet. Chasing implies someone else is blocking; in reality, DELIVERY-LEAD hasn't produced a proposal or talking points.
- Response: "What specifically did you send CLIENT-DECISION-1? What's the ask and the timeline? If we're asking for Phase 2, we need a value narrative - what did Phase 1 deliver, what does Phase 2 unlock. That's ours to write, not CLIENT-DECISION-1's."

**"Miro board - I'm waiting on PM"**

- Reality: DELIVERY-LEAD paid for Miro yesterday. We agreed to start updating it together today. If he frames this as blocked on me, push back.
- Response: "We agreed to start today. Let's block 30 min this afternoon. What sections do you want to own?"

**"I've been busy with [internal Northwind thing]"**

- Reality: DELIVERY-LEAD splits time between AdScale and internal Northwind work. This is a valid constraint but not an excuse for items he committed to. If he raises it, acknowledge it but don't let it absorb the daily sync.
- Response: "Understood. What can you commit to this week given your bandwidth? Let's be realistic and I'll plan around it."

**General principle**: Don't argue. Ask for specifics. "What did you send?", "When did you follow up?", "What's the next concrete step?" Forces accountability without confrontation.

## ENG-2's findings - ClickHouse data

**Evaluation pipeline data** (Slack 12:47 PM)

ENG-2 found that the production ClickHouse has a database `admin_data` which appears to be the same one used for the evaluation pipeline. If confirmed, we may be able to switch to it easily rather than building a separate data path. Directed at ENG-LEAD and ENG-1 for validation.

**Persona descriptions** (Slack 12:57 PM)

Table `sandbox.tmp_stg_personae_configs` in ClickHouse contains persona manifesto/descriptions. Currently holds 8,274 personas. ENG-2 flagged uncertainty about whether this table is refreshed periodically - needs confirmation from AdScale. This is relevant for the persona recommender work (embedding service, similarity matching).

**Action items from findings:**
- [ ] **ENG-LEAD/ENG-1** - confirm `admin_data` is the evaluation pipeline DB and assess switch effort
- [ ] **ENG-2/PM** - ask AdScale whether `sandbox.tmp_stg_personae_configs` is periodically updated or a stale snapshot
- [ ] **ENG-2** - clarify: are the 8,274 persona descriptions usable as-is for embedding, or do they need preprocessing?

## CLIENT-DS-LEAD - CLIENT-PERSONA-LEAD's feedback on persona score extraction (Slack, late afternoon)

CLIENT-DS-LEAD relayed feedback from CLIENT-PERSONA-LEAD on the persona score ClickHouse query. Four items:

1. **Remove HTTP filter** - no `web_pages` in the table, so the filter is dead code
2. **Avoid "multidim" term** - naming/terminology cleanup
3. **AVG necessity** - question whether AVG is needed given the table's partition keys (was used in Redshift code, may not apply to ClickHouse)
4. **Add FINAL keyword** - `FROM table FINAL` ensures pending ClickHouse mutations are applied before query runs

**Action items:**
- [ ] **ENG-LEAD/ENG-1/ENG-2** - apply CLIENT-PERSONA-LEAD's four feedback items to persona score extraction query
- [ ] **Team** - confirm whether AVG is structurally needed in ClickHouse or was a Redshift artifact

**Note:** This is our first direct technical feedback from CLIENT-PERSONA-LEAD (Persona team). Good sign - he's engaging on our work. Keep the loop tight.

## Open questions

- DoD v3: DELIVERY-LEAD and ENG-LEAD decided not to share with AdScale today. Reason unstated. Worth a quick question: is it timing, content concerns, or strategic - e.g. don't want to anchor AdScale on a document before the conversation is shaped?
- ENG-1: migration PR or ZIP PR - which one is still in progress?
- CLIENT-VP-DS's frustration: is this just about PR review overhead, or a broader concern about pace?
- Gate decisions (embedding, overlap, UX): will we get answers today or deferred again?
- CLIENT-PRODUCT-BIZ role - still unclear.
