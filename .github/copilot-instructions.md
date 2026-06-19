# Repository Copilot Instructions

This repository is a template for spec-driven development. When working here, follow these rules:

## Default behavior

- Start from the workflow, then the spec, then code.
- State assumptions explicitly when the request is incomplete.
- Prefer small, reversible changes.
- Do not introduce new tooling unless it solves a real problem.
- Favor free or open-source tools for personal and small-team use.
- Stop and confirm at stage boundaries instead of silently advancing.

## Stack guidance

- Default to the Python stack documented in `docs/sdd/stack.md`.
- For Android apps, prefer Kivy + Buildozer + SQLite when staying in Python is important.
- For shared data, start with SQLite and only move to a server-side database when there is a concrete need.
- Reject heavier stacks unless the requirements clearly justify their cost.

## Working protocol

1. Stage I: discuss the idea and expected behavior until the objective is clear.
2. Stage II: plan the incremental development phases and their validation.
3. Stage III: execute one phase at a time using small tasks that can be tested and committed.
4. Record Stage I decisions in the project decisions folder before leaving Stage I.
5. Before moving to a new stage, ask for confirmation if any decision is still ambiguous.

## Agent behavior

- Be useful by turning vague requests into concrete next steps.
- Be skillful by surfacing tradeoffs, risks, and missing requirements early.
- Do not over-explain when a direct answer is enough.
- Do not under-explain when a decision affects scope, cost, or correctness.
- Prefer the smallest clarifying question that removes the biggest uncertainty.

## SDD workflow

1. Clarify the objective and the non-goals.
2. Write or update the spec.
3. Convert the spec into a design note.
4. Split the work into ordered implementation tasks.
5. Implement one task slice at a time.
6. Verify each slice before widening scope.
7. Test and commit each task before moving to the next one.
8. Keep the project decisions folder updated when decisions or phase status change.

## Code quality expectations

- Keep the implementation simple enough to explain in one pass.
- Prefer explicit data flow over hidden magic.
- Add tests when behavior matters or could regress.
- Preserve backward compatibility unless the change explicitly replaces it.

## Personal-project constraints

- Assume the project may serve only a few colleagues.
- Optimize for low maintenance and low cost.
- Do not over-engineer for large-scale distributed systems unless the current project truly needs that.

## When Copilot should be critical

- If a request adds complexity, ask whether the benefit justifies it.
- If there are multiple implementation options, choose the simplest one that is still durable.
- If a design is vague, ask for concrete acceptance criteria before coding.
- If the stack choice is unclear, push for the smallest free stack that satisfies the app's real constraints.

## Preferred output style

- Be concise but specific.
- Call out tradeoffs directly.
- Explain why a choice is being made when it matters.
- If a suggestion is risky or expensive, say so plainly.