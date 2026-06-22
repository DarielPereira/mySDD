---
description: Plan validation, testing strategy, and deployment approach for a phase or feature
---

## Validation and Deployment Planning

Before (or during) Phase III execution, plan how the phase will be validated and deployed to production.
This prevents last-minute surprises and ensures quality gates are met before users see the changes.

This prompt is for phase-level validation and release planning. For a single task implementation slice,
use [implement.prompt.md](.github/prompts/implement.prompt.md). For code review, use [review.prompt.md](.github/prompts/review.prompt.md).

Use this prompt to:
- Define testing strategy (unit, integration, e2e, performance)
- Plan deployment approach (stages, rollback, monitoring)
- Identify validation blockers or risks
- Establish quality gates (what must pass before production)

## Testing Strategy

### Unit Tests

**Scope:** Individual functions, methods, classes. No external dependencies (database, APIs mocked).

**When to write:**
- Before or alongside implementation (TDD optional, but testing required)
- For every non-trivial function
- Especially for business logic, validation, error handling

**Coverage target:** >= 80% for new code. Higher for critical paths (auth, payment, data validation).

**Example test cases:**
```python
# User email validation function
def test_email_validation():
    # Valid emails
    assert is_valid_email("user@example.com") == True
    assert is_valid_email("user+tag@example.co.uk") == True

    # Invalid emails
    assert is_valid_email("not-an-email") == False
    assert is_valid_email("") == False
    assert is_valid_email("user@") == False

    # Edge cases
    assert is_valid_email(None) == False  # null input
    assert is_valid_email("a" * 256 + "@example.com") == False  # too long
```

### Integration Tests

**Scope:** Multiple components working together. Uses test database, test API, but not production.

**When to write:**
- For workflows that cross modules (e.g., user creation → email sending → database record)
- For database operations (create, read, update, delete)
- For external API interactions (with mocked responses)

**Test environment:**
- Test database (seeded with fixture data, reset between tests)
- Mocked external APIs (predictable responses, no actual calls to payment processors, email services, etc.)
- Test server (if testing APIs)

**Example test cases:**
```python
# User signup flow
def test_user_signup_success(test_db, mock_email_service):
    # Act: Create user via API
    response = POST("/api/signup", {"email": "new@example.com", "password": "secure123"})

    # Assert: HTTP 201 Created
    assert response.status_code == 201

    # Assert: User saved to database
    user = test_db.query(User).filter_by(email="new@example.com").first()
    assert user is not None
    assert user.password_hash != "secure123"  # not plaintext

    # Assert: Confirmation email sent
    assert mock_email_service.send_confirmation_email.called_with("new@example.com")

def test_user_signup_duplicate_email(test_db):
    # Arrange: User already exists
    test_db.create(User(email="existing@example.com", password_hash="..."))

    # Act: Try to create another user with same email
    response = POST("/api/signup", {"email": "existing@example.com", "password": "other123"})

    # Assert: HTTP 409 Conflict
    assert response.status_code == 409
    assert response.json["error"] == "Email already in use"
```

### End-to-End (E2E) Tests

**Scope:** Full user workflow in a production-like environment. Real browser (if web) or real API client.

**When to write:**
- For critical user journeys (signup, login, purchase, etc.)
- After unit and integration tests are solid
- Sparingly (E2E tests are slow and fragile; usually 5–10 per project, not 50)

**Test environment:**
- Staging environment (as close to production as possible)
- Real (or staging) external services (if feasible)
- Real database (separate from production)

**Example test cases:**
```python
# User signup → login → create post (browser automation)
def test_user_signup_to_post_creation():
    browser = selenium.Chrome()

    # Step 1: Signup
    browser.get("https://staging.example.com/signup")
    browser.find_element(By.ID, "email").send_keys("testuser@example.com")
    browser.find_element(By.ID, "password").send_keys("TestPassword123!")
    browser.find_element(By.ID, "signup-button").click()

    # Step 2: Verify signup page succeeded
    assert "Check your email for confirmation" in browser.page_source

    # Step 3: Confirm email (fetch link from test email inbox)
    confirmation_link = fetch_confirmation_email("testuser@example.com")
    browser.get(confirmation_link)

    # Step 4: Login
    browser.get("https://staging.example.com/login")
    browser.find_element(By.ID, "email").send_keys("testuser@example.com")
    browser.find_element(By.ID, "password").send_keys("TestPassword123!")
    browser.find_element(By.ID, "login-button").click()

    # Step 5: Verify logged in
    assert "Dashboard" in browser.page_source

    # Step 6: Create post
    browser.find_element(By.ID, "new-post").click()
    browser.find_element(By.ID, "post-content").send_keys("Hello, world!")
    browser.find_element(By.ID, "submit-post").click()

    # Step 7: Verify post appears
    assert "Hello, world!" in browser.page_source
```

