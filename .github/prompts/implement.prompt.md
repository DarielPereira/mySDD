---
description: Execute a single, scoped task with clarity and safety
---

## Implementation Discipline

You are implementing one task within a larger phase or feature. Your job is to deliver working code that:
- Matches the task specification (no scope creep)
- Passes all specified tests
- Does not break existing behavior
- Is easy to understand and review
- Can be rolled back safely if needed

This prompt is for single-task implementation. For phase-level validation and deployment planning, use
[validate-and-deploy.prompt.md](.github/prompts/validate-and-deploy.prompt.md).

## Core Rules

**1. Follow the spec.** The task description defines the scope. If you discover a requirement that was not in
the spec, stop and clarify instead of implementing extra work.

**2. Make the smallest change that works.** Avoid refactors, optimizations, or "nice-to-haves" unrelated to
the task. Refactoring belongs in a separate task or stage.

**3. Do not change behavior unless the task requires it.** Preserve backward compatibility. If the task calls
for breaking changes, document the impact and ensure the breaking change is intentional and communicated.

**4. Verify with tests FIRST.** Before writing implementation code:
- Write or update failing tests that cover the task requirement.
- Understand what "passing" means (exact conditions, boundary cases).
- Run tests to confirm they fail (so you know they will catch the bug if you get it wrong).

**5. Document tradeoffs.** If you deferred work, made a simplification, or took a risk, write it down in
code comments or a commit message. Future maintainers will thank you.

## Step-by-Step Workflow

1. **Read the task spec carefully.** Confirm:
   - What changes?
   - What does NOT change?
   - What tests must pass?
   - What could block the next task?

2. **Understand the test expectations.** Run the specified tests and confirm they fail (or do not yet exist).
   If tests do not exist, write them based on the task description.

3. **Implement the minimal solution.**
   - Make the failing test pass.
   - Do not overengineer.
   - If you find yourself implementing "X for the future," stop—defer it to another task.

4. **Run ALL tests** (not just the new ones) to confirm no regressions.
   - Unit tests
   - Integration tests
   - Linter / code style checks
   - Any performance benchmarks specified

5. **Before committing, verify:**
   - All specified tests pass.
   - No previously-passing tests broke.
   - The code is readable (future maintainers will work on this).
   - You can explain the change in 2-3 sentences.

6. **Write a clear commit message.** Example:
   ```
   Add email validation to User signup endpoint

   - Validate email format with regex
   - Return 400 Bad Request for invalid emails
   - Tests cover valid emails, invalid formats, and empty input
   - Backward compatible: existing valid users unaffected
   ```

## Tracking Progress

For **Stage III phases:** Update the active status file immediately after completing the task:
[docs/sdd/templates/S3-PhasesStatus-template.md](docs/sdd/templates/S3-PhasesStatus-template.md#L1)

For **Stage IV features:** Update the feature task file:
[docs/sdd/templates/S4-feature-name-tasks-template.md](docs/sdd/templates/S4-feature-name-tasks-template.md#L1)

Note:
- Task completion time (if tracking effort)
- Any blockers or unexpected findings
- What the next task should be

## Risk Management

**If the task feels too large,** break it into smaller subtasks instead of expanding scope. Ask: "Can I
deliver half of this in isolation and test it independently?"

**If you find a bug outside the task scope,** document it (ticket or comment) but do NOT fix it in this
task unless the bug breaks the task itself. Keep changes focused.

**If you need to refactor code to implement the task,** do the refactor first (in a separate commit or task)
, then implement the feature on top of clean code. This separates concerns and makes review easier.

## Good Practices

- **Test behavior, not implementation.** Tests should verify what the code does, not how it does it. This
  allows refactoring without breaking tests.
- **Explicit error handling.** If the task touches user input or I/O, handle errors explicitly. Do not silently
  fail.
- **Clear variable names.** Future you (and your teammates) will thank you.
- **Minimal dependencies.** If the task requires a new library, justify it. (Does it truly reduce complexity
  or risk?)
- **Commit frequently.** One commit per logical unit of work. This makes code review and rollback easier.