---
description: Compare implementation to project documentation and update the documentation to match reality
---

## Documentation Synchronization

Your job is to bring the project documentation back in sync with the codebase and current implementation.
Use this when the code has moved ahead of the docs, the docs have drifted, or a change needs to be reflected in the project-decisions artifacts.

The source of truth is the actual implementation. The documentation should describe what exists, what was decided, and what is intentionally deferred.

## What to Compare

Inspect both the implementation and the decision documents:

- Current code and tests
- [docs/sdd/project-decisions/README.md](docs/sdd/project-decisions/README.md)
- `S1-ProjectOutline.md`
- `S2-PlanPhases.md`
- `S2-TechnologyStack.md`
- `S2-DataModel.md`
- `S3-PhasesStatus.md`
- Active `S3-phase-<n>-tasks.md` files
- Active `S4-feature-<name>-tasks.md` files
- `phase-log.md` or any other progress notes

## What to Look For

**Implementation drift**
- Code exists for a feature or behavior not recorded in the plan
- A phase or task is marked incomplete even though implementation is done
- The documented stack or data model no longer matches the code

**Documentation drift**
- A task is marked complete without the expected implementation
- A phase plan still lists work that was already deleted or replaced
- Status files show old blockers that are no longer relevant
- Decision documents mention technology or data model choices that changed later

**Intentional deviation**
- Some code was implemented outside the original plan, but the team chose to keep it
- A scope change was accepted and should now be reflected in the documentation
- A temporary workaround became the current design and needs to be documented as such

## Update Rules

When the implementation and documentation differ:

1. **Document the reality first.** Update the relevant project-decisions file so it reflects what actually exists.
2. **Record the reason.** Explain why the difference happened.
3. **Preserve history.** Do not erase the original decision unless it was clearly superseded.
4. **Mark deferred or abandoned work clearly.** Do not leave ambiguous half-done items behind.
5. **Keep the wording factual.** Avoid justifying the change with vague language like "improved" or "cleaned up."

## What to Update

Choose the smallest set of files that keeps the project history accurate:

- **Stage I drift**: update the project outline if the real problem statement changed
- **Stage II drift**: update the phase plan, stack, or data model if the implementation forced a different direction
- **Stage III drift**: update `S3-PhasesStatus.md` and the active phase task file
- **Stage IV drift**: update the active feature task file and any related decisions
- **Milestone drift**: update `phase-log.md` or the equivalent execution history note if present

## Best Practices

- Make the documentation match the code, not the other way around, unless the team explicitly chooses to revert code.
- Use the change-log sections in the stack and data model documents when architecture changed.
- If code was changed outside the plan, note whether it should remain or be reverted.
- Keep historical context intact so future contributors can understand why the divergence happened.
- Prefer a narrow doc update over a broad rewrite unless several artifacts are now inconsistent.

## Output Format

Provide a concise sync report:

- **What changed in the implementation**
- **What documentation was updated**
- **What drift remains, if any**
- **Whether the docs now match reality**
- **Any follow-up needed**

## If You Find a Problem

If the docs reveal that the implementation diverged in an unsafe or unintended way:
- Say exactly what differs.
- State whether the change should be kept or reverted.
- If the difference is still acceptable, update the docs to reflect it.
- If it is not acceptable, flag it for correction instead of normalizing it.
