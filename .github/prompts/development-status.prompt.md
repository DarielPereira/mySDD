---
description: Determine the exact stage and phase of development from the project-decisions documentation
---

## Development Status Discovery

Your first job is to determine where the project actually is in the SDD workflow. Do not guess. Inspect the
project-decisions documentation and identify the current stage, active phase, and any unresolved work.

Use the canonical decision artifacts in [docs/sdd/project-decisions/](docs/sdd/project-decisions/) as the source of truth.
If the documentation is stale or contradictory, say so explicitly.

## What to Inspect

Look for the most recent and relevant artifacts in this order:

1. **[docs/sdd/project-decisions/README.md](docs/sdd/project-decisions/README.md)**
   - Confirms the expected file set and the intended workflow artifacts.
2. **Stage I outline**
   - `S1-ProjectOutline.md` or equivalent initial alignment document.
3. **Stage II plans**
   - `S2-PlanPhases.md`, `S2-TechnologyStack.md`, `S2-DataModel.md`
4. **Stage III execution state**
   - `S3-PhasesStatus.md`
   - Active `S3-phase-<n>-tasks.md` file(s)
5. **Stage IV feature work**
   - Active `S4-feature-<name>-tasks.md` file(s)
6. **Chronological markers**
   - `phase-log.md` or any notes that show the latest completed work

## Determine the Exact Status

Report:
- **Current stage**: I, II, III, or IV
- **Current phase**: the exact phase number/name if Stage II or III is active
- **Current feature**: if Stage IV work is active
- **Current task**: the active task, if one exists
- **Last completed item**: the most recent fully completed task or phase
- **Open blockers**: anything preventing forward movement
- **Documentation drift**: any mismatch between documents or stale entries

## Source-of-Truth Rules

Use the strongest evidence available:

- If `S3-PhasesStatus.md` exists and is current, treat it as the primary execution source.
- If `S3-PhasesStatus.md` is missing or stale, infer status from the newest phase task file and any milestone notes.
- If Stage IV feature files exist, determine whether they are active work or deferred ideas.
- If documentation conflicts, prefer the most recently updated canonical file and note the inconsistency.

Do not invent a state just to be helpful. If the evidence is incomplete, say what is known and what is unknown.

## Output Format

Use a concise status report with these sections:

- **Stage**
- **Phase / Feature**
- **Active Task**
- **Completed Work**
- **Blockers**
- **Confidence**
- **Evidence**

Example:

```markdown
## Stage
Stage III

## Phase / Feature
Phase 2: Frontend Integration

## Active Task
Task 3: Connect API client to login flow

## Completed Work
- Phase 1 complete
- Phase 2 tasks 1-2 complete

## Blockers
- Waiting on updated API contract from backend team

## Confidence
High

## Evidence
- S3-PhasesStatus.md updated today
- S3-phase-2-tasks.md lists Task 3 as active
- phase-log.md records Phase 1 completion
```

## Good Practices

- Prefer the latest updated document over memory or inference.
- Cross-check status against at least two sources when possible.
- Call out missing or inconsistent documentation instead of smoothing it over.
- Distinguish between "planned," "in progress," "complete," and "deferred."
- If the project has drifted from the written plan, note the drift rather than hiding it.

## If Status Cannot Be Determined

If the evidence is insufficient:
- State what file or artifact is missing.
- Explain what additional document would resolve the ambiguity.
- Do not continue as if the status were known.
