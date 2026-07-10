# Phase execution guides

One file per P3 phase (see [fabric-p3-plan-fpna.md](../fabric-p3-plan-fpna.md)).
Each file is the **single source of truth for that phase**: the exact steps to follow,
who does each one, what has been done, and what went wrong. A fresh Claude session
should be able to resume work from these files alone.

Plan (preview) is barely documented in the wild — **every deviation logged here feeds
`docs/gotchas.md`, which is a first-class deliverable of this project.**

## Conventions

- **`[YOU]`** — manual steps Gonzalo performs (Fabric portal, Power BI Desktop, Excel,
  screenshots).
- **`[CLAUDE]`** — steps Claude performs locally (repo files, git, code, docs).
- **📣 Portfolio** — a checkpoint to add/update the portfolio entry at
  `../../portfolio/astro` (`src/content/projects/fabric-plan-fpna.mdx`).
- **🎓 Learning** — a checkpoint where Claude actively checks understanding (quiz,
  diagram, authoritative sources) before the work builds on a new concept.
- Steps are numbered `A1, A2, …` per phase and ordered — do them top to bottom;
  interleaving matters.
- Checkboxes track progress. **Tick them as steps complete** — Claude updates the file
  when told a `[YOU]` step is done, and after finishing its own steps.
- The `Status` line at the top of each file is one of:
  `⬜ not started · 🔄 in progress (at step X) · ✅ done`.
- **Session log** at the bottom of each file: one dated line per working session with
  what was completed and any deviation from the written steps. Deviations also get a
  bullet under *Gotchas & deviations* so the steps stay truthful.
- To resume in a new session, tell Claude:
  *"Read docs/phases/phase-<x>.md — we're at step <n>."*

## Files

| Phase | File | Days | Status |
|---|---|---|---|
| A — Prerequisites & data | [phase-a-prereqs-data.md](phase-a-prereqs-data.md) | D12 | ⬜ |
| B — Plan item & PowerTable | [phase-b-plan-powertable.md](phase-b-plan-powertable.md) | D13 | ⬜ |
| C — Planning sheets & scenarios | [phase-c-planning-scenarios.md](phase-c-planning-scenarios.md) | D13–D14 | ⬜ |
| D — Intelligence, writeback, report | [phase-d-intelligence-writeback.md](phase-d-intelligence-writeback.md) | D14–D15 | ⬜ |
| E — Write-up & evidence | [phase-e-writeup-evidence.md](phase-e-writeup-evidence.md) | D15 | ⬜ |
