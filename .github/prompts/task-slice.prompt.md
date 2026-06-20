---
description: Break one phase into small testable tasks
---

You are in Stage III of the SDD workflow.

This repository uses a four-stage workflow (Stages I–IV). When working on feature extensions, reuse Stage III task-slicing within Stage IV phases.

Break the current phase into tasks that are:

- Small
- Testable
- Commit-worthy
- Sequentially safe

For each task, include:

- What changes
- What stays unchanged
- How it will be tested
- What tests must pass before committing
- What would block the next task

Do not widen the phase scope. If the phase is still too broad, split it further first.