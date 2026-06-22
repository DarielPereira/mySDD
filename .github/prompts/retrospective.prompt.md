---
description: Conduct a phase or project retrospective to capture learning and improve future work
---

## Retrospectives: Learning from Execution

After a phase completes (Stage III) or the project concludes (Stage IV), conduct a retrospective to
capture what worked, what did not, and what to improve next time.

Retros are not blame sessions—they are learning sessions. The goal is to improve the SDD workflow itself
and team capability.

## When to Run a Retrospective

- **After each phase completes** (Stage III): One hour, the team that executed the phase
- **After the project concludes** (after Stage IV, before archiving): One hour, full team
- **After a major blocker or rework**: Optional, but recommended if something went seriously wrong

## Phase Retrospective (After Stage III Phase Completion)

Scope: One phase only. Questions:

**What went well?**
- Which tasks completed faster than estimated? Why?
- What decisions (technology, data model, task breakdown) proved solid?
- What process worked (daily standup? async updates? code review cadence)?
- Did any unexpected learning accelerate later tasks?

Examples:
- "Task 2 (email validation) finished in 2 days instead of 3 because we reused validation from Phase 1"
- "Decision to use async job queue prevented blocking the API endpoint; good call"
- "Daily standup caught a dependency issue early; would have blocked Task 5"

**What was difficult?**
- Which tasks underestimated effort? Why?
- What decisions require rework or clarification?
- Where did the spec fall short or mislead?
- What external blockers slowed progress?

Examples:
- "Task 4 (integration testing) took 4 days instead of 1; test environment was flaky (database timeouts)"
- "Data model change mid-phase (adding user_id to posts) cascaded into 2 extra days of work"
- "Spec said 'email validation' but did not clarify format rules; had to ask midway through"

**What would you do differently next time?**
- How would you prevent the difficulties?
- What should the next phase learn from this one?
- Should the workflow (Stage II planning, task breakdown, validation) change?

Examples:
- "Next time, test environment should be verified before Phase starts (not discovered mid-Phase)"
- "Spec review should include 15-min \"questions\" standup before work begins"
- "Email validation logic should be extracted as utility function (avoid duplication in Phase 2)"

**Did the status file help?**
- Was the status file updated consistently?
- Did it accurately reflect progress?
- If work paused, could you resume from the notes?

If the status file was neglected, discuss why (too much overhead? not useful?) and whether to continue it.

## Project Retrospective (After All Phases / Features Complete)

Scope: Full project. Questions:

**Did we achieve the original goal?**
- Reread the S1 project outline. Did the final project match the stated idea?
- Are the non-goals still non-goals? (Did scope creep occur?)
- What was built? What was deferred or dropped?

Examples:
- "S1 goal: 'MVP user platform.' We delivered user auth + posting + simple feed. Good alignment."
- "Non-goal: 'No real-time notifications.' We added real-time in Phase 4 (scope creep, but worth it)"

**How close were estimates?**
- Reread the S2 phase plan. Did phases complete within estimates?
- Which phases overran? Why? (Unknowns discovered? spec misses? external blockers?)
- Can you predict future projects better based on this data?

Examples:
- "Phase 1 took 10 days vs. 7 estimated (42% overrun). Reason: database schema reviews took 2 extra days."
- "Phase 2 took 8 days vs. 8 estimated (on target). Good task breakdown helped."
- "Pattern: database/schema work always takes longer than estimates; allocate 1.5x buffer next time"

**Technology and data model: Did they hold up?**
- Did the S2 stack and data model support all phases without major rework?
- Were any technology decisions regretted? (Had to switch mid-project?)
- Did denormalization/normalization decisions work as intended?

Examples:
- "PostgreSQL choice was solid; handled load well. No regrets."
- "Denormalizing user_name into posts was premature (added redundancy, minimal perf gain)."
- "JSON fields in user_profile turned out to be a mistake; should have normalized. Lesson: avoid JSON for queryable data."

**What did we learn that changes future projects?**
- Process learnings: workflow steps, estimation, communication
- Technical learnings: libraries, patterns, architecture decisions
- Organizational learnings: team capability, dependencies, resource constraints

Examples:
- "SDD workflow with explicit phases and status tracking prevented scope creep. Keep this going."
- "Task breakdown down to 1-day granularity was right (5-day tasks led to uncertainty)."
- "Underestimated integration complexity (mocking external services is harder than expected)."

**What should the next project do differently?**
- Apply learnings immediately. Do not let knowledge decay.
- Update templates, prompts, or checklists based on what you learned.

Examples:
- "Update S2-TechnologyStack template to include 'database selection criteria' section."
- "Add to task-slice prompt: 'For database tasks, estimate 1.5x initial gut feeling.'"
- "Create a 'integration testing checklist' for future projects with external service mocking."

## Running the Retrospective Meeting

**Format:** 60 minutes, synchronous if possible (async notes if remote/async team)

**Time allocation:**
- 5 min: Context (read S1 outline, review phase plan, review status file)
- 20 min: What went well
- 20 min: What was difficult
- 10 min: What to do differently
- 5 min: Action items (who updates templates, processes, docs?)

**Facilitation tips:**
- Ask "why" questions to dig into root causes (not blame, but understanding)
- Encourage specificity (not "communication was hard" but "code review feedback arrived after 2 days; too late to act")
- Separate "things we did right" from "things we got lucky on" (both are valuable, different implications)
- Record action items with owners (e.g., "Alice updates S2 template by Friday")

**Document the retro.** Create a file:
`docs/sdd/project-decisions/Retro-<phase-or-project>.md`

Include:
- Date
- Attendees
- What went well
- What was difficult
- Key learnings
- Action items (with owners)

Example:
```markdown
# Phase 2 Retrospective (Nov 2024)

**Date:** Nov 10, 2024
**Attendees:** Alice (Lead), Bob (Backend), Carol (QA)

## What Went Well
- Async job queue implementation clean; no rework
- Code review turnaround improved (avg 4 hours vs. 24h previous phase)
- Status file kept us on track despite mid-week interruption

## What Was Difficult
- Test database seeding took 2 days (setup docs were wrong)
- Email service rate limits not documented (caused testing delay)
- Spec was vague on retry logic; had to ask mid-task

## Key Learnings
- Test environment setup should be verified in Phase 0 (pre-execution)
- Email service provider docs should be read BEFORE tasks, not during
- Retry logic should be explicitly in spec (not assumed)

## Action Items
- **Bob:** Update test setup docs by Nov 13
- **Carol:** Add email provider checklist to S2 template
- **Alice:** Add "retry strategy" field to S3 task template
```

## Following Up on Learnings

**Immediately after the retro:**
- Update templates and prompts based on action items
- Share retro notes with team (and any cross-team stakeholders)
- Schedule next project's Stage I with the updated workflow

**Long-term:**
- Track recurring issues across projects (e.g., "estimates are always 1.5x reality")
- Invest in fixing systemic problems (e.g., improve test environment, hire DevOps help)
- Build institutional knowledge: create guides, checklists, and examples based on lessons

## Retrospectives as Culture

Regular retrospectives signal that:
- Learning is valued, not blame
- The team can improve its own processes
- Mistakes are opportunities (not failures)
- Past work informs future work

Consistently running retros, acting on findings, and tracking improvements builds a team that gets
better at software delivery with each project.
