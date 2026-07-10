# Phase D — Intelligence, writeback, report

**Status:** ⬜ not started
**Days:** D14–D15 · **Plan:** [P3 §Phase D](../fabric-p3-plan-fpna.md) ·
**Requires:** Phase C ✅ (three scenarios saved)

## Outcome (done criteria)

- [ ] Intelligence sheet shows automated budget-vs-actuals variance; a **freshly fed
      actuals month** demonstrably updates the variances.
- [ ] **Writeback proven:** a number typed in a Planning sheet lands in a Fabric SQL
      database — query-editor screenshot of the row as the money shot.
- [ ] Power BI plan-vs-actual report closes the loop: actuals in, plan out, variance
      visible.

## Decisions (made up front)

| Decision | Choice | Why |
|---|---|---|
| Writeback destination | New Fabric SQL database `sqldb_plan_writeback` — **slot 2 of 3, the last one this project may use**. If creation is blocked, reuse the Plan metadata DB and record it | Budget from the master plan; the fallback is pre-authorized so a blocker costs minutes, not hours |
| Fresh-month mechanics | Extend the data by one month (`generate_mock.py --extra-month` or one more real-data slice), rerun `nb_load_actuals`, then **manually refresh** `sm_plan_actuals` | Import mode ⇒ variance can't move until the model refreshes — forgetting this *looks* like a Plan bug; it isn't. Worth experiencing deliberately |
| Report source | Report reads actuals from `sm_plan_actuals` and plan rows from the writeback DB (SQL endpoint / DirectQuery composite as needed) | The point is showing plan data living in a queryable DB *outside* the Plan UI; import-copying it back would blur that |
| Report scope | One page: variance matrix (account × month, budget vs actual vs Δ%), scenario slicer if writeback carries scenario labels, 2–3 cards | P1 Phase E was the report showcase; this report exists to prove the loop, not to win design awards |

## Steps

### D1 `[YOU]` Learn first (~45 min, timeboxed)

- [ ] Intelligence sheets + visualizing planning data:
      `https://learn.microsoft.com/fabric/iq/plan/intelligence-how-to-visualize-planning-data`
- [ ] Writeback / persisting plan data — read before touching anything:
      `https://learn.microsoft.com/fabric/iq/plan/planning-writeback/planning-how-to-persist-data`
      (covers *Create a writeback destination*).

### D1.5 `[CLAUDE]` 🎓 Understanding check — writeback architecture

- [ ] Quiz (`AskUserQuestion`): where plan data physically lives at each stage
      (sheet edit → metadata DB? → writeback DB) — draw it; why writeback to a SQL DB
      matters (plan data becomes a governed, queryable asset any tool can join);
      why the variance won't move after loading a new month until the import model
      refreshes; what the composite/DirectQuery trade-off is in the report.
- [ ] Record weak spots for the Phase E drills.

### D2 `[YOU]` Intelligence sheet — automated variance

- [ ] In `plan_fpna`: create an **Intelligence sheet** over budget (base scenario)
      vs actuals; variance and variance-% by account × month.
- [ ] Sanity-check one variance cell by hand (calculator, not vibes). Screenshot.

### D3 `[YOU]` + `[CLAUDE]` Feed a fresh actuals month

- [ ] `[CLAUDE]` Produce the extra month (mock flag or real slice), PR if code
      changed.
- [ ] `[YOU]` Upload to `Files/staging/`, rerun `nb_load_actuals`, then
      `sm_plan_actuals` → **Refresh now**. Wait for success.
- [ ] `[YOU]` Reopen the Intelligence sheet → variances updated for the new month.
      Screenshot before/after pair — this is the "living loop" evidence.

### D4 `[YOU]` Writeback destination + the money shot

- [ ] Create the writeback destination per the docs (D1): SQL DB
      `sqldb_plan_writeback` (**last SQL-DB slot this project touches**). If creation
      fails: reuse the metadata DB, log the error verbatim in `docs/gotchas.md`.
- [ ] Configure the Planning sheet to persist to it; **type a distinctive number**
      into one budget cell (e.g. `123456`) and save/submit.
- [ ] Open the SQL DB → query editor →
      `SELECT * FROM <writeback table> WHERE <value> = 123456` (adjust to the actual
      schema) → **screenshot the row**. This is the single most important capture of
      P3.
- [ ] Document the writeback table schema in `docs/provisioning.md`.

### D5 `[YOU]` Plan-vs-actual Power BI report

- [ ] New report `rpt_plan_vs_actual` in `ws-plan-dev`: actuals from
      `sm_plan_actuals`; plan rows from the writeback DB (its SQL endpoint) —
      composite/DirectQuery as needed (record what worked).
- [ ] Page: variance matrix account × month (budget | actual | Δ | Δ%), cards for
      quarter totals, scenario slicer if the writeback rows carry scenario labels.
- [ ] Verify the loop end-to-end: the `123456` cell from D4 is visible in the report.
      Screenshot.
- [ ] Commit via Source control (`feat(plan): plan-vs-actual report over writeback`).

### D6 `[CLAUDE]` Close + evidence + 📣 capture

- [ ] Evidence into `docs/evidence/phase-d/` (variance sheet, fresh-month pair,
      writeback row, report with the typed number); tick done-criteria, Status ✅,
      session log.
- [ ] 📣 **Portfolio (capture, not publish):** "typed in a sheet → row in SQL →
      visible in the report" as a three-shot sequence — Phase E's narrative spine.

## Gotchas & deviations

*(expected suspects: writeback destination creation blocked on trial → fallback path;
writeback schema undocumented; import-model refresh forgotten (by design in D3 —
observe it); composite model auth to the SQL endpoint)*

## Session log

- 2026-07-10 — Guide written during repo prep. Nothing built yet.
