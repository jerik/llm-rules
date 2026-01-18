# PYTHON POLICY – ROI-BASED
Apply these rules to all Python changes in this repo. Prefer the simplest correct solution.

## Code Standards
- Target: Python 3.x (project default). Keep code readable and explicit.
- Style: PEP 8, 4-space indent, max ~100 chars/line unless existing code differs.
- Typing: Use type hints for public functions and non-trivial logic; prefer `dataclasses` for simple models.
- Naming: `snake_case` for funcs/vars, `PascalCase` for classes, `UPPER_SNAKE_CASE` for constants.
- Errors: Fail fast with clear exceptions; validate inputs at boundaries.
- Logging: Use `logging` (no print) for non-trivial flows; no sensitive data in logs.
- Structure: Keep modules small; avoid circular imports; separate domain logic from IO.

## Patterns
- Prefer pure functions and dependency injection for testability.
- Avoid over-engineering: no unnecessary abstractions, frameworks, or premature generalization.
- Use composition over inheritance; keep side effects isolated.
- For scripts/CLI: predictable exit codes, robust argument parsing, idempotent operations.

## Tool Usage (Mandatory)
- Package management: **use `uv` instead of `pip`** for installs, syncing, and venvs.
- Dependencies: prefer `uv pip install` / `uv sync`; do not use bare `pip`.
- Linting/Format: use **ruff** (lint + format) via `uv run ruff`.
- Testing: use **pytest** via `uv run pytest`.
- Respect existing config in `pyproject.toml`.
- Do not introduce new dependencies without justification and minimal impact.

## Workflow
- Before coding: state assumptions and identify files to touch.
- Always use virtual environments
- After coding: ensure tests pass (or explain why they cannot) and summarize changes + risks.
