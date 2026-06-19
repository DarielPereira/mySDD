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

If the Android app needs shared data for every user, treat the mobile app as a client and add a backend:

- `kivy` for the Android client UI
- `fastapi` for the backend API
- `postgresql` for the central shared database

That is the better default when every user must see the same up-to-date data. SQLite is still fine for offline or single-user storage, but it is not the right default for multi-user shared state.

## Database guidance

Prefer the simplest storage layer that fits the use case:

1. Start with SQLite for local, offline-first data.
2. Add a small API and a central database when multiple devices or shared data need to stay synchronized.
3. Use a server-side database whenever every user must read and update the same data.

Recommended options by need:

- Single-device or offline-first app: SQLite
- Shared multi-user app: FastAPI + PostgreSQL
- Android app with shared data: Kivy client + FastAPI + PostgreSQL
- Growing app with more structure: FastAPI + PostgreSQL plus migrations and background jobs if the project needs them

## What to avoid by default

- Paid hosting or paid backend platforms for hobby projects
- Premature microservices
- Heavy mobile stacks that add complexity without clear payoff
- Database choices that require operational overhead the project does not need

## Decision rule

If a project is single-user or offline-first, solve it with Python, SQLite, and a small number of dependencies first. If the project must stay synchronized across users, start with a central API and PostgreSQL instead of forcing SQLite into a role it does not fit.