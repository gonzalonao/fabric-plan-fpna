# Phase E — Write-up & evidence

**Status:** ⬜ not started
**Days:** D15 · **Plan:** [P3 §Phase E](../fabric-p3-plan-fpna.md) ·
**Requires:** Phases A–D ✅ (this phase packages them)

## Outcome (done criteria)

- [ ] `docs/gotchas.md` consolidated — every preview bug, workaround, and limitation,
      each with symptom → cause (if found) → workaround → time lost.
- [ ] Evidence pack complete: all four sheet types, writeback proof, 60–90 s demo.
- [ ] README recruiter-ready; wiki note `fabric-plan.md` created.
- [ ] 📣 Portfolio entry `fabric-plan-fpna.mdx` live (or consciously held); LinkedIn
      post drafted.
- [ ] Interview drills run out loud, gaps noted.

## Decisions (made up front)

| Decision | Choice | Why |
|---|---|---|
| Gotchas format | One entry per issue: *symptom / cause / workaround / time lost / status (open · worked around · fixed by MS)* | Structured beats prose for the "preview gotchas ARE the deliverable" claim |
| Recording script | 60–90 s: PowerTable actuals → Planning sheet edit → scenario switch → Intelligence variance → **the SQL row** → the report | The writeback row is the climax; everything builds to it |
| LinkedIn timing | Draft now, publish only after all Done criteria are met | Master-plan CV rule: no public claims before Done |

## Steps

### E1 `[CLAUDE]` Consolidate the gotchas doc

- [ ] Harvest every *Gotchas & deviations* section from phase files A–D plus
      `docs/provisioning.md` margins into `docs/gotchas.md` (format per Decisions).
      Source phase files stay untouched.
- [ ] End with a short "would I recommend Plan today?" verdict paragraph — hedged,
      dated, honest. Recruiters and searchers land on exactly this.

### E2 `[CLAUDE]` README overhaul

- [ ] Rewrite `README.md` per the portfolio standard (`../../github/CLAUDE.md`):
  - One-sentence description + badges (Fabric, Plan preview, Power BI, Python).
  - Mermaid architecture: actuals (lakehouse → import model) → Plan item
    (PowerTable / Planning / Intelligence + metadata SQL DB) → writeback SQL DB →
    plan-vs-actual report.
  - What it demonstrates: early-adopter Plan evidence + the FP&A loop; **honest
    positioning paragraph** (preview product, trial capacity, what's mock vs real).
  - Links: `docs/provisioning.md`, `docs/model.md`, `docs/gotchas.md`, phase guides,
    demo recording, LinkedIn/GitHub profile.
- [ ] PR → merge; check rendering on the repo front page.

### E3 `[YOU]` Demo recording (60–90 s, silent, captions)

- [ ] Script per Decisions; rehearse once; `Win+Alt+R` or OBS. Capture **now** —
      the workspace dies with the trial.
- [ ] Hand to Claude → `[CLAUDE]` compress (< 10 MB in repo, else GitHub release),
      link from README, commit.

### E4 `[CLAUDE]` Wiki note

- [ ] Create `wiki/learning/fabric/fabric-plan.md`: what Plan is, the four components,
      the architecture as actually observed (provisioning findings), scenario-storage
      findings, gotchas digest, and **written-out answers to the P3 interview drills**
      (EPM/CPM landscape; same-model planning; writeback architecture; XMLA + why
      Pro/PPU don't qualify; top-down vs bottom-up; scenario mechanics; Infobridge;
      plan governance via workspace roles + item permissions).
- [ ] Update the wiki learning index.

### E5 `[YOU]` Interview drills (no notes, out loud)

- [ ] Run the P3 drill list from [the plan](../fabric-p3-plan-fpna.md#interview-drills)
      cold. Stumbled: ______ → re-read E4's note, repeat tomorrow.

### E6 `[CLAUDE]` + `[YOU]` 📣 Portfolio + LinkedIn + CV

- [ ] `[CLAUDE]` Create `../../portfolio/astro/src/content/projects/fabric-plan-fpna.mdx`
      (+ Es mirror; `azure-pipeline.mdx` is the format precedent): the FP&A-loop
      narrative, scenario triptych, the writeback three-shot sequence, honest
      early-adopter framing. Status `completed` only if Done criteria all hold.
- [ ] `[YOU]` Review wording, commit, deploy.
- [ ] `[CLAUDE]` Draft the LinkedIn post ("I built an FP&A loop on Fabric Plan the
      month it went GA") into `docs/linkedin-draft.md` — `[YOU]` publish only
      when Done criteria are met.
- [ ] CV note: P3's line goes into `../../jobsearch/CVs/Master` during the single
      post-D21 realignment — flag it in the realignment checklist rather than editing
      now (unless D21 has passed; then do it here).

### E7 `[CLAUDE]` Final gate

- [ ] Evidence-pack checklist (master plan §Evidence): item definitions committed
      (note which Plan items couldn't sync — that's a finding) ✓ docs ✓ screenshots +
      recording ✓ wiki note ✓.
- [ ] All phase files + phases README set to final status; last session-log entries;
      final commit. **P3 closed — P4 starts from
      `../../fabric-grid-ops-app/docs/phases/`.**

## Gotchas & deviations

*(this phase mostly harvests them — new ones here would be meta-gotchas: recording
size, mdx build breakage)*

## Session log

- 2026-07-10 — Guide written during repo prep. Nothing built yet.
