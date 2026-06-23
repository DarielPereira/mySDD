# Project Decisions

This folder stores decisions and artifacts produced while using the SDD workflow.

In the template repository, this folder is a starting point rather than a completed project record. The stage files are expected to be added by the project that uses the template.

Files here are intended to be authored and maintained by project contributors as they work through the SDD stages. Keep user-generated decision documents in this folder so the project's history, phase progress, and feature decisions can be restored from the repository.

Recommended files:

- `S1-ProjectOutline.md` — initial alignment, expected behavior, constraints, non-goals, assumptions
- `S2-PlanPhases.md` — the project-wide phase plan for Stage II
- `S2-TechnologyStack.md` — canonical technology stack register and change history
- `S2-DataModel.md` — canonical data model register and change history
- `S3-phase-<n>-tasks.md` — tasks for a specific phase (create one file per phase during execution)
- `S3-PhasesStatus.md` — status snapshot created and updated during Stage III
- `S4-feature-<name>-tasks.md` — feature task file for Stage IV work
- `phase-log.md` — (optional) chronological log of completed milestones

Recommended relationships:

- `S1-ProjectOutline.md` defines the problem and constraints that Stage II must satisfy.
- `S2-PlanPhases.md` defines the phase order, acceptance criteria, and validation approach.
- `S2-TechnologyStack.md` and `S2-DataModel.md` define the architecture that the phase plan depends on.
- `S3-phase-<n>-tasks.md` expands one Stage II phase into executable tasks.
- `S3-PhasesStatus.md` tracks the live implementation state during Stage III.
- `S4-feature-<name>-tasks.md` captures feature-specific scope once the initial project is stable.
- Later changes to the stack or data model are recorded in the `changes` sections of `S2-TechnologyStack.md` and `S2-DataModel.md`.

Do not delete this folder; it is used to record and restore important project decisions.

Last updated: 2026-06-22
