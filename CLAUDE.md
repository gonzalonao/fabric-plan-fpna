# CLAUDE.md

## Project

FP&A budgeting/variance loop on Fabric Plan (preview) (P3). Plan:
`docs/fabric-p3-plan-fpna.md` (master: `fabric-portfolio-plan.md` in the
workspace wiki).

**Execution state lives in `docs/phases/`** — one step-by-step guide per phase with
`[YOU]`/`[CLAUDE]` roles, checkboxes, and a session log. At session start, read the
active phase file; keep its checkboxes, status line, and session log updated as work
progresses (conventions in `docs/phases/README.md`).

**Instructions to Gonzalo are always given in great detail** — exact portal paths,
button names, values to type, and how to verify the result, in the style of the
`[YOU]` steps in `docs/phases/`. This applies to ad-hoc guidance too, not just the
phase guides.

## Branching — develop-flow

- `feature/*` branches off `develop`; PRs merge into `develop`.
- `develop` → `main` by PR only — `main` is the review gate; wait for explicit
  approval before merging.
- Fabric Git integration binds the dev workspace to `develop`.
- Conventional Commits (`type(scope): summary`); no AI attribution in commits or PRs.

## Rules

- Python follows the workspace standard (`.claude/rules/python.md`): ruff
  format/lint, `mypy --strict`, Google-style docstrings.
- SQL database budget: max 3 SQL DBs on trial capacity — the Plan metadata DB
  (auto-created) and the writeback DB use 2 of them. Reuse the metadata DB as
  writeback target if creation fails; create nothing else SQL-DB-shaped.
- Never commit secrets or data — `.env`, tokens and datasets are gitignored.
- Evidence pack as you go: item definitions as code, `docs/` notes (gotchas doc is a
  first-class deliverable), screenshots and a short recording. The trial workspace is
  ephemeral; the repo is the durable artifact.

## Standing objectives — portfolio, documentation, learning

These three run alongside every phase. Treat them as first-class deliverables, not
afterthoughts, and act on the checkpoints seeded in the phase guides.

1. **Portfolio.** This project is portfolio evidence. The portfolio site lives at
   `../../portfolio/astro` (Astro — English entries in `src/content/projects/*.mdx`,
   Spanish mirror in `src/content/projectsEs/`; `azure-pipeline.mdx` is the closest
   format precedent). At the **📣 Portfolio** checkpoints in the phase guides, capture
   the flagged assets as they happen (Phases B–D) and create/update the entry
   (`fabric-plan-fpna.mdx`) at the **Phase E** checkpoint. Proactively flag
   portfolio-worthy moments even between checkpoints.
2. **Reproducible documentation.** Keep `docs/phases/` the truthful, step-by-step build
   journal (conventions in `docs/phases/README.md`): tick checkboxes, keep the status
   line and session log current, and record every deviation — the standard is that a
   cold reader could reproduce the build and that it presents as structured work. For
   this project specifically, `docs/gotchas.md` and `docs/provisioning.md` are headline
   deliverables — preview findings ARE the portfolio value.
3. **Learning.** A second purpose is Fabric fluency for Gonzalo's job. At the
   **🎓 Learning** checkpoints (after each "Learn first" block), actively verify
   understanding before building on a concept: quiz with `AskUserQuestion`, draw
   architecture/concept diagrams (`show_widget` / Mermaid), and pull authoritative
   sources with WebSearch. Don't just build it — make sure he can explain it cold; the
   P3 plan's interview drills are the bar.

**Keep adjacent artifacts current** (reminders live at phase ends): the workspace wiki
(`../../wiki` — `fabric-plan` learning note at Phase E) and the master CV
(`../../jobsearch/CVs/Master`) once the project has a presentable result (single
realignment after D21 per the master plan).
