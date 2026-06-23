---
description: Analyze dependencies and integration points between phases to identify risks and validate sequence
---

## Dependency and Integration Analysis

Before executing phases or features, map dependencies and integration points. This prevents blocking,
rework, and cascading failures.

When dependencies involve external libraries/frameworks/APIs, fetch the latest relevant references
with Context7 MCP before finalizing dependency constraints or integration risks.

Use this prompt when the project has non-trivial cross-phase coupling, multiple teams, external
integrations, or parallel execution risk. For simple phase plans, keep dependency notes in
[phase-plan.prompt.md](.github/prompts/phase-plan.prompt.md) and do not invoke this prompt.

Use this prompt to:
- Identify what each phase depends on
- Detect circular dependencies or sequence problems
- Plan parallel work where possible
- Identify integration test points
- Document rollback dependencies

## Types of Dependencies

### Code/Schema Dependencies

Phase B cannot start until Phase A delivers a specific code capability or database schema.

Example:
- Phase 1 must deliver the user table before Phase 2 can build the login UI
- Phase 3 (caching) depends on Phase 2 API endpoints being stable (no breaking changes)

**Symbols:**
- Soft dependency: "Phase B should wait for Phase A, but could start with mock/stub" (optional, reduces risk)
- Hard dependency: "Phase B cannot start without Phase A" (blocking)

### Infrastructure Dependencies

Phase B needs infrastructure provisioned by Phase A (or external team).

Example:
- Backend cannot deploy without database infrastructure (RDS, PostgreSQL)
- Frontend cannot load-test without staging environment
- Email notifications cannot test without SES provisioned

**Risk:** Infrastructure provisioning may take longer than expected (external teams, approval processes).

**Mitigation:** Request infrastructure early; do not assume it exists.

### Data/Permission Dependencies

Phase B needs data created or permissions granted by Phase A or external team.

Example:
- QA cannot test features without test user accounts
- Frontend cannot test payment flow without Stripe credentials
- Mobile app cannot test push notifications without Apple/Google configurations

### Team/Resource Dependencies

Phase B depends on the same person or team finishing Phase A.

Example:
- Only Alice knows the auth system (Alice is bottleneck if Phase 2 needs auth changes)
- Mobile and backend work in parallel (need 2 engineers)

**Risk:** Key person unavailable → phase blocked.

**Mitigation:** Knowledge transfer, pair programming, documentation.

## Dependency Mapping

For each phase, ask:

1. **What must be complete before this phase starts?**
   - Prior phases (which ones?)
   - Infrastructure (what needs provisioning?)
   - External approvals (security review? legal? product?)
   - Team availability (who must be available?)

2. **What does this phase deliver that future phases depend on?**
   - Code/APIs
   - Database schema
   - Infrastructure
   - Documentation

3. **Are there any circular dependencies?**
   - Phase A depends on Phase B?
   - Phase B depends on Phase A?
   - If yes: You have a sequencing problem. Break the cycle.

4. **Can any phases run in parallel?**
   - Phase 2 and Phase 3: Can they work independently?
   - If yes: Can save time (but increases coordination complexity)

### Dependency Matrix Example

```
          Depends On
          │ P1 │ P2 │ P3 │ P4 │
├────┼────┼────┼────┼────┤
│ P1 │    │    │    │    │  <- No dependencies (start first)
├────┼────┼────┼────┼────┤
│ P2 │ 🔴 │    │    │    │  <- Depends on P1 (hard)
├────┼────┼────┼────┼────┤
│ P3 │ 🟡 │ 🔴 │    │    │  <- Soft dep on P1 (can use stubs), hard dep on P2
├────┼────┼────┼────┼────┤
│ P4 │    │ 🟡 │ 🟡 │    │  <- Soft deps on P2 & P3 (can start with stubs)
└────┴────┴────┴────┴────┘

🔴 = Hard dependency (blocking)
🟡 = Soft dependency (can work with stubs/mocks)
```

## Sequence Validation

**Is the proposed sequence valid?**

For each phase, check:
- [ ] All hard dependencies are satisfied (prior phases complete)
- [ ] All soft dependencies have a mock/stub available (if phase wants to start early)
- [ ] Infrastructure dependencies are known and requested
- [ ] No circular dependencies exist
- [ ] Key people are available when needed

Example validation:
```
 Phase 1: ✅ No dependencies, can start immediately
 Phase 2: ✅ P1 complete, can start Day X
 Phase 3: ❌ Hard dep on P2, but soft dep on P1 (can mock)
          ⚠️  NOTE: If P2 overruns, P3 is blocked. Consider starting P3 with stubs.
 Phase 4: ✅ Soft deps on P2 & P3, can start in parallel with P3 (needs coordination)
```

