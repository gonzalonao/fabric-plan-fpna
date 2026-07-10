# P3 — `fabric-plan-fpna` — Plan (preview) deep dive (D12–D15)

Last updated: 2026-07-10 · Part of [[fabric-portfolio-plan]] · Repo: `fabric-plan-fpna`

**Goal:** the dedicated learning project for **Plan (preview)** — the Fabric IQ EPM/CPM
workload (GA scheduled July 2026, so this is genuine early-adopter evidence). Build a real
FP&A loop over the Steam trading P&L: actuals → PowerTable planning model → budget +
scenarios in Planning sheets → variance in Intelligence sheets → **writeback** to a Fabric
SQL database → plan-vs-actual Power BI report. The write-up of what worked and what broke
is half the portfolio value — preview gotchas ARE the deliverable.

**Minimum shippable (cut line):** PowerTable + one Planning sheet + variance report on
mock data; writeback becomes stretch.

**Positioning note (honest):** Plan's CV payoff targets BI/analytics-engineer and Power
BI-heavy roles plus early-adopter signaling — it is FP&A tooling, not core DE. That's why
it's a satellite, and why the natural pairing on the CV is your "very advanced Power BI"
line.

## Phase A — Prerequisites & data (D12)

- Verify day-0 tenant settings took effect: Plan items enabled, XMLA endpoints on
  (capacity XMLA = Read Write), embed content in apps. Plan item creation auto-creates a
  Fabric SQL database (metadata) — this consumes 1 of the trial's 3 SQL DB slots
  ([[fabric-portfolio-plan]] budget).
- Data: extract monthly actuals from the steam Power BI star schema
  (`../powerbi/projects/portfolio`): revenue, COGS, fees, ROI by account × category ×
  month. **Anonymize** (Account A–I) and optionally rescale values before anything goes
  public. **Timebox: half a day** — fallback is `src/generate_mock.py` producing
  Contoso-shaped monthly financials with the same schema.
- Land as Parquet → small lakehouse → **import-mode** semantic model (safest with XMLA
  writeback paths; document the choice), with `dim_account`, `dim_category`, `dim_date`,
  `fact_actuals`.
- **Done:** semantic model queryable from Analyze-in-Excel (proves XMLA path works before
  Plan touches it).

## Phase B — Plan item & PowerTable model (D13)

- Create the plan item; inventory in `docs/` what got auto-provisioned (SQL DB, item
  children) — nobody has written this up publicly yet.
- PowerTable sheet: dimensional planning model account × category × month bound to the
  semantic model; understand member/hierarchy handling and how it aligns to model dims.
- **Done:** PowerTable renders actuals correctly against the semantic model.

## Phase C — Planning sheets & scenarios (D13–D14)

- Next-quarter budget with explicit assumptions (volume growth %, fee rate, price drift);
  spread top-down targets and adjust bottom-up per account — exercise both workflows.
- Scenarios: base / optimistic / pessimistic; document how Plan stores and switches them.
- **Done:** three scenarios saved and switchable; assumptions documented in the sheet and
  mirrored in `docs/model.md`.

## Phase D — Intelligence, writeback, report (D14–D15)

- Intelligence sheet: automated variance analysis budget-vs-actuals; feed it a fresh
  actuals month and watch variances update.
- **Writeback destination**: persist approved plan data to a Fabric SQL database (second
  and final SQL DB slot; if creation is blocked, reuse the metadata DB and note it).
- Power BI plan-vs-actual report on the shared semantic model — the full loop: actuals in,
  plan out, variance visible.
- **Done:** a number typed in a Planning sheet demonstrably lands in the SQL DB and shows
  up in the report.

## Phase E — Write-up & evidence (D15)

- `docs/gotchas.md`: every preview bug, workaround, and limitation hit (timeboxed 2 h per
  blocker, per the master risk register).
- Evidence pack (screenshots of all four sheet types, writeback proof, 60–90 s demo).
- Wiki note: create [[fabric-plan]] — what Plan is, the four components (Planning sheets,
  PowerTable, Intelligence sheets, Infobridge), architecture, drill answers.
- Draft LinkedIn post: "I built an FP&A loop on Fabric Plan the month it went GA" (publish
  after Done criteria met, per CV rule).

## Interview drills

What EPM/CPM is and where Plan sits vs Excel/TM1/Anaplan/Acterys; why planning on the
*same* semantic model as reporting kills reconciliation pain; writeback architecture
(where plan data physically lives); XMLA's role and why PPU/Pro capacities don't qualify;
top-down vs bottom-up spreading; scenario modeling mechanics; what Infobridge does; how
you'd govern who can edit a plan (workspace roles + item permissions).
