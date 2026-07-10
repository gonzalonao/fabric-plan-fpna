# Phase B — Plan item & PowerTable model

**Status:** ⬜ not started
**Days:** D13 · **Plan:** [P3 §Phase B](../fabric-p3-plan-fpna.md) ·
**Requires:** Phase A ✅ (`sm_plan_actuals` published, XMLA outcome known)

## Outcome (done criteria)

- [ ] Plan item `plan_fpna` created; **everything it auto-provisioned inventoried** in
      `docs/provisioning.md` (the write-up nobody has published yet — a headline
      deliverable of this project).
- [ ] PowerTable sheet renders actuals correctly against `sm_plan_actuals`
      (account × category × month), spot-checked against known numbers.
- [ ] SQL DB budget updated: metadata DB counted (1 of 3 used).

## Decisions (made up front)

| Decision | Choice | Why |
|---|---|---|
| Item naming | Plan item `plan_fpna`, created at `ws-plan-dev` root | Its auto-created children land wherever Fabric puts them — part of what we're documenting |
| Documentation posture | **Screenshot-first**: capture every dialog of the creation flow *before* clicking through it | Preview UIs change; the provisioning walkthrough is only credible with contemporaneous captures |
| Debug timebox | 2 h per blocker, then log it in `docs/gotchas.md` and route around | Master-plan rule; in a preview this deep, gotchas ARE the deliverable |
| Git expectations | Attempt a Source-control commit after creation; if Plan items aren't Git-syncable, that's a **documented finding**, not a failure | Early-adopter evidence includes knowing what the tooling can't do yet |

## Steps

### B1 `[YOU]` Learn first (~45 min, timeboxed)

- [ ] PowerTable overview: `https://learn.microsoft.com/fabric/iq/plan/powertable-overview`
- [ ] Skim the Plan get-started flow so the creation dialogs are familiar before you
      click: `https://learn.microsoft.com/fabric/iq/plan/planning-how-to-get-started`

### B2 `[YOU]` Create the Plan item (screenshot everything)

- [ ] `ws-plan-dev` → **+ New item** → search **Plan (preview)** → name `plan_fpna`.
- [ ] Screenshot **each step** of the creation wizard (model binding, any database
      prompts) as you go — before/after workspace item lists included.
- [ ] When asked for the data source / semantic model, bind `sm_plan_actuals`.
- [ ] After creation: screenshot the workspace item list again — the auto-created
      **Fabric SQL database** (plan metadata) should be visible. That's **slot 1 of
      3** on the SQL DB budget.

### B3 `[YOU]` + `[CLAUDE]` Inventory what got provisioned

- [ ] `[YOU]` Open every auto-created child item; capture: item names/types, the SQL
      DB's schema (open its query editor → expand tables → screenshot), connection
      details pane, and any settings pages.
- [ ] `[CLAUDE]` Write `docs/provisioning.md` from the captures: exact item list,
      metadata DB table inventory (names + apparent purpose), what's user-editable vs
      managed, how it counts against trial limits. This is the "nobody has written
      this up publicly yet" doc — thorough beats fast.
- [ ] `[YOU]` Workspace → Source control: note **which** of the new items appear as
      committable changes; commit what does
      (`feat(plan): create plan item and document provisioning`); log what doesn't
      in Gotchas.

### B4 `[YOU]` PowerTable sheet over actuals

- [ ] Inside `plan_fpna`: create a **PowerTable sheet** bound to `sm_plan_actuals`.
- [ ] Layout: rows = account (from `dim_account`), nested category; columns = month
      (from `dim_date`); values = revenue / COGS / fees.
- [ ] Verify the numbers: pick two known cells (e.g. Account A, 2026-03, revenue) and
      cross-check against the Phase A Analyze-in-Excel pivot. Exact match required —
      a mismatch means the model binding or aggregation is wrong; stop and diagnose.
- [ ] Note how PowerTable handles members/hierarchies vs the model's dims (does it
      read dimension tables directly? handle new members?) — bullet the observations
      in `docs/provisioning.md`.

### B5 `[CLAUDE]` 🎓 Understanding check — Plan architecture

- [ ] Quiz (`AskUserQuestion`): what physically got created when the Plan item was
      born and where plan data will live vs where actuals live; PowerTable vs
      Planning sheet vs Intelligence sheet vs Infobridge (one sentence each); why the
      metadata DB burns a trial SQL-DB slot and what the remaining budget is.
- [ ] Diagram the Plan architecture as currently understood (semantic model ↔ Plan
      item ↔ metadata SQL DB ↔ sheets) — this diagram gets refined in Phases C–D and
      lands in the README.
- [ ] Record weak spots for the Phase E drills.

### B6 `[CLAUDE]` Close + evidence

- [ ] Evidence into `docs/evidence/phase-b/` (creation wizard sequence, item list
      before/after, metadata DB schema, PowerTable rendering); tick done-criteria,
      Status ✅, session log.
- [ ] 📣 **Portfolio (capture, not publish):** the provisioning walkthrough +
      PowerTable shot are the early-adopter assets — keep them named for Phase E.

## Gotchas & deviations

*(expected suspects: creation wizard differing from docs; PowerTable model-binding
constraints — import-only assumptions; Plan children not Git-syncable; capacity
hiccups on 64 CU during sheet rendering)*

## Session log

- 2026-07-10 — Guide written during repo prep. Nothing built yet.
