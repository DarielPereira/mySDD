# Project Decisions

Use this folder as the written source of truth for a specific project.

The goal is to capture the decisions made during Stage I, keep them updated if the idea changes, and preserve a clear record that the planned phases are being followed as agreed.

## Files

- `stage-1-decisions.md`: the initial decisions from Stage I
- `phase-status.md`: the current phase, status, and confirmation checkpoint
- `phase-log.md`: optional running log of stage and phase changes, if the project needs more detail
- `phase-<n>-tasks.md`: checklist of tasks for a given phase (create when executing that phase)
- `docs/sdd/phase-tasks-template.md`: a template to create `phase-<n>-tasks.md` files

## How to use it

1. Fill in `stage-1-decisions.md` before moving out of Stage I.
2. Update the same folder whenever the idea, constraints, or scope changes.
3. Use `phase-status.md` to confirm which stage or phase is currently active.
4. Create a `phase-<n>-tasks.md` file for each phase when you begin work on that phase. Use `docs/sdd/phase-tasks-template.md` as a starting point.
5. Keep the entries short, factual, and aligned with the agreed plan.

## Rule

If a decision changes, update the folder immediately so the written record stays in sync with the real plan.

- **Agent behavior:** Agents must consult `phase-status.md` before starting work when unsure about the current position in the implementation, and must update `phase-status.md` after completing tasks or phase milestones so the record can be used as a restore point.