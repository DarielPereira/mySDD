

# S2 — Plan Phases (Template)

This template documents the full, planned sequence of incremental phases for the project. Use a single S2 file to list and describe all phases that will be addressed during implementation. The completed S2 plan becomes the project's canonical Phase Plan and should be saved to `docs/sdd/project-decisions/S2-PlanPhases.md` after review.

Purpose: capture the ordered set of phases, their dependencies, acceptance criteria, validation
approach, and risks so the project can be executed, validated, and restored from the repository.

## Overview

- Project (from S1):
- Owner(s):
- Date:
- Short summary of the planned phases and their goals.

## Phases

For each planned phase include a dedicated subsection with the following fields. Repeat the
subsection for `Phase 1`, `Phase 2`, etc.

### Phase <n> — Title

- Phase number: <n>
- Title:
- Owner(s):
- Date:

#### Short purpose

One clear sentence describing the phase's intent.

#### Dependencies

- Explicit dependencies on earlier phases, merged PRs, design approvals, infrastructure, or
  external teams. Include artifact names, branch names, issue/ticket IDs, or other concrete refs.

#### Expected outcome

- Concrete deliverables and measurable success criteria (acceptance tests, API contracts, UX
  flows, performance targets). Include example scenarios or measurable metrics.

#### Validation approach

- Validation methods (automated tests, manual QA checklist, e2e scenarios, benchmarks, rollout steps).
- Exact validation steps, commands, or test names so reviewers can reproduce validation locally.
- Rollout criteria and rollback conditions (if applicable).

#### Risks and tradeoffs

- Known risks with mitigations and any tradeoffs (scope, timeline, technical compromises).

#### Tasks (summary checklist)

- [ ] Task A — short description — Owner: — Verification: (how to confirm)
- [ ] Task B — short description — Owner: — Verification:

Keep this checklist as a high-level summary for the phase. Detailed, implementable tasks should
be created later as Stage III `S3-phase-<n>-tasks.md` files or as tickets in your tracker.

#### Estimated effort

- Estimated time or story points (optional):

#### Notes / Links

- Design notes, diagrams, links to relevant code, tickets, or documents.

---

## Saving the finalized S2 plan

After the S2 plan is reviewed and agreed, save it as `docs/sdd/project-decisions/S2-PlanPhases.md`.

## Last updated

Last updated: YYYY-MM-DD
