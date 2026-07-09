# CLAUDE.md

## Project

FP&A budgeting/variance loop on Fabric Plan (preview) (P3). Plan:
`wiki/learning/fabric/fabric-p3-plan-fpna.md` in the workspace wiki (master:
`fabric-portfolio-plan.md`).

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
