---
description: Document the initial Stage II technology stack and data model decisions
---

## Documenting Architecture Decisions in Stage II

You are documenting the technology stack and data model decisions that will guide Phase implementation
in Stage III. Use this prompt for the initial architecture record only. If an established decision changes
later during Stage III or IV, use [decision-log.prompt.md](.github/prompts/decision-log.prompt.md).

These initial decisions are recorded in two canonical registers:

1. [docs/sdd/templates/S2-TechnologyStack-template.md](docs/sdd/templates/S2-TechnologyStack-template.md#L1)
2. [docs/sdd/templates/S2-DataModel-template.md](docs/sdd/templates/S2-DataModel-template.md#L1)

Both documents serve dual purposes:
- **Now:** Communicate the chosen architecture to the implementation team.
- **Later:** Serve as a change log when decisions evolve (technology swaps, data model refactorings).

## Technology Stack Document

**What belongs here:**
- The complete set of technologies used (languages, frameworks, databases, platforms, deployment targets)
- How they interact and what boundaries exist between them
- Why each technology was chosen (criteria, trade-offs considered)
- Alternatives that were evaluated and rejected
- Current risks or trade-offs in the chosen stack
- A "changes" section (initially empty) to track technology shifts

**What does NOT belong here:**
- Implementation details (code examples, API signatures—these belong in the phase tasks)
- Developer setup instructions (belongs in docs/dev/)
- Specific tool configurations (belongs in the repository root as config files)

**Selection criteria to articulate:**
- **Team skill fit:** Does the team have experience with this tech? Learning curve acceptable?
- **Operational simplicity:** Can the team deploy, monitor, and maintain this in production?
- **Cost:** Is the technology free/cheap or expensive? Does cost scale predictably?
- **Ecosystem:** Are libraries, tools, and support available for common problems?
- **Lock-in risk:** Can we switch later if needed, or are we locked in?
- **Scope fit:** Does the technology solve the actual problem, or does it solve a bigger problem
  we do not have?

**For each technology, explain:**
- What role does it play in the system? (e.g., "Database for user accounts and transactions")
- What are its boundaries with other components? (e.g., "API is REST JSON; database is accessed only via ORM")
- What risks or constraints does it have? (e.g., "SQLite cannot handle concurrent writes from multiple processes")

## Data Model Document

**What belongs here:**
- The logical entities and their relationships (user, post, comment, etc.)
- What data each entity stores and why
- Constraints and validation rules (e.g., email must be unique, password must be >8 chars)
- How the data model maps to persistence (tables, collections, files, etc.)
- How the data model supports the phase sequence
- Current risks or trade-offs (e.g., denormalization for performance, deferral of indexes)
- A "changes" section (initially empty) to track data model evolution

**What does NOT belong here:**
- Database-specific syntax (SQL DDL goes in the phase tasks or migration scripts)
- Detailed validation logic (goes in the implementation code)
- API schemas (those are implementation details)

**Key questions to answer for each entity:**
- What is its purpose in the system?
- What data must it store, and what is optional?
- What constraints or rules apply? (uniqueness, referential integrity, value ranges)
- How does it relate to other entities?
- Will it grow unbounded, and if so, are there performance implications?
- Can it be denormalized for performance, or must it stay normalized?

**For relationships:**
- Is it one-to-one, one-to-many, or many-to-many?
- Is the relationship required or optional?
- How will you query it efficiently? (do you need an index, a join, a denormalization?)

## Recording Decisions: Trade-Offs and Alternatives

When documenting a choice:

**Clearly state the decision.** Example: "We chose PostgreSQL for the primary transactional database."

**Explain the trade-off.** What did you gain and lose? Example:
- Gain: ACID transactions, relational queries, strong consistency
- Loss: operational overhead (must run a server), not suitable for offline-first mobile apps

**Mention the alternatives considered.** What else did you evaluate? Example:
- "Considered SQLite: simpler (no server), but concurrent writes are problematic."
- "Considered MongoDB: flexible schema, but transactions were complex in Phase 1."

**Articulate the decision criteria.** Why did the chosen option win? Example:
- "PostgreSQL won because the team has Postgres experience, transactions are non-negotiable,
  and the project does not require offline-first capability."

## Changes Section: Capturing Evolution

Leave the "changes" section in both documents initially empty. When a technology or data model decision
is revisited (e.g., migrating from SQLite to Postgres, adding denormalization for caching), record:

**Decision:** The change being made (e.g., "Migrate from SQLite to PostgreSQL")
**Motivation:** Why now? What problem are we solving? (e.g., "SQLite concurrency limits blocking Phase 3 scaling tests")
**Alternatives:** What else did we consider? (e.g., "Could have refactored app to use mutex; would have been slower")
**Impact:** What changes downstream? (e.g., "Data migration script required; CI/CD must provision Postgres; requires managed service")
**Date:** When was this decision made?
**Phase:** Which phase or feature introduced this change?

## Critical Points for Expert Decisions

**1. Resist overengineering.** Choose technologies that solve the immediate problem (Stages I–III).
Defer infrastructure-as-code, auto-scaling, and distributed caching unless the current phases require them.

**2. Align stack with team capability.** Choosing the "best in class" technology is not optimal if your team
cannot operate it. A simpler technology that the team understands is better.

**3. Plan for evolution.** A good stack and data model should allow Phase 3+ to run faster without rework.
If Phase 2 requires migrating the database or rewriting the API, the stack was not well-chosen.

**4. Document constraints and risks.** Be explicit about limitations (e.g., "SQLite not suitable for >10
concurrent writers" or "NoSQL trade-off: eventual consistency, not ACID"). This prevents painful surprises.

**5. Commit to decisions, but revisit regularly.** Once documented, the stack and data model guide Phase
implementation. However, if implementation reveals the decision was wrong, record the change and move forward.
Do not silently work around a bad decision.

## Before Finalizing the Documents

- Does the team agree with the choices?
- Can the team operate and maintain the chosen stack?
- Does the data model support the full phase sequence without major rework?
- Are risks and trade-offs clearly articulated?
- Are alternatives documented (for future reference)?
- Is the "changes" section in place and ready for evolution?

If anything is uncertain, halt and clarify before moving to Phase execution.
