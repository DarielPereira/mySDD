---
description: Update Phase status after completing a task or phase during Stage III execution
---

## Maintaining Implementation Status in Stage III

You are updating the execution status as tasks and phases complete. This keeps the team synchronized and enables recovery if work is interrupted.

Use the status template: [docs/sdd/templates/S3-PhasesStatus-template.md](docs/sdd/templates/S3-PhasesStatus-template.md#L1)

The status file is created once when Stage III begins. It is updated after:
- Every completed task
- Every completed phase
- Any major blocker or scope change

## What to Update After Each Task

**1. Mark the task complete in the task list.**
- Change the checkbox from `[ ]` to `[x]`
- Preserve the task name and description

**2. Record what was verified.**
Example update:
```
- [x] Create users table with id, email, password_hash, created_at
  ✓ Verified: Unit tests cover create, read, update operations
  ✓ Verified: Email uniqueness constraint enforced (duplicate rejected with 409 Conflict)
  ✓ Verified: Password hashing uses bcrypt with salt; plaintext never stored
  ✓ Verified: Database migration runs cleanly on empty and populated databases
  ✓ Verified: All existing tests still pass (no regressions)
```

Include:
- What was actually built (be specific, not vague)
- What tests were run and passed
- What edge cases or security concerns were addressed
- Any surprises or learning points

**3. Update the "current active task" section.**
Example:
```
## Current Active Task
**Task 3:** Add email validation to signup endpoint
- Started: <timestamp>
- Estimated completion: 1 day
- Current status: In progress, 50% complete
- Blockers: None
```

**4. Update "next actions" after the active task is complete.**
Example:
```
## Next Actions
1. Task 4: Implement password reset flow (depends on Task 3 completion)
2. Code review of Tasks 1–3 (planned for tomorrow)
3. Deploy to staging after Task 5 complete (gated by integration tests)
```

## What to Update After Each Phase Completes

**1. Mark the phase complete.**
- Move the phase from "In Progress" to "Completed"
- Record completion date and actual effort

**2. Summarize phase outcomes.**
Example:
```
## Phase 2: Authentication Complete (Oct 15–Nov 2)
- **Planned effort:** 2 weeks (10 days)
- **Actual effort:** 11 days (aligned with estimate)
- **Tasks completed:** 8 of 8
- **Major outcomes:**
  - User registration and login endpoints working
  - Password reset flow verified with manual testing
  - Email validation prevents invalid signups
  - All acceptance criteria met
- **Risks realized:** None
- **Unexpected learnings:** CORS headers required extra setup; documented for Phase 3
```

**3. Update the phase status table.**
Example:
```
| Phase | Status | Started | Completed | Notes |
|-------|--------|---------|-----------|-------|
| 1. Core Schema | ✅ Done | Oct 1 | Oct 8 | Delivered on time |
| 2. Authentication | ✅ Done | Oct 8 | Nov 2 | 3-day overrun due to email service issues |
| 3. Feature X | ⏳ In Progress | Nov 2 | - | Started, blocking Task 1 on external API |
| 4. Deployment | ⏳ Planned | Nov 10 | - | Waiting for Phase 3 |
```

**4. Record any blockers or deferred work.**
Example:
```
## Blockers & Deferred Work
- **Blocker resolved:** Email service API rate limits (Phase 2, Task 7)
  - Impact: Delayed testing by 1 day
  - Resolution: Contacted support, increased limit to 100/hour
  - Learning: Document rate limits in setup docs for next project

- **Deferred from Phase 2:** Two-factor authentication
  - Originally planned, descoped to Phase 4 (Stage IV feature)
  - Reason: Not required for MVP; adds complexity that delays Phase 3
  - Revisit in Phase 4 if security requirements change
```

## Recovery Notes: Help Yourself Later

If work must pause (vacation, emergency, context switch), leave clear recovery notes.

Example:
```
## Recovery Notes (Nov 2)
**If resuming Phase 3 after interruption:**
- Last completed task: Task 2 (user profile schema)
- Next task: Task 3 (implement GET /users/:id endpoint)
  - Depends on Task 2 (schema complete, no external blockers)
  - Estimated 1 day
  - Code branch: feature/user-profile
- Current test coverage: 82% (target: >= 80%, met)
- Any pending code reviews? Check PR #45 (Task 2) before proceeding

**Known gotchas:**
- The user_id query parameter must be validated as integer (not string) to avoid SQL issues
- Integration tests require staging database seeded with test users (see `scripts/seed-staging.sql`)
```

## Status Update Discipline

**Update after each task, not after each day of work.** A "day" of work might be 3 tasks.
This keeps status accurate and prevents the file from going stale.

**Be concrete, not vague.**
- ❌ Avoid: "Task 1 done, seems to work"
- ✅ Prefer: "Task 1 complete. Verified: unit tests (15 tests, all pass), integration test (POST /users returns 201), manual smoke test on staging (created user, confirmed in database)"

**Record surprises immediately.**
- If a task took 3x longer than expected, note the reason and update the next task estimate.
- If you discovered a dependency was missing, document it so later tasks avoid the trap.
- If a test failed unexpectedly, record what it taught you.

**Use timestamps sparingly but strategically.**
- Record phase start/completion dates (e.g., Oct 15, Nov 2)
- Record task completion timestamps if tracking effort is important (e.g., 3 days to complete)
- Omit second-level precision; day-level is enough

## Sign-off Before Phase Completion

Before marking a phase complete, confirm:
- [ ] All tasks in the phase are marked complete
- [ ] All specified acceptance criteria from the phase plan are met
- [ ] All tests pass (unit, integration, performance if specified)
- [ ] Code review is done (if required)
- [ ] No blockers or deferred work will silently break the next phase
- [ ] The status file is updated with phase summary

If any confirmation fails, the phase is not complete. Move the incomplete tasks to the next phase or
create a follow-up task.

## Status File as a Changelog

Over time, the status file becomes a changelog of the project's evolution. It documents:
- What was built
- When it was built
- What was learned
- Where work stalled and why
- How problems were resolved

This is valuable for retrospectives, team onboarding, and future project decisions.
