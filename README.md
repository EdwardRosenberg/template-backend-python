# template-backend-python

**Tier 1 — Python Stack Template**

A stack-level template for Python backend services. It inherits organization-wide governance from [`template-base`](https://github.com/EdwardRosenberg/template-base) and adds Python-specific build tooling and quality gates — without any application boilerplate.

## What This Template Provides

| File / Directory | Purpose |
|---|---|
| `pyproject.toml` | Project configuration with dependency management, ruff, mypy, and pytest settings |
| `ruff.toml` | Ruff linter/formatter configuration (replaces flake8 + isort + black) |
| `.python-version` | Pins the Python version for pyenv / CI consistency |
| `.github/workflows/ci.yml` | CI that calls the base reusable workflow with `backend-tech-stack: python` |
| `.github/workflows/pr-title-lint.yml` | Delegates PR title validation to the base reusable workflow |
| `.github/dependabot.yml` | Automated dependency updates for GitHub Actions and pip |
| `sync-config.yml` | Declares `template-base` as the parent for platform-level sync |

## What This Template Does NOT Provide

- A runnable application (no web server, no routes)
- Framework-specific dependencies (FastAPI, Flask, Django)
- Application configuration or entry points

For a ready-to-run application scaffold, use a **Tier 2 archetype template** such as [`template-backend-python-fastapi`](https://github.com/EdwardRosenberg/template-backend-python-fastapi).

## Hierarchy

```
template-base  (Tier 0 — org-wide governance)
    └── template-backend-python  (Tier 1 — this repo, Python stack tooling)
            └── template-backend-python-fastapi  (Tier 2 — FastAPI archetype)
```

## Build Commands

```bash
# Install dependencies (in a virtual environment)
python -m venv venv
source venv/bin/activate   # or venv\Scripts\activate on Windows
pip install -e ".[dev]"

# Run linter
ruff check .

# Run formatter check
ruff format --check .

# Run type checker
mypy .

# Run tests
pytest
```

## Quality Gates

| Gate | Command | Enforced in CI |
|---|---|---|
| Lint | `ruff check .` | ✅ |
| Format | `ruff format --check .` | ✅ |
| Type check | `mypy .` | ✅ |
| Tests | `pytest` | ✅ |

## Sync

This template syncs governance files from `template-base` via the platform sync workflow. See [`sync-config.yml`](sync-config.yml) for the list of synced paths.

