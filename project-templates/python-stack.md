# Python Stack

Template für Python-Projekte.

## Coding Standards

### PEP 8
- MUST: PEP 8 Style Guide strikt befolgen

### Type Hints
- SHOULD: Type Hints wo sinnvoll verwenden
- Begründung: Erhöht Typsicherheit, verbessert IDE-Support

### Docstrings
- MUST: Docstrings für alle öffentlichen Methoden

## Tooling

### ruff
- MUST: ruff für Formatierung und Linting verwenden
- Commands:
  ```bash
  ruff format .      # Formatierung
  ruff check .       # Linting
  ```

### pytest
- MUST: pytest für Tests verwenden
- SHOULD: `tmp_path` Fixture für File I/O in Tests
- Command:
  ```bash
  pytest -v
  # oder mit uv:
  uv run pytest -v
  ```

### uv
- MUST: uv für python verwenden
- Commands:
  ```bash
  uv run pytest -v
  uv run ruff check .
  ```

## Definition of Done (Beispiel)

```bash
# 1. Formatierung
ruff format .

# 2. Linting
ruff check .

# 3. Tests
pytest -v

# Bei Fehler: Iterieren bis alle grün!
```

## Config
- Konfiguration in `pyproject.toml`:
  - `tool.ruff`
  - `tool.ruff.format`
  - `tool.pytest.ini_options`
