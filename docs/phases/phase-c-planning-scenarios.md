# Phase C — Planning sheets & scenarios

**Status:** ⬜ not started
**Days:** D13–D14 · **Plan:** [P3 §Phase C](../fabric-p3-plan-fpna.md) ·
**Requires:** Phase B ✅ (PowerTable rendering actuals correctly)

## Outcome (done criteria)

- [ ] Next-quarter budget built in a Planning sheet with **explicit, named
      assumptions** (volume growth %, fee rate, price drift).
- [ ] Both workflows exercised: **top-down** spread of a target *and* **bottom-up**
      per-account adjustment — with screenshots of each.
- [ ] Three scenarios (base / optimistic / pessimistic) saved and switchable; how Plan
      stores and switches them documented.
- [ ] Assumptions mirrored in `docs/model.md`.

## Decisions (made up front)

| Decision | Choice | Why |
|---|---|---|
| Budget window | Next quarter relative to the actuals' last month (e.g. actuals → 2026-06 ⇒ budget 2026-07 → 2026-09), monthly grain | Small enough to reason about cell-by-cell; big enough to show spreading |
| Assumptions (base) | Volume growth +2 %/month; fee rate held at trailing-3-month average; price drift +1 %/month | Derived from the actuals, not invented — note the actual derived numbers here: ______ |
| Scenario deltas | Optimistic: growth ×2, drift +2 %; Pessimistic: growth 0 %, fee rate +1 pt | Simple, explainable deltas — the demo is scenario *mechanics*, not forecast quality |
| Where assumptions live | Written in the sheet (named cells/inputs where Plan supports it) **and** mirrored in `docs/model.md` | The repo must survive the trial workspace's death |

## Steps

### C1 `[YOU]` Learn first (~45 min, timeboxed)

- [ ] Planning sheets overview + get-started:
      `https://learn.microsoft.com/fabric/iq/plan/planning-overview` and
      `https://learn.microsoft.com/fabric/iq/plan/planning-how-to-get-started`
- [ ] Creating a planning sheet (community docs are ahead of Learn here):
      `https://docs.fabricplan.com/documentation/readme/planning-sheets/how-tos/creating-a-planning-sheet`

### C1.5 `[CLAUDE]` 🎓 Understanding check — planning workflows

- [ ] Quiz (`AskUserQuestion`): top-down vs bottom-up planning (who uses which and
      when); what "spreading" means mechanically (allocation of a parent target to
      leaves — by even split, seasonality, or reference profile); what a scenario is
      *as data* (copy? overlay? versioned rows?) — the honest answer is "let's find
      out in C4", which is the point.
- [ ] Record weak spots for the Phase E drills.

### C2 `[YOU]` Build the budget Planning sheet

- [ ] In `plan_fpna`: create a **Planning sheet** for the budget window (rows:
      account × category; columns: the 3 budget months; measures: revenue, COGS,
      fees).
- [ ] Enter the base assumptions (per Decisions) wherever the sheet supports
      named inputs; otherwise as a clearly-labeled assumptions block. Screenshot.
- [ ] Seed the budget: start from trailing-3-month actuals as the reference profile
      (document how you brought them in — copy from PowerTable? formula? manual?).

### C3 `[YOU]` Exercise both workflows

- [ ] **Top-down:** set a total-revenue target for the quarter (e.g. +5 % vs the
      trailing quarter) and spread it down to account × category × month. Record
      *how* the spread allocated (evenly? pro-rata on history?) — screenshot before/
      after.
- [ ] **Bottom-up:** override two specific accounts by hand (e.g. Account C +20 %
      — "new client"; Account F → 0 — "churned") and watch the totals re-aggregate.
      Screenshot.
- [ ] Note in Gotchas anything that fought back (locked cells, spread granularity,
      recalculation lag).

### C4 `[YOU]` Scenarios: base / optimistic / pessimistic

- [ ] Save the current state as scenario **base**.
- [ ] Create **optimistic** and **pessimistic** per the Decisions deltas; switch
      between all three and screenshot each.
- [ ] Investigate how Plan **stores** scenarios: open the metadata SQL DB (Phase B)
      → re-inspect the tables → find where scenario data landed (new rows? version
      column? separate table?). Screenshot the evidence — this is provisioning-doc
      gold.

### C5 `[CLAUDE]` Document the model

- [ ] Write `docs/model.md`: budget window, assumptions per scenario (the actual
      numbers used), spread mechanics as observed, scenario storage findings from C4,
      and what still isn't understood (honesty beats polish).
- [ ] Update `docs/provisioning.md` with the C4 metadata-DB findings.
- [ ] Commit (`docs(plan): planning model, assumptions and scenario storage`).

### C6 `[CLAUDE]` Close + evidence + 📣 capture

- [ ] Evidence into `docs/evidence/phase-c/` (assumptions block, top-down before/
      after, bottom-up overrides, three scenario switches, metadata-DB scenario rows);
      tick done-criteria, Status ✅, session log.
- [ ] 📣 **Portfolio (capture, not publish):** the scenario-switch triptych is the
      Phase E hero asset — name and keep.

## Gotchas & deviations

*(expected suspects: spreading options thinner than classic CPM tools; scenario
switching latency; sheet size limits on 64 CU; preview save failures — save often)*

## Session log

- 2026-07-10 — Guide written during repo prep. Nothing built yet.
