# mySDD

This repository is a template for spec-driven development with GitHub Copilot.

It is intentionally lightweight:

- Markdown-first workflow
- Free tools only unless a project clearly needs more
- Small, private, or small-team projects
- Copilot used as a collaborator, not as a replacement for specs and tests

## What this template gives you

- A repeatable SDD workflow
- Repo-specific Copilot instructions
- Prompt templates for alignment, planning, implementation, validation, and review
- Workflow templates you can copy into a new project and save under `docs/sdd/project-decisions/`
- A commit-message template for solo development
- A project decisions folder for canonical stage, phase, and feature records

## Recommended workflow

1. Start with the four-stage workflow in `docs/sdd/workflow.md`.
2. Use the Stage I, II, III, and IV templates in `docs/sdd/templates/` to capture the project record.
3. Save the reviewed stage artifacts into `docs/sdd/project-decisions/` as the project evolves.
4. Use the prompts in `.github/prompts/` to keep each stage focused and consistent.
5. Test and commit each task before moving forward.
6. If implementation drifts from the plan, update the decision documents before widening scope.

## Philosophy

This template is optimized for personal projects and small collaboration. That means:

- Prefer the simplest tool that works
- Avoid paid services unless they clearly save time
- Keep the architecture boring and easy to maintain
- Favor explicit requirements over vague implementation ideas

## Next step

Copy the templates under `docs/sdd/` into a project-specific folder, then follow the stage gates before writing code.

If you want the project-specific source of truth, start with `docs/sdd/project-decisions/README.md` and keep that folder updated whenever a stage, phase, feature, or architecture decision changes.

The canonical templates live in `docs/sdd/templates/` and the matching prompts live in `.github/prompts/`.

To use the commit template locally, run:

```bash
git config commit.template .gitmessage.txt
```