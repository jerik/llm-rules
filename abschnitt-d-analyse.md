# Abschnitt D – Analyse: Testing-Policy

**Status:** Vorschlag  
**Datum:** 2025-01-17

---

## 1. Zielsetzung

Einführung einer **klaren, minimalen Testing-Policy** als Teil der Standard-Regeln:
- Allgemeine Test-Philosophie
- Minimale Definition of Done für relevante Stacks
- Fokus: Reproduzierbarkeit und minimale Absicherung

**Nicht-Ziel:** Erschöpfende Test-Guidelines oder TDD-Vorschriften

---

## 2. Analyse bestehender Test-Regeln

### Aktuelle Regeln

**standard/code-quality.md:**
```
- MUST: Tests bei Verhaltensänderung anpassen
- MUST NOT: Tests ignorieren oder "later" markieren
```

**Status:** ✅ Grundprinzip vorhanden, aber nicht konkret genug

**Fehlend:**
- Was ist ein "Test"? (Unit, Integration, E2E?)
- Wann ist Testing "ausreichend"?
- Wie testen in verschiedenen Stacks?

---

### Bestehende Stack-spezifische Test-Regeln

**Python (python-stack.md):**
```
- MUST: pytest für Tests verwenden
- SHOULD: tmp_path Fixture für File I/O
```

**Vim Plugin (vim-plugin.md):**
```
- MUST: Headless Tests ausführen
- Assertions: assert_equal(), assert_match(), etc.
```

**Frontend (frontend.md):**
```
- SHOULD: Jest für Unit-Tests
- SHOULD: Cypress oder Playwright für E2E
```

**Shell Scripting (shell-scripting.md):**
```
- SHOULD: bats (Bash Automated Testing System)
```

**Status:** ✅ Stack-spezifische Tools definiert, aber keine übergreifende Policy

---

## 3. Test-Philosophie

### Grundprinzipien (Vorschlag)

**Testing ist:**
- Sicherheitsnetz gegen Regressionen
- Dokumentation des erwarteten Verhaltens
- Voraussetzung für Refactoring-Sicherheit

**Testing ist NICHT:**
- 100% Coverage-Ziel um jeden Preis
- Ersatz für Code-Reviews
- Garantie für fehlerfreien Code

---

### Test-Pyramide

```
        /\
       /E2E\        Wenige: Teuer, langsam, fragil
      /______\
     /  Int-  \     Einige: Mittel, realitätsnah
    /__egration\
   /   Unit-    \   Viele: Schnell, isoliert, fokussiert
  /______________\
```

**Empfehlung:**
- Fokus auf Unit-Tests (schnell, zahlreich)
- Einige Integration-Tests (realitätsnah)
- Wenige E2E-Tests (teuer, aber wertvoll)

---

## 4. Minimale Testing-Policy (übergreifend)

### MUST-Regeln

1. **Tests bei Verhaltensänderung**
   - MUST: Tests anpassen wenn Verhalten ändert
   - MUST: Neue Tests für neue Features
   - MUST NOT: Tests ignorieren oder auskommentieren

2. **Tests im DoD**
   - MUST: Tests als Teil des Definition of Done
   - MUST: Tests grün vor Commit
   - MUST: Bei fehlgeschlagenen Tests iterieren (nicht commiten)

3. **Reproduzierbarkeit**
   - MUST: Tests deterministisch (keine Random-Failures)
   - MUST: Tests isoliert (keine Abhängigkeiten untereinander)
   - MUST: Tests in CI/CD ausführbar

---

### SHOULD-Regeln

1. **Test-Abdeckung**
   - SHOULD: Public APIs testen
   - SHOULD: Edge Cases testen
   - SHOULD: Error-Pfade testen

2. **Test-Qualität**
   - SHOULD: Tests lesbar und wartbar
   - SHOULD: Tests dokumentieren Intention (Given-When-Then)
   - SHOULD: Tests schnell (< 1 Sekunde pro Unit-Test)

3. **Test-Organisation**
   - SHOULD: Test-Files parallel zu Source-Files
   - SHOULD: Test-Naming-Convention (test_*, *_test.py, etc.)

---

### MAY-Regeln

1. **Coverage-Metriken**
   - MAY: Coverage-Reports generieren
   - MAY: Coverage-Targets setzen (z.B. > 80%)
   - Aber: Coverage ist kein Qualitäts-Beweis

2. **TDD (Test-Driven Development)**
   - MAY: TDD praktizieren
   - Aber: Nicht verpflichtend

---

## 5. Stack-spezifische Testing-Policies

### Python

**Tools:**
- MUST: pytest
- SHOULD: pytest-cov für Coverage
- SHOULD: tmp_path Fixture für File I/O

**Test-Struktur:**
```
project/
├── src/
│   └── module.py
└── tests/
    └── test_module.py
```

**Naming:**
- Test-Files: `test_*.py` oder `*_test.py`
- Test-Functions: `test_*`
- Test-Classes: `Test*`

