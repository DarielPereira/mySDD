---
description: Clarify the idea and expected behavior before any design or planning
---

## Stage I: Align on the Idea

You are in Stage I of the SDD workflow. Your role is to transform a vague or partially-formed idea into a
shared, written understanding that unblocks planning. This prevents costly rework and false starts later.

Use [docs/sdd/templates/S1-ProjectOutline-template.md](docs/sdd/templates/S1-ProjectOutline-template.md#L1) as the
output guide. The goal is not perfection—it is clarity on what matters and what is still unknown.

## Key Responsibilities

**Capture the core problem statement:** Express the idea in plain language. Avoid jargon. The statement should
be clear to someone unfamiliar with the domain. Test yourself: could a peer explain this back to you
accurately?

**Define expected behavior:** Be specific about observables—what the user or system should be able to do, see,
or verify. Expected behavior is measurable. Vague outcomes ("improve performance") require metrics or examples.
Good: "API responses within 200ms for 95th percentile." Avoid: "fast."

**Identify all constraints:** Technical (platform, compliance), legal (licensing, data), temporal (deadlines),
resource (team size, budget). Constraints bound the solution space. Missing constraints cause late surprises.

**Explicitly list non-goals:** What this project will NOT do. Non-goals prevent scope creep and set expectations.
Be specific: "We will not support batch processing" is clearer than "keep it simple."

**Surface assumptions:** What must be true for the plan to remain valid? Examples: "Assumes primary database
has 99.9% uptime," "Assumes team has access to staging environment," "Assumes existing API contract
remains stable." Explicitly call out any dependency on external teams or systems.

**Identify blocking unknowns:** What open questions would change the design or timeline? Prioritize the minimum
set of questions that would unlock planning. Avoid analysis paralysis—some questions can be answered during
execution.

## Decision Framework

If the idea is already clear and the S1 outline is complete:

- Restate the agreed behavior and the primary risks.
- Confirm the agreement checkpoint (is everyone aligned?).
- Move directly to Stage II planning.

If the idea is vague:

- Ask the fewest clarifying questions that remove ambiguity.
- Prioritize questions that affect scope or risk, not implementation details.
- Suggest concrete examples to ground discussion.

## Good Practices

- **Explicit over implicit.** Write assumptions down. Unspoken assumptions breed misalignment.
- **Separate concerns.** Distinguish between the problem (what) and the solution (how). Stage I addresses the problem.
- **Fail fast.** Catch scope creep and misaligned expectations now, not during implementation.
- **Be critical but constructive.** Point out gaps, but help the team understand why they matter.