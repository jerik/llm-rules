# Testing

Minimale Testing-Policy für reproduzierbare und wartbare Tests.

---

## Test-Philosophie

### Was Testing ist

- **Sicherheitsnetz** gegen Regressionen
- **Dokumentation** des erwarteten Verhaltens
- **Voraussetzung** für sichere Refactorings
- **Feedback-Mechanismus** während Entwicklung

### Was Testing NICHT ist

- **Kein Ersatz** für Code-Reviews
- **Keine Garantie** für fehlerfreien Code
- **Kein Selbstzweck** (100% Coverage um jeden Preis)
- **Kein Blocker** wenn Tests zu komplex werden

---

## Test-Pyramide

Empfohlene Balance der Test-Typen:

```
        /\
       /E2E\        Wenige: Teuer, langsam, fragil
      /______\
     /  Int-  \     Einige: Mittel, realitätsnah
    /__egration\
   /   Unit-    \   Viele: Schnell, isoliert, fokussiert
  /______________\
```

**Fokus:** Unit-Tests (schnell, zahlreich) mit einigen Integration-Tests und wenigen E2E-Tests.

---

## Minimale Testing-Regeln

### MUST-Regeln

#### Tests bei Verhaltensänderung

- MUST: Tests anpassen wenn Verhalten sich ändert
- MUST: Neue Tests für neue Features schreiben
- MUST NOT: Tests ignorieren oder auskommentieren
- MUST NOT: Tests mit "später fixen" markieren

**Begründung:** Verhindert unerkannte Regressionen

---

#### Tests im Definition of Done

- MUST: Tests als Teil des DoD ausführen
- MUST: Tests grün vor Commit
- MUST: Bei fehlgeschlagenen Tests iterieren bis grün

**Begründung:** Qualitätsstandard durchsetzen

---

#### Reproduzierbarkeit

- MUST: Tests deterministisch (keine Random-Failures)
- MUST: Tests isoliert (keine Abhängigkeiten untereinander)
- MUST: Tests in beliebiger Reihenfolge ausführbar

**Begründung:** Zuverlässige Test-Suite

---

### SHOULD-Regeln

#### Test-Abdeckung

- SHOULD: Public APIs testen
- SHOULD: Edge Cases testen (Grenzen: 0, null, leer, max)
- SHOULD: Error-Pfade testen (Invalid Input, Exceptions)
- SHOULD: Bug-Fixes mit Regression-Tests absichern

**Begründung:** Minimale Absicherung kritischer Pfade

---

#### Test-Qualität

- SHOULD: Tests lesbar und wartbar halten
- SHOULD: Tests schnell (< 1 Sekunde pro Unit-Test)
- SHOULD: Test-Namen beschreibend (was wird getestet, erwartetes Ergebnis)

**Beispiel:**
```python
# ✅ Beschreibend
def test_divide_by_zero_raises_error():
    ...

# ❌ Nicht beschreibend
def test_divide_3():
    ...
```

---

#### Test-Organisation

- SHOULD: Test-Files parallel zu Source-Files organisieren
- SHOULD: Stack-spezifische Naming-Conventions befolgen

---

### MAY-Regeln

#### Coverage-Metriken

- MAY: Coverage-Reports generieren
- MAY: Coverage-Targets setzen (z.B. > 80%)

**Aber:** Coverage ist kein Qualitäts-Beweis

---

#### TDD (Test-Driven Development)

- MAY: TDD praktizieren (Test-First)
- MAY: Red-Green-Refactor Cycle nutzen

**Aber:** Nicht verpflichtend

---

## Wann ist "genug" getestet?

### Minimale Absicherung

**Mindestens testen:**

1. **Happy Path** (Normal-Fall)
   - Der häufigste, erwartete Pfad

2. **Edge Cases** (Grenzen)
   - Null, leer, Minimum, Maximum
   - Grenzwerte

3. **Error-Pfade**
   - Invalid Input
   - Exceptions
   - Fehlerbehandlung

**Beispiel:**
```python
# Funktion: divide(a, b)

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

**Gut getestet ist, wenn:**
- ✅ Public APIs haben Tests
- ✅ Kritische Business-Logik hat Tests
- ✅ Bug-Fixes haben Regression-Tests
- ✅ Tests sind schnell und deterministisch

**Nicht nötig:**
- ❌ 100% Line-Coverage
- ❌ Tests für jeden Private-Helper
- ❌ Tests für triviale Getter/Setter
- ❌ Tests für Third-Party Libraries

---

## Was NICHT testen

### Triviale Logik

```python
# Kein Test nötig
class User:
    def get_name(self):
        return self.name
```

### Third-Party Libraries

- Nicht die Library selbst testen
- **Aber:** Integration mit Library testen

```python
# ❌ Nicht testen
def test_requests_library_works():
    response = requests.get("https://api.example.com")
    assert response.status_code == 200

# ✅ Testen
def test_our_api_client_handles_errors():
    client = OurAPIClient()
    with pytest.raises(APIError):
        client.fetch_invalid_endpoint()
```

### Framework-Internals

- Nicht testen wie Framework funktioniert
- **Aber:** Testen dass eigene Code korrekt funktioniert

### Private Implementation-Details

- Testen gegen Public API, nicht interne Hilfsmethoden
- **Ausnahme:** Komplexe interne Logik kann extrahiert und getestet werden

---

## Test-Isolation

### Isolation-Prinzip

**MUST:**
- Tests dürfen sich nicht gegenseitig beeinflussen
- Tests müssen in beliebiger Reihenfolge laufen
- Tests müssen parallel ausführbar sein (wo möglich)

### Shared State vermeiden

**❌ FALSCH:**
```python
# Globale Variable wird mutiert
data = []

