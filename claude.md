# Claude Code Instructions — template-backend-python

This is a **Tier 1 Python stack template**. It provides Python-specific build tooling and quality gates inherited by all Python archetype templates (Tier 2). It contains **no runnable application** — only shared tooling configuration.

## Repository Purpose

Provide shared Python project configuration (`pyproject.toml`, ruff, mypy, pytest settings) that archetype templates like `template-backend-python-fastapi` inherit. This ensures consistent linting, formatting, type checking, and testing across all Python projects.

## Tech Stack

- **Language**: Python 3.11+
- **Linter/Formatter**: Ruff (replaces flake8 + isort + black)
- **Type checker**: mypy (strict mode)
- **Test framework**: pytest
- **CI**: Calls `template-base` reusable workflow with `backend-tech-stack: python`

## Build Commands

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate   # or venv\Scripts\activate on Windows

# Install dev dependencies (when pyproject.toml has content)
pip install -e ".[dev]"

# Quality checks
ruff check .               # Lint
ruff format --check .      # Format check
mypy .                     # Type check
pytest                     # Tests
```

This repo has no runnable application. The `pyproject.toml` defines shared tool configuration that archetype templates extend.

## What You Can Change

- `pyproject.toml` — Tool configuration for ruff, mypy, pytest; shared dependency constraints
- `ruff.toml` — Ruff linter/formatter rules (if present)
- `.python-version` — Pinned Python version for pyenv and CI
- `sync-config.yml` — Sync paths and parent/child declarations
- `.github/workflows/ci.yml` — CI configuration
- `.github/dependabot.yml` — Dependency update configuration

## What You Must NOT Change

- Do not add application source code (`app/`, `src/`)
- Do not add framework-specific dependencies (FastAPI, Flask, Django)
- Do not add entry points or server configuration

## Python Conventions

- **Python version**: 3.11+ — use modern syntax (match statements, type union `X | Y`, `Self` type)
- **Type hints**: Required on all function signatures — mypy strict mode is enabled
- **Imports**: Use absolute imports; ruff enforces isort-compatible ordering
- **Line length**: 120 characters (configured in ruff)
- **String quotes**: Double quotes (ruff default)
- **Docstrings**: Use Google-style docstrings for public functions and classes
- **Naming**: `snake_case` for functions/variables, `CamelCase` for classes, `UPPER_SNAKE_CASE` for constants

## Ruff Rules

The following rule sets are enabled (configured in `pyproject.toml` or `ruff.toml`):

| Code | Category | Purpose |
|---|---|---|
| `E`, `W` | pycodestyle | Basic style errors and warnings |
| `F` | pyflakes | Unused imports, undefined names |
| `I` | isort | Import ordering |
| `N` | pep8-naming | Naming conventions |
| `UP` | pyupgrade | Modernize syntax for target Python version |
| `B` | flake8-bugbear | Common bug patterns |
| `A` | flake8-builtins | Shadowing builtins |
| `SIM` | flake8-simplify | Simplifiable code patterns |
| `TCH` | flake8-type-checking | TYPE_CHECKING block optimization |
| `RUF` | ruff-specific | Ruff's own rules |

## Quality Gates

| Gate | Command | Enforced in CI |
|---|---|---|
| Lint | `ruff check .` | ✅ |
| Format | `ruff format --check .` | ✅ |
| Type check | `mypy .` | ✅ |
| Tests | `pytest` | ✅ |
| PR title lint | `pr-title-lint.yml` workflow | ✅ |

## Sync Awareness

**Parent**: `template-base` (Tier 0)
**Children**: `template-backend-python-fastapi` (Tier 2)

Governance files from `template-base` are synced via PRs. When modifying tool configuration (ruff rules, mypy settings, pytest options), consider the impact on `template-backend-python-fastapi`.

## Versioning

This repo has no version file or publishable package. Note the appropriate semver increment in your PR description based on your change type:

- **PATCH** — ruff/mypy config tweaks, dependency constraint updates, doc changes
- **MINOR** — new tool integrations, new ruff rule sets enabled
- **MAJOR** — breaking changes like requiring a new Python version or removing tool configuration that children depend on

## PR Conventions

- Titles must follow Conventional Commits: `<type>(<scope>): <description>`
- Run all quality gates (`ruff check .`, `ruff format --check .`, `mypy .`, `pytest`) before submitting
- If modifying ruff/mypy settings, verify against the FastAPI archetype

