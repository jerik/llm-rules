# Python Policy

Apply these rules to all Python code in this repository. Prefer the simplest correct solution.

## Code Standards
- Target: Python 3.x (project default)
- Style: PEP 8, 4-space indent, max ~100 chars/line
- Typing: Type hints for public functions and non-trivial logic
- Models: Prefer `dataclasses` for simple data structures
- Naming:
  - `snake_case`: functions, variables
  - `PascalCase`: classes
  - `UPPER_SNAKE_CASE`: constants
- Errors: Fail fast with clear exceptions, validate inputs at boundaries
- Logging: Use `logging` module (no print), no sensitive data in logs
- Structure: Small modules, no circular imports, separate domain logic from IO

## Patterns
- Prefer pure functions and dependency injection
- Composition over inheritance
- Isolate side effects
- No over-engineering: no unnecessary abstractions or premature generalization
- Scripts/CLI: predictable exit codes, robust argument parsing, idempotent operations

## Tools (Mandatory)
| Task | Tool | Command |
|------|------|---------|
| Package management | uv | `uv pip install`, `uv sync` |
| Virtual env | uv | `uv venv` |
| Linting & Format | ruff | `uv run ruff check`, `uv run ruff format` |
| Testing | pytest | `uv run pytest` |

- Respect existing `pyproject.toml` config
- Do not introduce dependencies without justification

## Workflow
- Before coding: State assumptions, identify files to modify
- Always use virtual environments
- After coding: Ensure tests pass, summarize changes and risks