### Performance Tests

**Scope:** Response time, throughput, resource usage under load.

**When to run:**
- Before production release (if the feature is performance-critical)
- After major code changes (refactors, new queries)
- Periodically (monthly, quarterly) to catch regressions

**Metrics to track:**
- Latency: p50, p95, p99 response times
- Throughput: requests/sec the system can handle
- Resource usage: CPU, memory, disk during load
- Errors: fail rate under sustained load

**Example performance test:**
```python
# Load test: Can the API handle 100 concurrent users?
def test_api_load(locust):
    class UserBehavior(HttpUser):
        @task
        def get_feed(self):
            # GET /api/feed should respond in <200ms
            self.client.get("/api/feed", timeout=5)

    # Run with 100 concurrent users for 10 minutes
    locust.run(UserBehavior, users=100, duration=600)

    # Assert performance targets
    assert locust.stats.get_response_time_percentile(0.95) < 200  # p95 < 200ms
    assert locust.stats.total_fail_count == 0  # no errors
```

## Deployment Strategy

### Environments

Define stages from dev to production:

1. **Local:** Developer machine (unit tests)
2. **Test:** Shared test environment (integration tests)
3. **Staging:** Production-like environment (E2E and performance tests)
4. **Production:** Live customer environment (monitored, can be rolled back)

Each environment should be as similar to production as possible (same database, services, infrastructure).

### Deployment Approach

**Rolling deployment (safest for most projects):**
- Deploy to 10% of servers, monitor for 10 minutes
- If healthy, deploy to 50%, monitor 10 minutes
- If healthy, deploy to 100%
- Rollback plan: revert to previous version immediately if errors detected

**Blue-green deployment (zero downtime):**
- Maintain two identical production environments (blue and green)
- Deploy new version to green while blue handles traffic
- Switch router to green once verified
- Blue becomes the rollback target

**Feature flags (decouple deployment from activation):**
- Deploy code to production but keep feature disabled
- Gradually enable for % of users (10% → 50% → 100%)
- Can disable instantly if issues detected
- Good for risky or experimental features

### Rollback Plan

Before deploying, answer:
- How will we detect a bad deployment? (error rates, performance degradation)
- How quickly can we rollback? (automated? manual? how long?)
- What data will be lost if we rollback? (database state changes)
- Can we rollback without losing data?

Example rollback plan:
```
Rollback trigger: Error rate > 1% for 5 minutes, or latency p95 > 500ms
Rollback mechanism: Git revert + redeploy (5 min total)
Data risk: If user data was written during bad deployment, rollback may lose it
Mitigation: Database transaction log; can restore to point-in-time if needed (manual, ~30 min)
```

## Quality Gates

Define conditions that must be true before deploying:

- [ ] All unit tests pass (coverage >= 80%)
- [ ] All integration tests pass
- [ ] Critical E2E tests pass (signup, login, core workflows)
- [ ] No performance regressions (p95 latency within 10% of baseline)
- [ ] Security scan passes (no SQL injection, XSS, auth bypasses)
- [ ] Code review approved (at least one other engineer)
- [ ] Database migrations tested and rollback verified
- [ ] Documentation updated
- [ ] Monitoring/alerting configured (will we detect if it breaks?)

If any gate fails, fix it before deploying. Do not skip gates to meet deadlines.

## Phase Validation Checklist

Before marking a phase complete:

- [ ] All acceptance criteria from S2 phase plan are met
- [ ] All quality gates pass
- [ ] Phase tests pass on staging environment
- [ ] Performance benchmarks (if any) met
- [ ] Rollback plan documented and tested
- [ ] Monitoring and alerting configured
- [ ] Team agrees the phase is ready for production
- [ ] Deployment plan communicated to all stakeholders

## Monitoring Post-Deployment

**First hour:**
- Watch error rates, latency, and CPU/memory usage
- Have team on standby for rollback
- Check user reports (Slack, support channels)

**First day:**
- Monitor business metrics (if applicable: signups, revenue, engagement)
- Confirm no unexpected side effects
- Celebrate if things are working!

**First week:**
- Watch for subtle bugs or performance issues that only appear under realistic load
- Gather user feedback
- Adjust monitoring or alerting if needed

## Anti-Patterns to Avoid

- ❌ "We'll test in production" (not acceptable)
- ❌ "We'll fix it in the next release" (yes, but have a rollback plan for now)
- ❌ Deploying without monitoring (how will you know if it breaks?)
- ❌ Skipping E2E tests for risky features (user workflows are where bugs hide)
- ❌ No rollback plan (someday a rollback will be critical)
- ❌ Deploying Friday afternoon (no support team available if things break)

Deploy with confidence: test thoroughly, monitor continuously, keep rollback ready.