def test_append():
    data.append(1)
    assert len(data) == 1  # Nur beim ersten Run!

def test_empty():
    assert len(data) == 0  # Schlägt fehl!
```

**✅ RICHTIG:**
```python
def test_append():
    data = []  # Frische Instanz
    data.append(1)
    assert len(data) == 1

def test_empty():
    data = []  # Frische Instanz
    assert len(data) == 0
```

---

## Fixtures & Setup/Teardown

### Fixtures für Wiederverwendung

**Python (pytest):**
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
```

**JavaScript (Jest):**
```javascript
describe('Calculator', () => {
  let calculator;

  beforeEach(() => {
    calculator = new Calculator();  // Fresh instance
  });

  afterEach(() => {
    calculator = null;  // Cleanup
  });

  it('adds numbers', () => {
    expect(calculator.add(2, 3)).toBe(5);
  });
});
```

---

## Komplexität & User-Fragen

### Wenn Tests zu komplex werden

**MUST:** User fragen bei Unsicherheit

**Situationen für Rückfrage:**

1. **Unklar was testen:**
   ```
   f1: Soll ich für diese Helper-Function einen Test schreiben?
   f2: Oder reicht es, sie über die Public API zu testen?
   ```

2. **Mocking zu komplex:**
   ```
   f1: Diese Function hat 5 externe Abhängigkeiten.
   f2: Soll ich alle mocken oder einen Integration-Test schreiben?
   ```

3. **Test dauert lange:**
   ```
   f1: Dieser Test braucht 10 Sekunden (DB-Setup).
   f2: Soll ich das akzeptieren oder optimieren?
   ```

4. **Coverage vs. Aufwand:**
   ```
   f1: Coverage liegt bei 60%. Soll ich mehr Tests schreiben?
   f2: Oder ist das für dieses Modul ausreichend?
   ```

**Prinzip:** Tests sollen helfen, nicht behindern.

---

## Stack-spezifische Details

### Python

**Tools:** pytest, pytest-cov

**Siehe:** [project-templates/python-stack.md](../project-templates/python-stack.md) Testing-Sektion

**Key Points:**
- Test-Files: `test_*.py` oder `*_test.py`
- Test-Functions: `test_*`
- Fixtures für Setup/Teardown
- `tmp_path` für File I/O

---

### Shell Scripting

**Tools:** bats (Bash Automated Testing System)

**Siehe:** [project-templates/shell-scripting.md](../project-templates/shell-scripting.md) Testing-Sektion

**Key Points:**
- Test-Files: `*.bats`
- `@test` Annotation
- `run` Command für Assertions
- `[ "$status" -eq 0 ]` für Exit-Codes

---

### Frontend (JavaScript)

**Tools:** Jest (Unit), Cypress/Playwright (E2E)

**Siehe:** [project-templates/frontend.md](../project-templates/frontend.md) Testing-Sektion

**Key Points:**
- Test-Files: `*.test.js`, `*.spec.js`
- Jest für Unit-Tests
- Cypress/Playwright für E2E
- React Testing Library für React-Components

---

## Test-Typen

### Unit-Tests

**Fokus:** Einzelne Funktionen/Klassen isoliert

**Eigenschaften:**
- Schnell (< 1 Sekunde)
- Keine externen Abhängigkeiten (Mock/Stub)
- Deterministisch

**Beispiel:**
```python
def test_add():
    assert add(2, 3) == 5
```

---

### Integration-Tests

**Fokus:** Zusammenspiel mehrerer Komponenten

**Eigenschaften:**
- Langsamer als Unit-Tests
- Echte Abhängigkeiten (DB, File-System)
- Realistischer

**Beispiel:**
```python
def test_save_user_to_database():
    user = User(name="Alice")
    db.save(user)
    loaded = db.load(user.id)
    assert loaded.name == "Alice"
```

---

### E2E-Tests (End-to-End)

**Fokus:** Komplette User-Flows

**Eigenschaften:**
- Langsamste Tests
- Gesamtes System
- Fragilste (viele Moving Parts)

**Beispiel (Cypress):**
```javascript
it('user can login and see dashboard', () => {
  cy.visit('/login');
  cy.get('input[name="email"]').type('user@example.com');
  cy.get('input[name="password"]').type('password');
  cy.get('button').click();
  cy.url().should('include', '/dashboard');
});
```

---

## Definition of Done (Testing)

### Vor jedem Commit

**MUST:**
1. Alle Tests ausführen
2. Alle Tests grün
3. Bei Rot: Iterieren bis grün

**Stack-spezifische Commands:**

**Python:**
```bash
pytest -v
# Optional: Mit Coverage
pytest --cov=src tests/
```

**Shell:**
```bash
bats test/*.bats
```

**Frontend:**
```bash
npm test                    # Jest
npm run test:e2e           # Cypress (optional)
```

---

## Zusammenfassung

### Top Testing-Regeln

1. **MUST:** Tests anpassen bei Verhaltensänderung
2. **MUST:** Tests grün vor Commit
3. **MUST:** Tests isoliert und deterministisch
4. **SHOULD:** Happy Path + Edge Cases + Error-Pfade testen
5. **SHOULD:** Public APIs testen
6. **Komplexität:** Bei Unsicherheit User fragen

### "Genug" getestet

- ✅ Happy Path covered
- ✅ Edge Cases covered
- ✅ Error-Pfade covered
- ✅ Tests schnell und zuverlässig

### Nicht nötig

- ❌ 100% Coverage
- ❌ Triviale Logik testen
- ❌ Third-Party Libraries testen
- ❌ Private Implementation-Details testen

### Bei Komplexität

**Immer User fragen wenn Tests zu komplex werden oder unklar ist, was/wie getestet werden soll.**
