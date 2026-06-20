# SDD Workflow

This repository uses a four-stage workflow with explicit checkpoints between stages.

## Stage I: Align on the idea and behavior

Goal: discuss the idea until the expected behavior is clear and the agent can be both useful and skillful.

The discussion should produce:

- The problem statement
- The desired behavior
- The constraints and non-goals
- The assumptions that are currently in force
- The open questions that still block implementation
- The initial written decisions in `docs/sdd/project-decisions/stage-1-decisions.md`

Before moving on, confirm that the objective and behavior are understood well enough to write a spec.

## Stage II: Plan incremental phases

Goal: turn the agreed idea into a small sequence of development phases.

Each phase should include:

- A short purpose
- The dependency on earlier phases
- The expected outcome
- The validation approach
- The risks or tradeoffs

Before moving on, confirm that the phase order and scope are acceptable.

## Stage III: Execute phases as small tasks

Goal: implement one phase at a time by defining small, accomplishable tasks.

Each task should be:

- Small enough to finish in one pass
- Testable on its own
- Worth committing on its own
- Explicit about what will change and what will not

For every task:

- Implement the task
- Run the relevant tests for the task
- Commit the change only after the tests pass
- Summarize what was verified
- Update `docs/sdd/project-decisions/phase-status.md` after each completed phase or stage so the work can be restored from the written record at any time
 - Before starting a task, consult `docs/sdd/project-decisions/phase-status.md` to confirm the current stage and phase. If unsure about the current implementation point, stop and resolve the ambiguity using `phase-status.md` before proceeding.
 - After completing a task or phase milestone, update `docs/sdd/project-decisions/phase-status.md` immediately so the repository can be restored to this point.

When implementing a phase from Stage II, create a `phase-<n>-tasks.md` file in `docs/sdd/project-decisions/` that registers the tasks to be solved for that phase. The `phase-<n>-tasks.md` file should contain a short purpose, a checklist of all tasks (so solved vs remaining items are visible at a glance), owners (optional), validation steps for each task, and a `Last updated` line. Name the file using the phase number (for example `phase-2-tasks.md`).

Before moving to the next task or phase, confirm that the result is clear and that the next decision is still aligned with the plan.

## Stage IV: Add new features

Goal: safely add new features to a project that has completed the initial development cycle.

When to use it:

- Enter Stage IV when the project author explicitly instructs the agent to start adding new features.
- Or enter Stage IV when Stages I–III have been completed and the project is stable; treat this as a maintenance/extension phase.

What to do in Stage IV:

- Define the new feature scope and constraints (treat as a mini Stage I for the feature).
- Plan feature implementation as a set of phases or tasks (reuse Stage II/III patterns for feature work).
- Create `phase-<n>-tasks.md` files for feature phases and update `phase-status.md` as you progress.
- Run tests, document any decisions, and keep the `project-decisions` folder up to date so the feature work is restorable.

## Working rule

If anything is ambiguous at a stage boundary, stop and clarify before advancing. Do not silently assume a bigger design decision than the user has agreed to.