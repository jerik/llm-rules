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

## Testing

Siehe auch: [standard/testing.md](../standard/testing.md)

### Tools

**pytest:**
- MUST: pytest für Tests verwenden
- SHOULD: `tmp_path` Fixture für File I/O in Tests
- SHOULD: pytest-cov für Coverage (optional)

**Commands:**
```bash
pytest -v                    # Alle Tests
pytest tests/test_module.py  # Spezifischer Test
pytest --cov=src tests/      # Mit Coverage
pytest -k "test_add"         # Nur Tests mit "add" im Namen
```

### Test-Struktur

```
project/
├── src/
│   └── calculator.py
└── tests/
    └── test_calculator.py
```

### Naming-Conventions

- Test-Files: `test_*.py` oder `*_test.py`
- Test-Functions: `test_*`
- Test-Classes: `Test*`

### Beispiel

```python
# tests/test_calculator.py
import pytest
from src.calculator import add, divide

def test_add_positive_numbers():
    """Test adding two positive numbers"""
    assert add(2, 3) == 5

def test_add_negative_numbers():
    """Test adding two negative numbers"""
    assert add(-2, -3) == -5

def test_add_zero():
    """Test adding zero (edge case)"""
    assert add(0, 5) == 5

def test_divide_normal():
    """Test normal division (happy path)"""
    assert divide(10, 2) == 5

def test_divide_by_zero_raises():
    """Test division by zero raises error (error path)"""
    with pytest.raises(ZeroDivisionError):
        divide(10, 0)
```

### Fixtures

```python
import pytest
from pathlib import Path

@pytest.fixture
def temp_config_file(tmp_path):
    """Create a temporary config file for testing"""
    config_file = tmp_path / "config.json"
    config_file.write_text('{"key": "value"}')
    return config_file

def test_load_config(temp_config_file):
    """Test loading config from file"""
    config = load_config(temp_config_file)
    assert config["key"] == "value"
```

### File I/O Testing

**MUST:** Use `tmp_path` Fixture

```python
def test_write_file(tmp_path):
    """Test writing to a file"""
    file_path = tmp_path / "output.txt"
    write_data(file_path, "test content")
    assert file_path.read_text() == "test content"
```

### Mocking (optional)

```python
from unittest.mock import Mock, patch

def test_api_call_with_mock():
    """Test API call with mocked response"""
    with patch('requests.get') as mock_get:
        mock_get.return_value.json.return_value = {"data": "test"}
        result = fetch_data_from_api()
        assert result == {"data": "test"}
```

### Coverage

**SHOULD:** Aim for reasonable coverage (projektspezifisch)

```bash
# Generate coverage report
pytest --cov=src --cov-report=html tests/

# View report
open htmlcov/index.html
```

**Bei Komplexität:** User fragen ob Coverage ausreichend ist

## Definition of Done (Beispiel)

```bash
# 1. Formatierung
ruff format .

# 2. Linting
ruff check .

# 3. Tests
pytest -v

# 4. Optional: Coverage
pytest --cov=src tests/

# Bei Fehler: Iterieren bis alle grün!
```

## Config
- Konfiguration in `pyproject.toml`:
  - `tool.ruff`
  - `tool.ruff.format`
  - `tool.pytest.ini_options`

---

## Security

Siehe auch: [standard/error-handling-security.md](../standard/error-handling-security.md)

### Code Execution

**MUST NOT:**
- `eval(user_input)` oder `exec(user_input)`
- `pickle.loads()` mit untrusted data
- `subprocess.run(..., shell=True)` mit User-Input
- `__import__(user_input)`

**MUST:**
- `ast.literal_eval()` statt `eval()` für sichere Data-Parsing
- `subprocess.run()` mit List-Arguments (nicht shell=True)
- JSON/YAML für Data, nicht pickle bei untrusted sources

**Beispiel:**
```python
# ❌ FALSCH
result = eval(user_input)
subprocess.run(f"ls {user_path}", shell=True)

# ✅ RICHTIG
import ast
result = ast.literal_eval(user_input)  # Nur für literals
subprocess.run(["ls", user_path])       # Kein shell=True
```

### SQL Injection Prevention

**MUST:**
- Parameterized Queries (Prepared Statements)
- ORM mit Parameter-Binding (SQLAlchemy, Django ORM)

**MUST NOT:**
- String-Interpolation für SQL-Queries
- F-Strings mit User-Input für SQL

**Beispiel:**
```python
# ❌ FALSCH
cursor.execute(f"SELECT * FROM users WHERE id={user_id}")
cursor.execute("SELECT * FROM users WHERE id=" + user_id)

# ✅ RICHTIG
cursor.execute("SELECT * FROM users WHERE id=?", (user_id,))  # sqlite3
cursor.execute("SELECT * FROM users WHERE id=%s", (user_id,))  # psycopg2
```

### File Operations

**MUST:**
- Path-Validierung (kein ../, keine absoluten Pfade wenn nicht nötig)
- `os.path.join()` oder `pathlib.Path` für Pfade
- Permission-Checks vor File-Access

**MUST NOT:**
- Unsanitized Pfade aus User-Input
- `open(user_path)` ohne Validierung
- String-Concatenation für Pfade

**Beispiel:**
```python
# ❌ FALSCH
with open(f"/data/{user_file}") as f:  # Path Traversal möglich
    content = f.read()

# ✅ RICHTIG
import os
from pathlib import Path

base_dir = Path("/data")
user_file = Path(user_input).name  # Nur Filename, kein Path

safe_path = base_dir / user_file
if not safe_path.resolve().is_relative_to(base_dir.resolve()):
    raise ValueError("Invalid path: path traversal detected")

with open(safe_path) as f:
    content = f.read()
```

### Secrets & Credentials

**MUST NOT:**
- Hardcodierte API-Keys, Passwords, Tokens
- Secrets in Logs (auch nicht maskiert)

**MUST:**
- Environment-Variablen oder Config-Dateien
- `.env` Files in `.gitignore`
- `python-dotenv` für .env-Loading

**Beispiel:**
```python
# ❌ FALSCH
API_KEY = "sk-abc123def456"

# ✅ RICHTIG
import os
from dotenv import load_dotenv

load_dotenv()
API_KEY = os.getenv("API_KEY")
if not API_KEY:
    raise ValueError("API_KEY environment variable not set")
```

### Dependencies

**SHOULD:**
- Regelmäßig Dependency-Updates
- `pip-audit` für Security-Checks
- Pin-Versionen in `requirements.txt` oder `pyproject.toml`

```bash
# Security-Scan
pip-audit

# oder mit uv
uv pip audit
```

---

## Security Checklist (Python)

1. **MUST:** `ast.literal_eval()` statt `eval()`
2. **MUST:** Parameterized Queries (SQL)
3. **MUST:** `subprocess.run()` mit List (nicht shell=True)
4. **MUST:** Path-Validierung vor File-Ops
5. **MUST NOT:** `pickle.loads()` mit untrusted data
6. **MUST:** Secrets aus Environment-Variablen
7. **SHOULD:** `pip-audit` regelmäßig ausführen
