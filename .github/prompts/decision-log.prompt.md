---
description: Record changes to an existing technology or data model decision
---

## Recording Architectural Decisions

Use this prompt only when an existing Stage II technology stack or data model decision changes during
Stage III or IV. Do not use it for the initial architecture record; use [S2-decisions.prompt.md](.github/prompts/S2-decisions.prompt.md) instead.

This creates a changelog of architectural evolution without duplicating the original decision record.

## When to Record a Decision

**Technology changes:**
- Migrating from one database to another (e.g., SQLite → PostgreSQL)
- Swapping a framework or library (e.g., vanilla JS → React)
- Adding or removing a service (e.g., adding caching layer, dropping a microservice)
- Changing deployment targets (e.g., from EC2 to Kubernetes)

Record in: [docs/sdd/templates/S2-TechnologyStack-template.md](docs/sdd/templates/S2-TechnologyStack-template.md) "Changes" section

**Data model changes:**
- Adding a new entity (e.g., "audit log" table)
- Denormalizing for performance (e.g., storing a computed value in a table)
- Changing relationships (e.g., from many-to-many to one-to-many)
- Restructuring for compatibility (e.g., flattening nested JSON to normalized tables)
- Removing deprecated data

Record in: [docs/sdd/templates/S2-DataModel-template.md](docs/sdd/templates/S2-DataModel-template.md) "Changes" section

## Decision Record Structure

For both technology and data model changes, include:

### 1. Decision (What)

State the change clearly and concretely.

❌ Avoid: "Switched to a better database"
✅ Prefer: "Migrated transactional data from SQLite to PostgreSQL; SQLite remains for read-only cache on client"

### 2. Motivation (Why)

What problem were we solving? What was the trigger?

Examples:
- "SQLite hitting concurrency limits during Phase 3 load testing. Repeated SQLITE_BUSY errors with >3 concurrent writers."
- "Added audit_log table to comply with compliance requirement for data change tracking."
- "Denormalized user_name into posts table to eliminate join in high-traffic query endpoint (GET /feed)."

**Be specific about the problem:**
- Quantify if possible: "POST endpoint latency increased from 50ms to 500ms as posts table grew to 100k rows"
- Reference the phase or task where it surfaced: "Discovered during Phase 3, Task 8: Load testing with 10k concurrent users"
- Link to issue or decision: "See GitHub issue #123 for full discussion"

### 3. Alternatives Considered

What other options did you evaluate? Why did you reject them?

Examples:

**Technology change:**
- "Alternative: Refactor app to use connection pooling and mutexes in SQLite"
  - Rejected: Would require significant codebase changes; team estimated 3 weeks rework
  - Chose PostgreSQL instead: migrating data 2 days, no code changes needed

**Data model change:**
- "Alternative: Solve N+1 join problem with application-level caching (Redis)"
  - Rejected: Adds operational complexity; requires cache invalidation logic
  - Chose denormalization instead: single table query; redundancy acceptable (1 extra KB per post)

If only one alternative was considered, say so: "Alternative considered: stay with SQLite and refactor app to reduce concurrency. Rejected because timeline did not allow."

### 4. Impact

What changes as a result of this decision? Who is affected?

**Code changes:**
- "No code changes required; SELECT statements remain the same (schema abstraction via ORM)"
- "Migration required: convert SQLite export to PostgreSQL import (script created in PR #456)"
- "Breaking change: table name changed from `user_account` to `users`; update all queries"

**Operational impact:**
- "New external dependency: PostgreSQL server. Deployment now requires managed Postgres instance (AWS RDS)."
- "CI/CD changes: test suite now seeds Postgres database instead of SQLite; added .sql migration runner."
- "Data retention: audit logs increase storage by ~50GB/year; added retention policy to delete logs > 12 months."

**Performance impact:**
- "Latency improvement: GET /feed endpoint drops from 500ms to 80ms (verified in staging)."
- "Throughput improvement: can now handle 100 concurrent writes (was limited to 10 with SQLite)."

**Risk and cost:**
- "Risk: production migration window required 30-min downtime (scheduled at 3am UTC). Rollback plan: restore from automated backup."
- "Cost: Postgres managed service adds $150/month. One-time migration cost: 2 engineer-days."

