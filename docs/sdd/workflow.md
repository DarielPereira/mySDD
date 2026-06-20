# SDD Workflow

This repository uses a three-stage workflow with explicit checkpoints between stages.

## Stage I: Align on the idea and behavior

Goal: discuss the idea until the expected behavior is clear and the agent can be both useful and skillful.

The discussion should produce:

- The problem statement
- The desired behavior
- The constraints and non-goals
- The assumptions that are currently in force
- The open questions that still block implementation
- The initial written decisions in `docs/sdd/project-decisions/phase-1-decisions.md`

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

Before moving to the next task or phase, confirm that the result is clear and that the next decision is still aligned with the plan.

## Working rule

If anything is ambiguous at a stage boundary, stop and clarify before advancing. Do not silently assume a bigger design decision than the user has agreed to.