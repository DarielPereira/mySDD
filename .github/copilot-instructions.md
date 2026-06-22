# Repository Copilot Instructions

This repository is a template for spec-driven development. When working here, follow these rules:

## Default behavior

- Start from the workflow, then the prompts/templates, then code.
- State assumptions explicitly when the request is incomplete.
- Prefer small, reversible changes.
- Do not introduce new tooling unless it solves a real problem.
- Favor free or open-source tools for personal and small-team use.
- Stop and confirm at stage boundaries instead of silently advancing.

## Stack guidance

- Default to the Python stack documented in `docs/sdd/stack.md`.
- For Android apps, prefer Kivy + Buildozer + SQLite when staying in Python is important.
- For shared multi-user data, default to FastAPI + PostgreSQL behind the app client.
- Use SQLite for offline-first or single-user data, not as the default shared database.
- Reject heavier stacks unless the requirements clearly justify their cost.

## Working protocol

1. Stage I: discuss the idea and expected behavior until the objective is clear.
2. Stage II: plan the incremental development phases, technology stack, and data model together.
3. Stage III: execute one phase at a time using small tasks that can be tested and committed.
4. Stage IV: add new features to an existing or completed project once the initial cycle is stable.
5. Save the Stage I outline and the Stage II plan/stack/data model into `docs/sdd/project-decisions/` as the canonical project record.
6. Before moving to a new stage, ask for confirmation if any decision is still ambiguous.

## Agent behavior

- Be useful by turning vague requests into concrete next steps.
- Be skillful by surfacing tradeoffs, risks, and missing requirements early.
- Do not over-explain when a direct answer is enough.
- Do not under-explain when a decision affects scope, cost, or correctness.
- Prefer the smallest clarifying question that removes the biggest uncertainty.

## SDD workflow

1. Clarify the objective and the non-goals.
2. Write or update the S1-ProjectOutline artifact.
3. Break the work into S2 phase, stack, and data model documents.
4. Split the work into ordered implementation tasks.
5. Implement one task slice at a time.
6. Verify each slice before widening scope.
7. Test and commit each task before moving to the next one.
8. Keep the project decisions folder updated when decisions, phase status, or feature scope change.

## Prompt usage

- Use `clarify.prompt.md` for Stage I alignment.
- Use `phase-plan.prompt.md` for Stage II phase planning.
- Use `S2-decisions.prompt.md` for the initial Stage II architecture record.
- Use `dependency-analysis.prompt.md` only for non-trivial dependency or integration analysis.
- Use `task-slice.prompt.md` for Stage III task decomposition.
- Use `implement.prompt.md` for one task-sized change.
- Use `S3-status-update.prompt.md` when updating implementation status.
- Use `S4-feature-spec.prompt.md` when starting a Stage IV feature.
- Use `validate-and-deploy.prompt.md` for phase-level validation and release planning.
- Use `development-status.prompt.md` to identify the current stage and active phase or task.
- Use `doc-sync.prompt.md` when code and documentation have drifted apart.
- Use `regression-check.prompt.md` for test-heavy plan validation after a change.
- Use `review.prompt.md` for human-style review of correctness, alignment, and quality.
- Use `decision-log.prompt.md` only when an established stack or data model decision changes later.
- Use `retrospective.prompt.md` after a phase or project completes.

## Code quality expectations

- Keep the implementation simple enough to explain in one pass.
- Prefer explicit data flow over hidden magic.
- Add tests when behavior matters or could regress.
- Preserve backward compatibility unless the change explicitly replaces it.
- Treat `docs/sdd/project-decisions/` as the source of truth for current stage, phase, and feature status.
- Use the template and prompt structure to keep spec, plan, implementation, and validation aligned.

## Personal-project constraints

- Assume the project may serve only a few colleagues.
- Optimize for low maintenance and low cost.
- Do not over-engineer for large-scale distributed systems unless the current project truly needs that.

## When Copilot should be critical

- If a request adds complexity, ask whether the benefit justifies it.
- If there are multiple implementation options, choose the simplest one that is still durable.
- If a design is vague, ask for concrete acceptance criteria before coding.
- If the stack choice is unclear, push for the smallest free stack that satisfies the app's real constraints.
- If the requested work does not match the current stage, call that out before proceeding.

## Preferred output style

- Be concise but specific.
- Call out tradeoffs directly.
- Explain why a choice is being made when it matters.
- If a suggestion is risky or expensive, say so plainly.