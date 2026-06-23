# mySDD

This repository is a template for spec-driven development with GitHub Copilot.

It is intentionally lightweight:

- Markdown-first workflow
- Small, private, or small-team projects
- Copilot used as a collaborator, not as a replacement for specs and tests

## What this template gives you

- A repeatable SDD workflow
- Repo-specific Copilot instructions
- Prompt templates for alignment, planning, implementation, validation, and review
- Workflow templates you can copy into a new project and save under `docs/sdd/project-decisions/`
- A commit-message template for solo development
- A project decisions folder for canonical stage, phase, and feature records

The repository is intentionally shipped as a scaffold. The `docs/sdd/project-decisions/` folder starts with guidance only; the stage artifacts are created later by the project that uses this template.

## Recommended workflow

1. Start with the four-stage workflow in `docs/sdd/workflow.md`.
2. Use the Stage I, II, III, and IV templates in `docs/sdd/templates/` to capture the project record.
3. Save the reviewed stage artifacts into `docs/sdd/project-decisions/` as the project evolves.
4. Use the prompts in `.github/prompts/` to keep each stage focused and consistent.
5. Test and commit each task before moving forward.
6. If implementation drifts from the plan, update the decision documents before widening scope.

## Next step

Copy the templates under `docs/sdd/` into a project-specific folder, then follow the stage gates before writing code.

If you are starting a new project from this template, create the canonical files in `docs/sdd/project-decisions/` as soon as Stage I and Stage II decisions are made.

If you want the project-specific source of truth, start with `docs/sdd/project-decisions/README.md` and keep that folder updated whenever a stage, phase, feature, or architecture decision changes.

The canonical templates live in `docs/sdd/templates/` and the matching prompts live in `.github/prompts/`.

