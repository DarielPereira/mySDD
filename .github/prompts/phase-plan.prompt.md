---
description: Break the agreed idea into a sequence of incremental, independently-deliverable phases
---

## Stage II: Plan Phases and Architecture

You are in Stage II. The S1 project outline is complete. Now break the work into phases and align the technology stack and data model so they support the phase sequence and can be evolved without disruption.

Deliver three interconnected documents:

1. **[docs/sdd/templates/S2-PlanPhases-template.md](docs/sdd/templates/S2-PlanPhases-template.md#L1)** — The phase plan
2. **[docs/sdd/templates/S2-TechnologyStack-template.md](docs/sdd/templates/S2-TechnologyStack-template.md#L1)** — Technology choices and change tracking
3. **[docs/sdd/templates/S2-DataModel-template.md](docs/sdd/templates/S2-DataModel-template.md#L1)** — Data structure and persistence strategy

## For Each Phase, Specify

**Name and purpose.** A one-sentence statement of what the phase delivers and why it matters in the overall sequence.
Good: "Phase 1: Core data model and persistence layer, enabling subsequent feature work." Avoid: "Phase 1: Backend."

**Explicit dependencies.** What must be true before this phase can start? List:
- Prior phases (e.g., "requires Phase 1 completion and merged to main")
- Infrastructure or approvals (e.g., "S3 bucket provisioned, AWS credentials in CI/CD")
- Team or third-party dependencies (e.g., "Design review from Product")
- External constraints (e.g., "after platform version X is released")

Use concrete references (PR numbers, ticket IDs, commit hashes) instead of vague terms like "prior work."

**Measurable outcome and acceptance criteria.** What does "done" look like? Include:
- Deliverables (e.g., API endpoints, database schema, UI components)
- Metrics or examples (e.g., "load API with 10k records in <200ms")
- Tests that must pass (e.g., "integration tests cover all CRUD operations")

Acceptance criteria remove ambiguity about when a phase is truly complete.

**Validation approach.** How will you verify the outcome? Specify:
- Automated tests (unit, integration, e2e)
- Manual testing steps
- Performance benchmarks or load tests
- Staged rollout criteria (if applicable)

Validation is not deferred to later—it is part of the phase.

**Risks and tradeoffs.** What could go wrong? What are you deferring or sacrificing?
Examples: "We defer caching to Phase 3 (risk: slower reads early); we trade off perfect normalization for simplicity (risk: some redundant storage)."

## Technology Stack and Data Model

The phase plan must be compatible with the canonical Stage II architecture records:

- [docs/sdd/templates/S2-TechnologyStack-template.md](docs/sdd/templates/S2-TechnologyStack-template.md#L1)
- [docs/sdd/templates/S2-DataModel-template.md](docs/sdd/templates/S2-DataModel-template.md#L1)

Use [S2-decisions.prompt.md](.github/prompts/S2-decisions.prompt.md) when you need to write or revise the initial technology stack or data model decisions.
This prompt only checks that the chosen stack and model support the phase sequence, dependencies, and validation.
If the stack or data model is still undecided, stop and resolve that before finalizing the phase plan.

## Ordering Phases for Safety

Phases should be ordered so that:

- Early phases deliver measurable value or learning.
- Dependencies flow forward (Phase 2 depends on Phase 1, not vice versa).
- No phase is blocked waiting for external work.
- Later phases can use or build on earlier outcomes.

**Separate concerns:** If two features are independent, consider separate phases (even if sequential) rather than combining them. This enables parallel validation and reduces rework.

If you need a deeper dependency matrix or parallel-execution analysis, use
[dependency-analysis.prompt.md](.github/prompts/dependency-analysis.prompt.md).

## Before Finalizing

- Do all phases fit within reasonable time and resource constraints?
- Is there a clear rollback strategy if a phase fails partway through?
- Are there any circular dependencies (Phase A depends on Phase B, Phase B on Phase A)? If yes, combine or reorder.
- Can any phase be tested or validated in isolation?
- Are there assumptions or external dependencies that could strand work in progress?

If anything is unclear, stop and clarify with the team.