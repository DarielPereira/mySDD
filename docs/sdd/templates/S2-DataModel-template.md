# S2 — Data Model (Template)

Use this template to record the project's data model, how the data structures relate to the stack,
and the decisions behind the model selection. The finalized document should be saved in
`docs/sdd/project-decisions/S2-DataModel.md` and treated as the authoritative register for the
project's data model choices.

This document should describe both the current data model and any later changes to it. If the
project changes data requirements or persistence direction, update this document and record the
decision, the motivation, and the impact on the project plan.

## Project context

- Project name:
- Owner(s):
- Date:
- Related S1 project outline:
- Related S2 phase plan:
- Related S2 technology stack:

## Model summary

Write a short summary of the selected data model and why it fits the project.

## Data map

Describe the main entities, records, tables, documents, or objects in the model and how they relate
to each other.

For each major data element include:

- Data element name
- Purpose in the project
- Key fields or attributes
- Relationships to other elements
- Constraints or invariants
- Why it was modeled this way

Example sections:

- Core entities
- Reference or lookup data
- User-generated data
- Derived or computed data
- Stored settings or configuration

## Interactions and boundaries

Explain how the data model relates to the technologies in the stack, including persistence layers,
API boundaries, validation, serialization, indexing, migrations, and any cross-cutting concerns.

## Modeling criteria

List the criteria used to choose the model (simplicity, query patterns, scalability, maintainability,
offline support, consistency, migration cost, licensing, etc.).

## Alternatives considered

List viable alternatives that were rejected and why they were not selected.

## Current risks and tradeoffs

Record known risks, tradeoffs, assumptions, and mitigations for the chosen model.

## Data model changes

Use this section whenever the project changes data direction.

- Change date:
- Data affected:
- New model choice:
- Replaced model choice:
- Motivation for the change:
- Impact on phases, tasks, or timelines:
- Decision record / link:

## Validation and fit

Explain how the selected data model is validated as a good fit for the project (proof-of-concept,
schema review, migration test, prototype, or implementation result).

## Last updated

Last updated: YYYY-MM-DD
