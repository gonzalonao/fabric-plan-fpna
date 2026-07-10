# Phase A — Prerequisites & data

**Status:** ⬜ not started
**Days:** D12 (≈ 2026-07-22) · **Plan:** [P3 §Phase A](../fabric-p3-plan-fpna.md) ·
**Requires:** P1/P2 closed or parked cleanly (this project has its own workspace)

## Outcome (done criteria)

- [ ] Workspace `ws-plan-dev` on trial capacity, Git-bound to this repo's `develop`.
- [ ] `lh_plan` holds `dim_account`, `dim_category`, `dim_date`, `fact_actuals`
      (anonymized real data **or** mock — decided within the timebox).
- [ ] **Import-mode** semantic model `sm_plan_actuals` published, relationships +
      base measures in place.
- [ ] XMLA path proven: the model opens in Analyze-in-Excel (or an XMLA client) —
      recorded outcome either way (this is the sprint's planned XMLA probe).

## Decisions (made up front — revisit only with a reason)

| Decision | Choice | Why |
|---|---|---|
| Workspace | New `ws-plan-dev` (trial), Git-bound to this repo `develop`, folder `/fabric` | Same pattern as P1; keeps Plan's auto-created items away from the energy workspace |
| Data source | Steam trading P&L from the Power BI star schema — **timeboxed to half a day**; fallback is `src/generate_mock.py` | Real data tells a better story, but Plan is the point of P3, not data wrangling |
| Anonymization | Accounts renamed `Account A–I`; values optionally rescaled by a fixed secret factor **before** anything lands in the lakehouse | Repo and screenshots are public; the raw extract never leaves the local machine (gitignored `data/`) |
| Fact shape | Wide monthly grain: `(account_key, category_key, month_key, revenue_eur, cogs_eur, fees_eur)`; margin/ROI are **measures**, not columns | Planning tools spread over additive columns; ratios must be computed, never stored |
| Model storage mode | **Import** via Power BI Desktop → publish (not Direct Lake) | Plan + XMLA writeback paths are safest with import (per plan doc); a lakehouse "new semantic model" would be Direct Lake. Decision documented in `docs/model.md` |
| SQL DB budget | This phase creates **zero** SQL databases | The trial caps at 3; Plan's metadata DB (Phase B) and the writeback DB (Phase D) take 2 — nothing else SQL-DB-shaped gets created in this project, ever |

## Steps

### A1 `[YOU]` Learn first (~1 h, timeboxed)

- [ ] What Plan is + the four components (Planning sheets, PowerTable, Intelligence
      sheets, Infobridge): `https://learn.microsoft.com/fabric/iq/plan/overview`
- [ ] Prerequisites page — read fully; it defines this phase:
      `https://learn.microsoft.com/fabric/iq/plan/overview-prerequisites`
      Key facts: tenant needs *Users can create Plan (preview) items*, *Allow XMLA
      endpoints…*, *Embed content in apps*; capacity XMLA = Read/Write (set on Day 0 ✓);
      **plan item creation auto-creates a Fabric SQL database** for metadata.
- [ ] Region check already ✓ (Day 0: Plan preview available in North Europe).

### A1.5 `[CLAUDE]` 🎓 Understanding check — EPM/CPM + XMLA

- [ ] Quiz (`AskUserQuestion`): what EPM/CPM tooling does and where Plan sits vs
      Excel/TM1/Anaplan; why planning on the *same* semantic model as reporting kills
      reconciliation pain; what XMLA is and why Pro/PPU capacities don't qualify;
      import vs Direct Lake for a writeback-adjacent model.
- [ ] Record weak spots for the Phase E drills.

### A2 `[YOU]` Workspace + Git binding

- [ ] `app.fabric.microsoft.com` → **Workspaces → + New workspace** → `ws-plan-dev`,
      License mode **Trial**.
- [ ] GitHub → the existing fine-grained PAT `fabric-git-integration` → **edit** →
      Repository access → add `gonzalonao/fabric-plan-fpna` (keep Contents:
      Read & write). (One PAT, explicitly-listed repos — still least-privilege.)
- [ ] `ws-plan-dev` → Workspace settings → **Git integration** → GitHub →
      repo `gonzalonao/fabric-plan-fpna` · branch `develop` · folder `/fabric` →
      **Connect and sync**.
- [ ] `[CLAUDE]` Verify the sync commit landed on `develop`; check `.gitignore`
      covers `data/` (raw extracts stay local).

### A3 `[CLAUDE]` Mock generator first (the guaranteed fallback)

- [ ] On `feature/mock-data`: write `src/generate_mock.py` per house rules (typed,
      ruff, mypy-strict, Google docstrings, pytest for the shape/invariants):
      Contoso-shaped monthly financials, 9 accounts (A–I) × 4–6 categories ×
      2024-01 → 2026-06, seasonal + trend + noise, wide fact schema from the
      Decisions table; writes Parquet to `data/out/`. PR → merge.
- [ ] This runs **before** the extraction attempt — the schema it emits *is* the
      contract A4 must match, and the fallback is then zero-cost.

### A4 `[YOU]` + `[CLAUDE]` Real-data extraction (timebox: half a day, hard stop)

- [ ] `[YOU]` Open the steam Power BI project (`../../powerbi/projects/portfolio` —
      resolve the exact path when starting) and export monthly actuals: revenue,
      COGS, fees by account × category × month (Analyze-table export / DAX query —
      whatever's fastest).
- [ ] `[CLAUDE]` Normalize to the A3 schema, anonymize (Account A–I mapping kept
      **only** in local gitignored `data/mapping.json`), optionally rescale; emit
      Parquet to `data/out/`.
- [ ] **Timebox check:** if this isn't producing clean Parquet within half a day —
      stop, use A3's mock, note the decision here, move on. Plan is the deliverable.

### A5 `[YOU]` + `[CLAUDE]` Land data in `lh_plan`

- [ ] `[YOU]` `ws-plan-dev` → **+ New item → Lakehouse** → `lh_plan` (plain, no
      schemas — nothing here needs them, and fewer preview features stacked under
      Plan is deliberate).
- [ ] `[YOU]` Upload the four Parquet files to Files (`Files/staging/`); create empty
      notebook `nb_load_actuals`, attach `lh_plan`, commit.
- [ ] `[CLAUDE]` Hybrid flow: write `nb_load_actuals` (Git `.py`, PR → merge) —
      Files → validated Delta tables (`dim_account`, `dim_category`, `dim_date`,
      `fact_actuals`), fail loudly on schema drift.
- [ ] `[YOU]` Update all, run it; verify the four tables under Tables. Commit.

### A6 `[YOU]` Import-mode semantic model in Power BI Desktop

- [ ] Power BI Desktop → **OneLake catalog** → `lh_plan` → connect to the **SQL
      analytics endpoint** → select the 4 tables → **Import** storage mode (this is
      the moment Direct Lake would sneak in — pick Import explicitly).
- [ ] Model: `fact_actuals[account_key]→dim_account`, `[category_key]→dim_category`,
      `[month_key]→dim_date`; mark `dim_date` as date table; hide keys.
- [ ] Base measures: `Revenue = SUM(revenue_eur)`, `COGS`, `Fees`,
      `Margin = [Revenue]-[COGS]-[Fees]`, `Margin % = DIVIDE([Margin],[Revenue])`.
- [ ] **Publish** to `ws-plan-dev` as `sm_plan_actuals`. Commit workspace changes via
      Source control if the model appears there.

### A7 `[YOU]` XMLA probe (planned Day-0 risk-register check)

- [ ] Excel → Insert → PivotTable → From Power BI → `sm_plan_actuals` — pivot on
      Revenue by account/month. Renders = XMLA path works. Screenshot.
- [ ] If blocked: try Tabular Editor against
      `powerbi://api.powerbi.com/v1.0/myorg/ws-plan-dev`. Record the exact error and
      outcome **here and in the master plan's risk register** — Phase D's writeback
      strategy depends on it.

### A8 `[CLAUDE]` Close + evidence

- [ ] Evidence into `docs/evidence/phase-a/` (workspace, tables, model view,
      Analyze-in-Excel pivot); tick done-criteria, Status ✅, session log (record:
      real vs mock decision, XMLA outcome).

## Gotchas & deviations

*(expected suspects: OneLake catalog connector naming in Desktop; import from SQL
endpoint vs "lakehouse" connector confusion; PAT edit propagation delay)*

## Session log

- 2026-07-10 — Guide written during repo prep (P3 planning session). Nothing built yet.