## Integration Test Points

Where phases meet, create integration tests:

**Phase 1 → Phase 2 (Backend API → Frontend):**
- Frontend calls backend endpoints
- Data flows correctly (API response format, pagination, error codes)
- Authentication works (JWT tokens, expiration)
- Integration test: Frontend signup → backend saves user → API returns token → frontend receives token

**Phase 2 → Phase 3 (Frontend → Notifications):**
- When frontend creates a post, backend job queue is triggered
- Email is sent correctly
- Integration test: Create post via frontend → verify email sent → verify recipient

**Phase 1 → Phase 3 (API → Caching):**
- Cached data is consistent with database
- Cache invalidation works when data changes
- Integration test: Update post → verify cache invalidated → verify new data served

### Integration Test Checklist

For each integration point:
- [ ] Define what "integration works" means (specific data, timing, error cases)
- [ ] Write the integration test (before phases even touch the boundary)
- [ ] Run it in staging environment (before production)
- [ ] Include in pre-deployment validation

## Parallel Phase Execution

Can phases run in parallel (with different teams)?

**Conditions for parallelization:**
- Phases have no hard dependencies on each other (or can use stubs)
- You have enough people
- Integration points are well-defined and tested

Example:
- Phase 2 (Frontend) can run in parallel with Phase 1 (Backend) if:
  - Phase 1 provides a working stub API early
  - Frontend team knows the API contract
  - Integration tests are defined upfront

**Cost of parallelization:**
- Coordination overhead (more communication, more meetings)
- Rework risk (if API contract changes, frontend may need to adjust)
- Merge complexity (parallel branches that need integration testing)

**When NOT to parallelize:**
- Teams are small (communication overhead > benefit)
- Phases have hard dependencies (one must finish before other starts)
- Risk of breaking integration is high

**Best practice:** Run phases sequentially unless clear benefit to parallelization.

## Handling Broken Dependencies

**Scenario: A hard dependency is not met (e.g., infrastructure not provisioned).**

**Options:**
1. **Wait:** Pause the phase until dependency is met (simple, but delays timeline)
2. **Stub/Mock:** Create a temporary mock that mimics the dependency (allows progress, requires rework later)
3. **Resequence:** Move the phase earlier or split it differently (changes project timeline)

Example: Backend needs database, but infrastructure team hasn't provisioned it yet.
- Option 1: Wait for infrastructure (blocks backend team)
- Option 2: Use SQLite locally for development; migrate to Postgres when infrastructure ready (allows progress)
- Option 3: Do dependency analysis first; request infrastructure before phase starts (best practice)

**Best practice:** Identify dependencies in S2 planning. Request infrastructure, approvals, and data early.

## Rollback Dependencies

When planning rollback, consider dependencies:

**Scenario: Phase 2 (Frontend) shipped with a bug. Can we rollback?**

Depends on:
- Did Phase 2 change the database schema? (If yes, rolling back code may leave stale schema)
- Did Phase 2 change the API contract? (If yes, rolling back frontend may leave users with incompatible versions)
- Are there data dependencies? (If frontend created new data, rollback may lose it)

Example rollback plan:
```
Rollback Phase 2 Frontend:
- Revert frontend code to previous version
- Verify API is still compatible (no schema changes, no breaking API updates)
- No data loss (frontend doesn't write to new tables)
- Rollback time: 5 minutes
- Risk: Users with new frontend version in cache may see errors (mitigate: invalidate cache)

Rollback Phase 1 Backend:
- Revert backend code to previous version
- Run database migration rollback script
- Restore database snapshot if needed
- Rollback time: 30 minutes (includes database migration)
- Risk: High (touches data layer); requires backup and dry-run in staging
```

## Example: Dependency Analysis for User Forum Platform

