# SDD Workflow

This repository uses a four-stage workflow with explicit checkpoints between stages.

## Stage I: Align on the idea and behavior

Goal: reach a concise, shared understanding of what to build, why it matters, and how we will know it's successful.

Use the S1 project outline template to capture this alignment: [docs/sdd/templates/S1-ProjectOutline-template.md](docs/sdd/templates/S1-ProjectOutline-template.md#L1).

The alignment should produce a short Project header and written answers for:

- Problem statement (core idea)
- Expected behavior (observable outcomes and success criteria)
- Constraints (technical, legal, time, resource)
- Non-goals (explicit exclusions to avoid scope creep)
- Assumptions that must hold for the plan to be valid
- Open questions that block implementation

When the template is complete and the agreement checkpoint is checked, save a copy into the project's decisions folder so it becomes the canonical source-of-truth (for example, `docs/sdd/project-decisions/S1-ProjectOutline.md`).

Before moving on, confirm the objective and behavior are clear enough to write a spec.

## Stage II: Plan incremental phases

Goal: break the agreed idea into a small sequence of incremental phases that can be delivered, validated, and rolled back independently where appropriate.

Use the Stage II templates to capture the full plan of phases, the selected technology stack, and the project data model:
[docs/sdd/templates/S2-PlanPhases-template.md](docs/sdd/templates/S2-PlanPhases-template.md#L1),
[docs/sdd/templates/S2-TechnologyStack-template.md](docs/sdd/templates/S2-TechnologyStack-template.md#L1),
and [docs/sdd/templates/S2-DataModel-template.md](docs/sdd/templates/S2-DataModel-template.md#L1).

Select the technical stack and the data model while defining the phase plan so the chosen
technologies, data structures, dependencies, and the implementation sequence stay aligned.

The S2 plan should list every planned phase and, for each phase, include the fields:

- **Short purpose:** one sentence describing the phase's intent.
- **Dependencies:** explicit prior phases, PRs, infra or team dependencies (include branch or ticket ids).
- **Expected outcome:** concrete deliverables and acceptance criteria (include measurable metrics).
- **Validation approach:** exact validation steps, tests to run, and rollout/rollback criteria.
- **Risks and tradeoffs:** known risks, mitigations, and deferred items.

Save the reviewed S2 plan as `docs/sdd/project-decisions/S2-PlanPhases.md` so it becomes the canonical project plan. During execution, teams may create per-phase `S3-phase-<n>-tasks.md` files in `docs/sdd/project-decisions/` or tracker tickets that expand tasks into implementable items — but the S2 plan remains the single source-of-truth for phase order, dependencies, acceptance criteria, and validation steps.

Also save the reviewed stack document as `docs/sdd/project-decisions/S2-TechnologyStack.md` so it becomes the canonical stack register for the project. Any later change that affects the technology stack must be recorded there together with the decision to change, the motivation, the alternatives considered, and the impact on phases, tasks, or schedule.

Also save the reviewed data model document as `docs/sdd/project-decisions/S2-DataModel.md` so it becomes the canonical data model register for the project. Any later change that affects the data model must be recorded there together with the decision to change, the motivation, the alternatives considered, and the impact on phases, tasks, or schedule.

Before moving on, confirm that the phase order, dependencies, and scope are acceptable and that the S2 file contains the acceptance criteria and validation steps needed to verify success.

## Stage III: Execute phases as small tasks

Goal: implement one phase at a time by defining small, accomplishable tasks.

When implementing each phase from Stage II, create a `S3-phase-<n>-tasks.md` file in
`docs/sdd/project-decisions/` from [docs/sdd/templates/S3-phase-n-tasks-template.md](docs/sdd/templates/S3-phase-n-tasks-template.md#L1). The file should contain the phase purpose, a checklist of small and testable tasks, owners, validation steps for each task, risks or follow-ups, and a `Last updated` line. Use one file per phase so the implementation can be tracked and restored in detail.

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
- At Stage III entry, create `docs/sdd/project-decisions/S3-PhasesStatus.md` from
	[docs/sdd/templates/S3-PhasesStatus-template.md](docs/sdd/templates/S3-PhasesStatus-template.md#L1).
- Before starting a task, consult `docs/sdd/project-decisions/S3-PhasesStatus.md` to confirm the current stage, phase, and active task. If unsure about the current implementation point, stop and resolve the ambiguity using that file before proceeding.
- After completing each task, update `docs/sdd/project-decisions/S3-PhasesStatus.md` immediately.
- After completing each phase, update `docs/sdd/project-decisions/S3-PhasesStatus.md` to reflect the verified phase status and the next phase/task to start.

Before moving to the next task or phase, confirm that the result is clear and that the next decision is still aligned with the plan.

## Stage IV: Add new features

Goal: safely add new features to a project that has completed the initial development cycle.

When to use it:

- Enter Stage IV when the project author explicitly instructs the agent to start adding new features.
- Or enter Stage IV when Stages I–III have been completed and the project is stable; treat this as a maintenance/extension phase.

What to do in Stage IV:

- Define the new feature scope and constraints (treat as a mini Stage I for the feature).
- Plan feature implementation as a set of feature tasks (reuse Stage III patterns for feature work)
	and document them with [docs/sdd/templates/S4-feature-name-tasks-template.md](docs/sdd/templates/S4-feature-name-tasks-template.md#L1).
- Create `S4-feature-<name>-tasks.md` files in `docs/sdd/project-decisions/` for each introduced
	feature, using the Stage IV template as the source format.
- Run tests, document any decisions, and keep the `project-decisions` folder up to date so the
	feature work is restorable.
## Working rule

If anything is ambiguous at a stage boundary, stop and clarify before advancing. Do not silently assume a bigger design decision than the user has agreed to.