# Recommended Stack

This repository is intentionally optimized for personal projects and small collaboration, so the stack should stay free, simple, and easy to maintain.

## Default Python stack

- Python 3.12+
- `venv` for environments
- `pip` for package installation
- `pytest` for tests
- `ruff` for linting and formatting
- `sqlite3` for local storage
- `pydantic` for data validation when needed
- `fastapi` for a small API layer when a service is required

## Android app path

If the goal is to build Android applications while staying in Python, the most practical free stack is:

- `kivy` for the app UI
- `buildozer` for Android packaging
- `sqlite3` for local app data

This path is reasonable for personal projects, prototypes, and small internal tools. It is not the strongest choice if you need a highly polished native Android experience.

## Database guidance

Prefer the simplest storage layer that fits the use case:

1. Start with SQLite for local, offline-first data.
2. Add a small API only when multiple devices or shared data actually require it.
3. Use a server-side database only when the project really needs centralization.

Recommended options by need:

- Single-device or offline-first app: SQLite
- Small shared service: FastAPI + SQLite
- Growing app with more structure: FastAPI + PostgreSQL if and only if SQLite becomes a real limitation

## What to avoid by default

- Paid hosting or paid backend platforms for hobby projects
- Premature microservices
- Heavy mobile stacks that add complexity without clear payoff
- Database choices that require operational overhead the project does not need

## Decision rule

If a project can be solved with Python, SQLite, and a small number of dependencies, do that first. Move to heavier tools only after the simpler stack becomes a proven bottleneck.