```markdown
# Dependency Analysis: User Forum Platform

## Phase Sequence

Proposed: Phase 1 → Phase 2 → Phase 3 → Phase 4

## Phase 1: Core API & Database

Dependencies:
- None (first phase)

Deliverables:
- PostgreSQL database schema (users, posts, comments, topics)
- REST API endpoints (CRUD for users/posts/comments)
- JWT authentication
- Email service (SES) integration

Data available for Phase 2:
- API endpoints stable (no breaking changes planned)
- Test data: seed script with 100 test users, 500 test posts
- Documentation: API contract (OpenAPI spec)

## Phase 2: Frontend (Web & Mobile)

Dependencies:
- Hard: Phase 1 complete and stable API in staging
- Hard: API contract documented (OpenAPI)
- Soft: Phase 1 test data available (or can create own)
- Infrastructure: Staging environment with Phase 1 deployed

Deliverables:
- Web UI (React)
- Mobile UI (React Native)
- E2E tests covering signup → post → comment workflow

Integration points with Phase 1:
- Frontend calls /api/signup, /api/login, /api/posts endpoints
- Frontend receives JWT token; must include in subsequent requests
- Frontend handles API error responses (400, 401, 404, 500)

Integration test:
```
test_signup_to_post_creation():
  1. POST /api/signup with email, password → receive JWT
  2. GET /api/posts with JWT → receive post list
  3. POST /api/posts with title, body → create post
  4. GET /api/posts → new post visible
```

## Phase 3: Notifications

Dependencies:
- Hard: Phase 1 API stable (will hook into comment creation)
- Soft: Phase 2 frontend (can add notification settings UI, but test with curl)
- Infrastructure: Background job queue running (Bull + Redis)
- Infrastructure: Email service (SES) credentials

Deliverables:
- notification_preference table
- Background job to send emails
- Notification settings UI (Phase 2 team adds later)

Integration points:
- Phase 3 hooks into Phase 1's comment creation endpoint
- When comment posted, Phase 3 job queue is triggered
- Email sent asynchronously

Integration test:
```
test_email_sent_on_comment():
  1. User A creates post via /api/posts
  2. User B comments on post via /api/comments
  3. Phase 3 background job detects comment
  4. Email sent to User A
  5. Verify: Email in inbox with comment text
```

## Phase 4: Advanced Features

Dependencies:
- Soft: Phase 1 API (can integrate later)
- Soft: Phase 2 frontend (can integrate later)
- Soft: Phase 3 notifications (can integrate later)

(Feature work; can be scoped independently)

## Parallelization Opportunities

**Can Phase 2 and Phase 3 run in parallel?**
- Phase 3 depends on Phase 1 API being stable
- Phase 2 also depends on Phase 1 API
- Both can start together, but must wait for Phase 1
- Coordination cost: Phase 2 and Phase 3 teams need to communicate on notification_preference table (needed by both)
- Recommendation: Run sequentially (Phase 2 first, Phase 3 after). Reduces coordination.

**Can Phase 4 run in parallel with Phase 3?**
- Yes, if features are independent
- Coordination: Minimal (different features, different tables)
- Recommendation: Yes, parallelize if resources available

## Dependency Risks

- **Risk 1: Phase 1 API contract changes during Phase 2 (breaking change)**
  - Mitigation: Phase 1 team commits to API stability during Phase 2 timeline
  - Mitigation: Phase 2 has integration tests that fail if API changes

- **Risk 2: Background job queue not provisioned before Phase 3 starts**
  - Mitigation: Provision infrastructure before Phase 1 starts
  - Mitigation: Phase 3 team has local job queue (Bull + Redis) for development

- **Risk 3: Phase 2 overruns, delays Phase 3 start**
  - Mitigation: Phase 3 team starts with stubs; mocks API instead of calling real endpoints
  - Mitigation: Once Phase 2 done, Phase 3 integrates real endpoints

## Sequence Validation

✅ Phase 1 → Phase 2: Valid (Phase 1 complete, API stable, test data available)
✅ Phase 2 → Phase 3: Valid (Phase 1 API still stable; Phase 3 doesn't depend on Phase 2 UI)
✅ Phase 3 → Phase 4: Valid (Phase 4 independent)
✅ Phase 2 || Phase 3: Possible (both depend on Phase 1; can run parallel if coordination is tight)

Recommendation: Execute sequentially (Phase 1 → Phase 2 → Phase 3 → Phase 4)
Alternative: Run Phase 3 in parallel with Phase 2 if a second team is available
```

## Checklist: Am I Ready to Commit to This Sequence?

- [ ] All hard dependencies are identified and feasible
- [ ] Infrastructure dependencies are requested and have ETAs
- [ ] External approvals/reviews have timelines
- [ ] No circular dependencies exist
- [ ] Integration test points are defined
- [ ] Rollback dependencies are understood
- [ ] Team has capacity to execute (no key-person bottlenecks)
- [ ] Phase estimates account for integration testing

If all checked: Proceed to Phase execution.
If any unchecked: Go back to S2 planning to clarify and resolve.
