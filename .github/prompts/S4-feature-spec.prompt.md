---
description: Define the scope and acceptance criteria for a Stage IV feature before implementation
---

## Specifying a Stage IV Feature

You are in Stage IV, adding a feature to a completed project. Before implementation, define the feature
scope clearly using the Stage IV feature specification approach.

Use the template: [docs/sdd/templates/S4-feature-name-tasks-template.md](docs/sdd/templates/S4-feature-name-tasks-template.md#L1)

A Stage IV feature is not a full phase (Stage III). It is a scoped capability that fits into the existing
architecture without major rework. However, it still deserves a clear specification to prevent scope creep
and misalignment with the existing system.

## Feature Specification Components

**Feature name and owner.**
- What is the feature called? (e.g., "Email notifications for post comments")
- Who owns it? (author, team, stakeholder)
- When was it defined? (date)
- What area of the system does it affect? (e.g., "posts, notifications, email service")
- What is the related plan? (link to the original decision or request)

**Purpose and rationale.**
Why does this feature matter? Examples:
- "Users requested instant notification of replies to their posts (user request from survey)"
- "Enables engagement tracking without polling (architecture improvement)"
- "Reduces manual notification checking, improving user retention (business metric)"

**Scope: What changes and what does NOT.**
Be explicit. Examples:

What changes:
- Add notification_preference table (email on/off per user)
- Add background job to send emails when comment is posted
- Add email template for "new comment" notification
- Modify POST /comments endpoint to trigger notification job

What does NOT change:
- Existing comment schema (no new fields)
- Email service provider (use existing SES setup)
- User authentication (use existing JWT validation)
- Database schema outside of notifications (no migration of post table)

**Dependencies and assumptions.**
What must be true for this feature to work?
- "Assumes email service (SES) is already provisioned and credentials in .env"
- "Assumes background job queue (Bull with Redis) is running (from Phase 1)"
- "Depends on post and comment tables existing (from Phase 2)"
- "Assumes team has access to staging email account for testing"

If any dependency is missing, the feature cannot proceed.

**Acceptance criteria.**
What does "done" look like? Be measurable and specific. Examples:

Functional:
- User can enable/disable email notifications in settings (toggle saved to database)
- Email is sent when a comment is posted on user's post
- Email contains comment text, author name, and link to the post
- User receives max 1 email per minute (prevent spam if multiple comments in rapid succession)

Non-functional:
- Email sent within 30 seconds of comment posting
- Email delivery success rate > 95% (retries on temporary failure, logs failures)
- No database queries added to comment posting endpoint (notification triggered async)

Edge cases:
- User receives no email if notifications are disabled
- User receives no email if post author deleted their account before comment was sent
- Notification preference survives user profile updates

**Validation approach.**
How will you verify the feature works?

Manual testing:
- Create a post, have another user comment, verify email arrives
- Test with notifications enabled and disabled
- Test with rapid comments to verify rate limiting

Automated testing:
- Unit tests: notification preference toggle saves/loads correctly
- Integration tests: comment posting triggers notification job
- Email service mock: verify correct recipient, subject, body

Load/performance:
- Background job processes 100 emails per minute without queue buildup

**Risks and trade-offs.**
What could go wrong or what are you deferring?

Risks:
- Email deliverability: if email service is down, notifications fail (mitigate: add retry logic, alert on failures)
- User data privacy: collecting user email preferences (mitigate: document data retention, add delete preference on account deletion)

Trade-offs:
- Synchronous vs. asynchronous: sending email async (faster, but notifications are not instant—mitigate: usually ok, but document 30-second expectation)
- Deferring: two-factor authentication via email (separate feature, can be added later without breaking this)

**Scope control.**
If the feature feels too large, split it.

Example: "Email notifications for post comments" might split into:
- Phase 1: Email on/off preference (no emails actually sent yet)
- Phase 2: Send email when comment posted (on by default)
- Phase 3: Digest email (once per day summary, instead of real-time)

Each phase is independently valuable and testable.

## Distinguishing Features from Phases

**Phase (Stage III):**
- Delivers significant capability (e.g., "authentication system")
- Often requires new infrastructure or data model
- Depends on prior phases

**Feature (Stage IV):**
- Adds capability to an existing system
- Fits within existing architecture
- Can often be reverted without affecting core functionality
- Scoped enough to implement in days or weeks, not months

If the feature requires:
- Migrating the database schema
- Rewriting core components
- Adding a new service or infrastructure

... then it is closer to a Phase and should be re-evaluated. It might be a stage boundary instead.

## Before Implementing: Confirmation Checklist

- [ ] Feature purpose is aligned with project goals (not just nice-to-have)
- [ ] Scope is clear (what changes, what does NOT)
- [ ] Dependencies exist (no external blockers)
- [ ] Acceptance criteria are measurable
- [ ] Risk and trade-offs are documented
- [ ] Feature can be implemented in 1–4 weeks without disrupting other work
- [ ] Team agrees on priority and timeline
- [ ] Related decisions or design notes are linked (if relevant)

If any checkbox is uncertain, halt and clarify before creating tasks.

## Deferred Features

If a feature request does not meet Stage IV criteria, record it as a deferred feature request:
- Feature name
- Why it was deferred (too large, not aligned, external dependency)
- When to revisit (after Phase X, or when external condition changes)
- Who should re-evaluate? (team lead, product owner)

Example:
```
**Deferred:** Two-factor authentication
- Reason: Security feature; not required for MVP; adds complexity to auth flow
- Revisit: After Phase 4 (if user data sensitivity increases) or if security incident requires it
- Owner: Security team to assess priority
```

Deferred features are not dropped—they are explicitly postponed and tracked.