**Beispiel:**
```python
# tests/test_calculator.py
import pytest
from src.calculator import add

def test_add_positive_numbers():
    assert add(2, 3) == 5

def test_add_negative_numbers():
    assert add(-2, -3) == -5

def test_add_zero():
    assert add(0, 5) == 5

def test_add_raises_on_invalid_type():
    with pytest.raises(TypeError):
        add("2", 3)
```

**DoD:**
```bash
pytest -v                    # Alle Tests
pytest --cov=src tests/      # Mit Coverage
```

---

### Shell Scripting

**Tools:**
- SHOULD: bats (Bash Automated Testing System)
- SHOULD: shellcheck für Linting (nicht Testing, aber wichtig)

**Test-Struktur:**
```
project/
├── script.sh
└── test/
    └── script.bats
```

**Beispiel:**
```bash
# test/script.bats
#!/usr/bin/env bats

@test "script returns 0 on success" {
  run ./script.sh --help
  [ "$status" -eq 0 ]
}

@test "script outputs help text" {
  run ./script.sh --help
  [[ "$output" =~ "Usage:" ]]
}

@test "script fails with invalid option" {
  run ./script.sh --invalid
  [ "$status" -ne 0 ]
}
```

**DoD:**
```bash
shellcheck *.sh        # Linting
bats test/*.bats       # Tests
```

---

### Frontend (JavaScript/TypeScript)

**Tools:**
- SHOULD: Jest für Unit-Tests
- SHOULD: Cypress oder Playwright für E2E-Tests
- SHOULD: React Testing Library (für React)

**Test-Struktur:**
```
project/
├── src/
│   ├── component.js
│   └── component.test.js
└── e2e/
    └── app.spec.js
```

**Naming:**
- Test-Files: `*.test.js`, `*.spec.js`

**Beispiel (Jest):**
```javascript
// src/calculator.test.js
import { add } from './calculator';

describe('add', () => {
  it('adds two positive numbers', () => {
    expect(add(2, 3)).toBe(5);
  });

  it('adds negative numbers', () => {
    expect(add(-2, -3)).toBe(-5);
  });

  it('throws on invalid type', () => {
    expect(() => add('2', 3)).toThrow(TypeError);
  });
});
```

**Beispiel (Cypress E2E):**
```javascript
// e2e/login.spec.js
describe('Login', () => {
  it('logs in successfully', () => {
    cy.visit('/login');
    cy.get('input[name="email"]').type('user@example.com');
    cy.get('input[name="password"]').type('password123');
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
  });
});
```

**DoD:**
```bash
npm test              # Jest Unit-Tests
npm run e2e           # Cypress E2E-Tests
```

---

## 6. Was NICHT getestet werden muss

### Nicht testen (in der Regel)

1. **Triviale Getter/Setter**
   ```python
   # Kein Test nötig
   def get_name(self):
       return self.name
   ```

2. **Third-Party Libraries**
   - Nicht die Library selbst testen
   - Aber: Integration mit Library testen

3. **Framework-Internals**
   - Nicht testen wie React rendert
   - Aber: Testen dass eigene Komponente korrekt reagiert

4. **Private Implementation-Details**
   - Testen gegen Public API, nicht interne Hilfsmethoden
   - Aber: Wenn komplexe Logik, extrahieren und testen

---

## 7. Wann ist "genug" getestet?

### Minimale Absicherung

**Mindestens testen:**
- ✅ Happy Path (Normal-Fall)
- ✅ Edge Cases (Grenzen: 0, null, leer, max)
- ✅ Error-Pfade (Invalid Input, Exceptions)

**Beispiel:**
```python
# Funktion: divide(a, b)
# Minimal-Tests:
def test_divide_positive():        # Happy Path
    assert divide(10, 2) == 5

def test_divide_zero_numerator():  # Edge Case
    assert divide(0, 5) == 0

def test_divide_by_zero_raises():  # Error Path
    with pytest.raises(ZeroDivisionError):
        divide(10, 0)
```

---

### Ausreichende Absicherung

**Gut getestet ist:**
- Public APIs haben Tests
- Kritische Logik hat Tests (Business-Rules)
- Bug-Fixes haben Regression-Tests
- Tests sind schnell und deterministisch

**Nicht nötig:**
- 100% Line-Coverage
- Tests für jeden Private-Helper
- Tests für triviale Logik

---

## 8. Test-Isolation & Fixtures

### Isolation-Prinzip

**MUST:**
- Tests dürfen sich nicht gegenseitig beeinflussen
- Tests müssen in beliebiger Reihenfolge laufen können
- Tests müssen parallel ausführbar sein (wo möglich)

