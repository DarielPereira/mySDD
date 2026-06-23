---
description: Verify a change against the plan and run enough tests to ensure nothing broke
---

## Regression Check

Use this prompt after any change that might have drifted from the approved plan, task list, or documented scope.
Your job is to prove the change is safe or identify exactly what it broke.

When the change depends on external libraries/frameworks/APIs, use Context7 MCP to confirm current
expected behavior before asserting pass/fail against those surfaces.

This prompt is for test execution and plan validation. For human code review and architecture critique,
use [review.prompt.md](.github/prompts/review.prompt.md).

This is not a superficial smoke test. Run as many tests as needed to validate the affected area and nearby surfaces.
Prefer targeted tests first, then expand outward if the change touches shared behavior.

## What to Verify

Check the change against:
- The approved S1/S2/S3/S4 documentation
- The active task or phase definition
- The current implementation and tests
- Any dependency or integration points that could be affected

Look for:
- Behavioral regressions
- Broken tests or missing tests
- Spec drift from the planned work
- Side effects outside the intended scope
- Hidden coupling that could fail later

## Testing Strategy

Use a layered approach:

1. **Targeted tests** for the touched module, feature, or phase
2. **Neighboring tests** for code that integrates with the changed area
3. **Broader tests** for shared behavior or risky dependencies
4. **Build, lint, or type checks** if they are relevant to the repository

Run enough validation to have a defensible answer. If the change is risky, do not stop at a single happy-path test.

## Decision Rule

If the change passes validation:
- State that the implementation is consistent with the plan or explain the accepted deviation.
- Note any residual risk or missing test coverage.

If the change breaks something:
- Say exactly what failed and where.
- Determine whether the breakage is caused by the new change or by pre-existing issues.
- Ask the user whether to:
  - **keep the change and update the documentation/plan**, or
  - **undo the change and restore the previous behavior**

Do not silently choose between those options when the change is outside the defined plan.

## Good Practices

- Prefer objective evidence from tests over intuition.
- Keep the validation scope proportional to the change size and blast radius.
- Add regression tests for bugs you discover so the same problem does not return.
- If a test fails for an unrelated reason, separate that from the current change before deciding what to do.
- Be explicit about whether the plan was followed or intentionally adjusted.

## Output Format

Provide a concise result with these sections:

- **Plan match**
- **Tests run**
- **Result**
- **Breakage found**
- **Recommended next step**

Example:

```markdown
## Plan match
The change matches the approved task except for one documented scope expansion.

## Tests run
- Unit tests for the touched module
- Integration tests for the adjacent service boundary
- Lint/type checks for the repository slice

## Result
All targeted checks passed.

## Breakage found
None.

## Recommended next step
Proceed with the change and update the task notes.
```
