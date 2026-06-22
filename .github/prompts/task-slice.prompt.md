---
description: Decompose a phase into independently-executable, testable tasks
---

## Stage III: Execute Phases as Tasks

You are in Stage III. The S2 phase plan exists. Now decompose one phase into the smallest tasks that are:

- **Small.** Completable in a single work session (hours, not days). Small tasks reduce the risk of undiscovered
  blockers and make progress visible.
- **Testable.** Each task has a clear validation step. You can confirm it works without depending on later tasks.
- **Commit-worthy.** The codebase is in a releasable state after each task (backward compatible, no broken
  dependencies, tests pass).
- **Sequentially safe.** Later tasks do not block earlier ones. If Task B depends on Task A, say so explicitly.

## Task Specification

Use the appropriate template:

- **Stage III phases:** [docs/sdd/templates/S3-phase-n-tasks-template.md](docs/sdd/templates/S3-phase-n-tasks-template.md#L1)
- **Stage IV features:** [docs/sdd/templates/S4-feature-name-tasks-template.md](docs/sdd/templates/S4-feature-name-tasks-template.md#L1)

For each task, specify:

**What changes.** Be explicit. Examples:
- "Add `user_id` column to the `posts` table with NOT NULL constraint after backfill."
- "Implement `POST /api/v1/users` endpoint with request validation and error handling."
- "Refactor `AuthService.validate()` to use async/await."

Avoid: "Implement user authentication." (Too broad.)

**What does NOT change.** Explicit scope bounds. Examples:
- "Does not change the public API contract."
- "Does not add caching (deferred to Phase 3)."
- "Does not modify the database schema beyond the specified migration."

This prevents scope creep and manages expectations.

**How it will be tested.** Specify the exact validation:
- Unit tests (with examples of test cases)
- Integration tests (environment, dependencies)
- Manual testing steps (if applicable)
- Performance checks (if applicable)

Examples:
- "Unit tests cover valid/invalid email formats, missing fields, and SQL injection attempts."
- "Integration test: create a user via API, verify database record, confirm password hash is not exposing plaintext."

**What tests must pass before committing.** Concrete conditions. Examples:
- "All unit tests pass: `npm test`"
- "Linter passes: `npm run lint`"
- "Integration tests pass against test database."
- "Code coverage for this module >= 80%."

**What could block the next task.** Identify dependencies and risks. Examples:
- "Depends on Task 1: database migration must be applied."
- "Depends on: API key provisioning for third-party service (external dependency, may delay)."
- "Risk: if Task 2 fails, Task 3 will need rework (cascade risk)."

## Maintaining Status

For Stage III, track progress using the status template:
[docs/sdd/templates/S3-PhasesStatus-template.md](docs/sdd/templates/S3-PhasesStatus-template.md#L1)

Update after completing each task:
- Mark the task completed
- Note what was verified
- Update the "next task" section
- Record any blockers or unexpected findings

## Task Ordering

Order tasks so that:

- Tasks with no dependencies come first.
- If Task A blocks Task B, Task A comes first.
- Tests can be written and run independently (no waiting for later tasks).
- Rollback is simple (if needed, changes can be reverted without breaking dependencies).

## Scope Control

If the phase still feels too large:

- Split it further (combine with earlier phases if the phase is now too small).
- Create a separate "deferred" section documenting what is intentionally left out and why.

Do NOT expand scope mid-phase. Rework belongs in a separate phase or Stage IV feature.

## Good Practices

**Fail fast.** Catch integration issues early by testing across module/service boundaries as soon as possible.
**SOLID principles.** Each task should have a single responsibility. If a task touches 5 modules, it is too broad.
**Clear contracts.** If a task depends on a module or API, document the contract (inputs, outputs, error conditions).
**Minimal viable implementation.** Implement only what is necessary for the phase goal. Perfection belongs in
later maintenance or Stage IV enhancements.