### 5. Date and Context

When was the decision made? What phase/task? Who made it?

Examples:
- "Date: Nov 15, 2024 | Phase: 3 (Load testing) | Task: 8 (Concurrency testing) | Decided by: Architecture team"
- "Date: Jan 3, 2025 | Feature: User auditing (Stage IV) | Task: 2 (Audit log schema) | Decided by: Compliance team + engineering lead"

## Examples

### Technology Change Example

```markdown
## Technology Changes

### Change 1: Migrate from SQLite to PostgreSQL
**Date:** Nov 15, 2024
**Context:** Phase 3, Task 8 (Load testing) | Decided by: Architecture team

**Decision:**
Migrate transactional database from SQLite to PostgreSQL. SQLite remains as read-only cache
on clients for offline capability.

**Motivation:**
During Phase 3 load testing (Oct 28), encountered SQLite concurrency limits. With >3 concurrent
WRITE operations, queries failed with SQLITE_BUSY errors. Testing target was 100 concurrent users;
SQLite could not meet this requirement.

**Alternatives Considered:**
- Refactor app to use connection pooling and mutexes: Estimated 3 weeks engineering; added complexity
- Stick with SQLite and reduce load test target: Not acceptable (business requirement: 100+ concurrent users)

**Impact:**
- Code: ORM queries unchanged (database layer abstraction held)
- Operations: AWS RDS Postgres provisioned; adds $150/month
- Deployment: Created data migration script (PR #456); one-time 15-min migration during 3am UTC downtime
- Performance: Concurrent write throughput increases from 3 to 100+ ops/sec
- Risk: Postgres dependency; if RDS is unavailable, app cannot write (mitigation: automated failover alerts)

**Validation:**
- Load test re-run: 100 concurrent users, sustained 10-min test, 0 SQLITE_BUSY errors
- Data integrity: Automated verification script confirmed 100% data match pre/post migration
```

### Data Model Change Example

```markdown
## Data Model Changes

### Change 1: Add audit_log table
**Date:** Jan 3, 2025
**Context:** Stage IV Feature: User auditing | Decided by: Compliance team + engineering lead

**Decision:**
Add audit_log table to track all mutations to sensitive entities (user, posts, permissions).
Log schema: id, entity_type, entity_id, action (create/update/delete), changed_fields, user_id, timestamp.

**Motivation:**
Compliance requirement: SOC2 Type II certification requires change tracking for sensitive data.
Trigger: Security audit scheduled for Q1 2025.

**Alternatives Considered:**
- Implement audit via database-level triggers: Complex to maintain; not portable across environments
- Use event sourcing pattern: Requires significant architectural refactor; overkill for current scope
- Defer to future: Not acceptable (audit deadline is fixed; Q1 audit is non-negotiable)

**Impact:**
- Schema: Add audit_log table (10 columns); no changes to existing tables
- Code: Add logging hook on save/delete; ~50 lines of code
- Storage: ~1 KB per logged action; estimated 50GB/year (at current data change rate)
- Operations: Add log retention policy (delete logs >12 months) to manage storage
- Performance: Minimal (logging is async; indexed on entity_id and timestamp)

**Validation:**
- Unit tests: Verify all mutations are logged
- Integration tests: Create/update/delete actions trigger correct audit entries
- Manual test: Verify audit logs visible in admin dashboard
```

## Recording Discipline

**Record decisions promptly.** Do not defer decision logging until "later." Record as soon as the decision is made
and communicated to the team.

**Link to source materials.** Reference the GitHub issue, pull request, or discussion where the decision
was debated. This provides context for future readers.

**Be honest about trade-offs.** Do not paper over compromises. If you chose Option A knowing Option B was "better"
but Option A was faster, say so. This helps future teams understand constraints.

**Update context as it changes.** If the impact of a decision becomes clearer over time (e.g., "realized
Postgres added 10% latency in some queries"), update the decision record with the new learning.

**Use the decision log as a retrospective tool.** When the project completes, review the technology and
data model changes. What decisions worked well? What would you change next time?
