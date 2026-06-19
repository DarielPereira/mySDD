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
- Prompt templates for planning, implementation, and review
- Spec and decision-log templates you can copy into any new project
- A commit-message template for solo development
- A recommended Python and Android stack for free personal, small-scale, and shared multi-user projects
- A project decisions folder in `docs/sdd/project-decisions/`

## Recommended workflow

1. Start with the three-stage workflow in `docs/sdd/workflow.md`.
2. Use `docs/sdd/spec-template.md` to define the objective and behavior.
3. Review `docs/sdd/stack.md` when the project depends on Python, Android, or databases.
4. Move to `docs/sdd/design-template.md` and `docs/sdd/tasks-template.md` only after the idea is clear.
5. Use the prompt templates to keep each stage focused.
6. Test and commit each task before moving forward.

## Philosophy

This template is optimized for personal projects and small collaboration. That means:

- Prefer the simplest tool that works
- Avoid paid services unless they clearly save time
- Keep the architecture boring and easy to maintain
- Favor explicit requirements over vague implementation ideas

## Next step

Copy the templates under `docs/sdd/` into a project-specific folder, then follow the stage gates before writing code.

If you want the project-specific source of truth, start with `docs/sdd/project-decisions/README.md` and keep that folder updated whenever a planning decision changes.

To use the commit template locally, run:

```bash
git config commit.template .gitmessage.txt
```