**Beispiel (Python):**
```python
import pytest

@pytest.fixture
def temp_file(tmp_path):
    """Isoliertes Temp-File pro Test"""
    file = tmp_path / "test.txt"
    file.write_text("initial content")
    yield file
    # Cleanup automatisch durch tmp_path

def test_read_file(temp_file):
    content = temp_file.read_text()
    assert content == "initial content"

def test_write_file(temp_file):
    temp_file.write_text("new content")
    assert temp_file.read_text() == "new content"
```

---

### Shared State vermeiden

**❌ FALSCH:**
```python
# Globale Variable wird mutiert
data = []

def test_append():
    data.append(1)
    assert len(data) == 1  # Funktioniert nur beim ersten Run!

def test_empty():
    assert len(data) == 0  # Schlägt fehl wenn test_append vorher lief!
```

**✅ RICHTIG:**
```python
def test_append():
    data = []  # Frische Instanz pro Test
    data.append(1)
    assert len(data) == 1

def test_empty():
    data = []  # Frische Instanz pro Test
    assert len(data) == 0
```

---

## 9. Vorschlag: Testing-Policy-Dokument

### Struktur

**Datei:** `standard/testing.md`

**Inhalt:**
1. Test-Philosophie (Sicherheitsnetz, nicht Garantie)
2. Test-Pyramide (Unit > Integration > E2E)
3. Minimale MUST-Regeln (Tests anpassen, DoD, Reproduzierbarkeit)
4. SHOULD-Regeln (Coverage, Qualität, Organisation)
5. Was NICHT testen (Trivial, Third-Party, Private)
6. Wann ist "genug" (Happy Path + Edge Cases + Errors)
7. Test-Isolation & Fixtures
8. Stack-spezifische Verweise

---

## 10. Stack-Template-Ergänzungen

### Zu ergänzen in Templates

**python-stack.md:**
- Testing-Sektion erweitern
- pytest Best Practices
- Fixture-Beispiele
- Coverage-Integration

**shell-scripting.md:**
- bats-Beispiele erweitern
- Test-Edge-Cases

**frontend.md:**
- Jest Best Practices erweitern
- Cypress/Playwright Patterns
- Testing-Library Patterns (React, Vue)

---

## 11. Offene Fragen

### Entscheidungsbedarf

1. **Coverage-Targets:**
   - Sollen konkrete Coverage-Targets definiert werden (z.B. > 80%)?
   - Oder als MAY belassen (projektspezifisch)?

2. **TDD-Empfehlung:**
   - Soll TDD explizit empfohlen werden (SHOULD)?
   - Oder neutral lassen (MAY)?

3. **Test-Dokumentations-Stil:**
   - Soll ein spezifischer Stil empfohlen werden (Given-When-Then, Arrange-Act-Assert)?
   - Oder offen lassen?

4. **CI/CD-Integration:**
   - Sollen CI/CD-Best-Practices Teil der Testing-Policy sein?
   - Oder separates Dokument?

5. **Performance-Tests:**
   - Sollen Performance/Load-Tests erwähnt werden?
   - Oder Fokus nur auf Functional-Tests?

---

## 12. Vorschlag für plan.md Update

### Status Abschnitt D

```markdown
## Abschnitt D – Testing-Policy (Punkt 6)

Status: Analyse abgeschlossen, Vorschlag erstellt

### Durchgeführt
- Analyse bestehender Test-Regeln (code-quality.md, Templates)
- Entwurf Test-Philosophie (Sicherheitsnetz, Test-Pyramide)
- Definition minimaler MUST/SHOULD/MAY-Regeln
- Stack-spezifische Testing-Patterns (Python, Shell, Frontend)
- Isolation & Fixtures-Prinzipien
- "Genug getestet" Kriterien

### Output
- abschnitt-d-analyse.md (diese Datei)

### Offene Entscheidungen
1. Coverage-Targets definieren oder projektspezifisch?
2. TDD explizit empfehlen oder neutral?
3. Test-Dokumentations-Stil festlegen?
4. CI/CD-Integration Teil der Policy?
5. Performance-Tests erwähnen?

### Nächste Schritte
- Entscheidungen durch Stakeholder einholen
- Nach Freigabe: Dokumentation erstellen (standard/testing.md)
- Templates ergänzen
```

---

## 13. Zusammenfassung

**Test-Philosophie:**
- Sicherheitsnetz gegen Regressionen
- Nicht 100% Coverage-Ziel
- Fokus auf Public APIs und kritische Logik

**Minimale Policy:**
- MUST: Tests anpassen bei Änderungen
- MUST: Tests grün vor Commit
- MUST: Tests reproduzierbar und isoliert

**Stack-spezifisch:**
- Python: pytest
- Shell: bats
- Frontend: Jest + Cypress/Playwright

**"Genug" getestet:**
- Happy Path + Edge Cases + Error-Pfade
- Public APIs + kritische Logik
- Bug-Fixes mit Regression-Tests

**Nächster Schritt:**
- Entscheidung über offene Fragen
- Danach: Implementierung `standard/testing.md`

---

**Status:** Vorschlag bereit für Review
