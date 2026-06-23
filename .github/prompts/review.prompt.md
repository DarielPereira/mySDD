---
description: Critically evaluate a change against SDD principles, spec, and best practices
---

## Code Review Through an SDD Lens

Review the change with three goals:
1. **Correctness.** Does it do what it claims? Are there bugs?
2. **Alignment.** Does it match the spec, phase plan, and decision record?
3. **Quality.** Is it maintainable, testable, and safe?

If the change relies on external libraries/frameworks/APIs, check current documentation via
Context7 MCP before concluding correctness or recommending architecture adjustments.

This prompt is for human-style review and design critique. For test-driven regression validation, use
[regression-check.prompt.md](.github/prompts/regression-check.prompt.md).

## Correctness: Core Questions

**Behavioral regressions?** Run the full test suite. Do any previously-passing tests fail? If yes, investigate:
- Is the failure expected (the change intentionally breaks something)?
- If unexpected, this is a blocker—the change introduces a bug.

**Logic errors?** Trace through the code:
- Do boundary cases (empty input, null values, max values) work correctly?
- Does error handling work (try/catch, validation, missing dependencies)?
- Are there off-by-one errors, race conditions, or resource leaks?

**Missing validation?** If the change accepts user input or external data:
- Is the input validated? (type, length, format, range)
- Are error messages clear (helps debugging)?
- Can invalid input break the system or expose data?

Example: If a new endpoint accepts a user ID, confirm:
- The ID is validated (is it a valid integer? does the user exist?)
- Responses differ for valid vs. invalid IDs (no information leakage)
- The endpoint returns appropriate HTTP status codes (400 for bad input, 404 for not found)

## Alignment: Spec and Plan Adherence

**Does the change match the task spec?**
- If the task says "Add email validation," does the code only validate emails (no extra features)?
- If the task says "Preserve backward compatibility," does the change break any existing APIs or data contracts?
- If the task lists "What does NOT change," confirm those areas were not touched.

**Does the change reflect the phase plan?**
- If the phase defers caching to Phase 3, the code should not add caching now.
- If the phase specifies acceptance criteria (e.g., "load 10k records in <200ms"), verify the code meets it.

**Does it respect technology and data model decisions?**
- If the stack specifies SQLite (not Postgres), the code should not assume Postgres features.
- If the data model specifies "no denormalization in Phase 1," the code should not denormalize data.

**Stage-specific checks:**

- **Stage III:** Is the status file updated? Do task completion notes document what was verified?
- **Stage IV:** Does the feature task file exist and match the implementation? Are feature-specific decisions recorded?

## Quality and Maintainability

**Overengineering?**
- Is the code more complex than needed? (KISS: Keep It Simple)
- Does it solve a problem the task does not ask for? (YAGNI: You Aren't Gonna Need It)
- Example: If the task is "add a user name field," implementing a full user profile system is overengineering.

**Code clarity?**
- Is the code readable? Would a new team member understand it without asking?
- Are variables named clearly? (Avoid: `x`, `temp`, `data1`)
- Are comments present where logic is non-obvious?

**Testing?**
- Are new tests present for new behavior?
- Do tests cover happy paths AND edge cases? (valid input, boundary values, error cases)
- Is test coverage sufficient for the changed code (typically >= 80%)?
- Do tests run in isolation (no hidden dependencies on execution order)?

**Dependencies?**
- Are new dependencies justified? (Does the library reduce complexity or risk?)
- Are dependencies pinned to specific versions (to avoid unexpected breakage)?

## Risk Assessment

**Breaking changes?**
- If the change modifies a public API, data schema, or contract, is it intentional?
- Is the breaking change documented and communicated to users/teams?
- Is there a migration path for existing data or clients?

**Security and data safety?**
- Does the change expose credentials, API keys, or sensitive data?
- Are SQL queries parameterized (preventing injection attacks)?
- Are user permissions checked (not everyone should access all data)?
- Is data validated before storage (garbage in, garbage out)?

**Rollback safety?**
- If this change is deployed and breaks production, can it be rolled back safely?
- If the change includes a database migration, can it be reversed?
- Are there dependent changes that must be coordinated?

## Feedback Style

**Be specific.** Instead of "this is confusing," say: "This loop variable `i` is reused in nested loops, which could
cause off-by-one errors. Suggest renaming to `row_idx` and `col_idx`."

**Distinguish critical from nice-to-have:**
- **Blockers** (must fix): Bugs, spec mismatches, regressions, security issues.
- **Strongly suggested** (should fix): Code clarity, maintainability, missing tests.
- **Nice-to-have** (consider): Minor style preferences, future optimizations.

**Approve or request changes clearly.** Examples:
- "Approved: all tests pass, spec is met, no regressions."
- "Request changes: the error handling for invalid user IDs is missing. Once fixed, re-request review."
- "Comment: consider extracting the validation logic into a helper function for reuse. Not required for this task."