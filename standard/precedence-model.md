# Präzedenz- & Konfliktmodell

Explizites Modell zur Regelpriorität und Konfliktauflösung innerhalb der llm-rules.

---

## Zweck

Dieses Dokument definiert:
- **Präzedenz-Hierarchie:** Welche Regel gilt bei Überschneidungen?
- **Konfliktauflösung:** Wie mit widersprüchlichen Regeln umgehen?
- **Zulässige Überschreibungen:** Wann dürfen Regeln überschrieben werden?

---

## Präzedenz-Hierarchie

### Ebenen (höchste zuerst)

```
1. Standard-Regeln (standard/)
   ↓
2. Template-Regeln (project-templates/)
   ↓
3. Skill-Regeln (skills/)
   ↓
4. Projekt-Regeln (projektspezifische Dateien)
```

### Grundprinzip

**Regel höherer Ebene schlägt Regel niedrigerer Ebene**

**Ausnahme:** Projekt-Regeln können mit expliziter **EXCEPTION** überschreiben

---

## MUST / SHOULD / MAY Semantik

Nach RFC 2119:

| Keyword | Bedeutung | Überschreibbar? |
|---------|-----------|-----------------|
| **MUST** | Zwingend erforderlich | Nur mit EXCEPTION |
| **MUST NOT** | Absolut verboten | Nur mit EXCEPTION |
| **SHOULD** | Dringend empfohlen | Ja, mit Begründung |
| **SHOULD NOT** | Nicht empfohlen | Ja, mit Begründung |
| **MAY** | Optional | Immer |

### Präzedenz bei gleicher Ebene

Wenn in gleicher Datei:
- **MUST** überschreibt **SHOULD**
- **SHOULD** überschreibt **MAY**

Wenn in verschiedenen Dateien gleicher Ebene:
- Konflikt → User fragen

---

## Zulässige Operationen

### 1. Präzisierung (erlaubt)

Niedrigere Ebenen **DÜRFEN** höhere Ebenen präzisieren.

**Beispiel: Standard → Template**

**Standard (code-quality.md):**
```markdown
### Dokumentation
- MUST: Docstrings für öffentliche APIs
```

**Template (python-stack.md):**
```markdown
### Dokumentation
- MUST: Docstrings für öffentliche APIs
- MUST: Docstrings im Google-Style-Format
- SHOULD: Type Hints in Docstrings
```

**Status:** ✅ **Erlaubt** (Präzisierung + Ergänzung, kein Widerspruch)

---

**Beispiel: Standard → Projekt**

**Standard (code-quality.md):**
```markdown
### Diff-Größe
- SHOULD: Kleine, reviewbare Diffs bevorzugen
```

**Projekt (PROJECT-RULES.md):**
```markdown
### Diff-Größe
- SHOULD: Diffs max. 200 Zeilen
```

**Status:** ✅ **Erlaubt** (Konkretisierung)

---

### 2. Widerspruch (verboten)

Niedrigere Ebenen **DÜRFEN NICHT** höheren Ebenen widersprechen.

**Beispiel: Standard → Template (VERBOTEN)**

**Standard (git-workflow.md):**
```markdown
### Sprache
- MUST: Code und Kommentare nur in Englisch
```

**Template (python-stack.md):**
```markdown
### Sprache
- MAY: Code in beliebiger Sprache
```

**Status:** ❌ **Verboten** (Widerspruch zu MUST)

---

**Beispiel: Template → Skill (VERBOTEN)**

**Template (python-stack.md):**
```markdown
### Testing
- MUST: pytest für Tests verwenden
```

**Skill (hypothetisch):**
```markdown
### Testing
- MUST: unittest verwenden
```

**Status:** ❌ **Verboten** (Tool-Entscheidungen gehören ins Template, nicht in Skills)

---

### 3. Explizite Überschreibung (Projekt-Ebene)

Projekt-Regeln **KÖNNEN** mit **EXCEPTION** überschreiben.

#### EXCEPTION-Syntax

```markdown
# EXCEPTION: <Quelle> | Begründung: <Text>

[Überschreibende Regel]
```

**Erforderliche Elemente:**
- `EXCEPTION:` Keyword
- Quelle (welche Regel wird überschrieben)
- Begründung (warum ist Überschreibung nötig)

---

#### Beispiel: Explizite Projekt-Exception

**Standard (git-workflow.md):**
```markdown
### Sprache
- MUST: Commits nur in Englisch
```

**Projekt (PROJECT-RULES.md):**
```markdown
# EXCEPTION: standard/git-workflow.md | Begründung: Internes Team, alle deutschsprachig

### Git Workflow
- MUST: Commits in Deutsch erlaubt
```

**Status:** ⚠️ **Zulässig** (explizite, begründete Exception)

---

## Konfliktauflösungs-Strategie

### Ablauf bei erkanntem Konflikt

Das LLM MUSS:

#### 1. Konflikt identifizieren

Erkenne widersprüchliche Regeln:
- Welche Regeln widersprechen sich?
- Auf welcher Ebene liegen sie?
- Sind beide MUST oder MUST vs. SHOULD?

#### 2. Präzedenz anwenden

**Regel höherer Ebene hat Vorrang**

Beispiel:
- Standard: "MUST: Commits in Englisch"
- Template: "MAY: Commits in Deutsch"
- **Ergebnis:** Standard gewinnt → Commits in Englisch

**Ausnahme:** Explizite EXCEPTION auf Projekt-Ebene

#### 3. Bei Unsicherheit: User fragen

Wenn Konflikt nicht eindeutig auflösbar:
- Konflikt dem User melden
- Fragen stellen (f1, f2, ...)
- **NICHT raten oder implizit entscheiden**

---

### Beispiel-Ablauf

**Konflikt erkannt:**
```
Standard (git-workflow.md): 
  "MUST: Commits nur in Englisch"

Projekt (PROJECT-RULES.md): 
  "Commits in Deutsch erlaubt" (ohne EXCEPTION)
```

**LLM-Verhalten:**
```
KONFLIKT: Widersprüchliche Regeln erkannt

Konflikt:
- standard/git-workflow.md: "MUST: Commits nur in Englisch"
- PROJECT-RULES.md: "Commits in Deutsch erlaubt"

Die Projekt-Regel widerspricht der Standard-Regel, 
aber ist nicht als EXCEPTION markiert.

f1: Soll die Projekt-Regel die Standard-Regel überschreiben?
f2: Falls ja, bitte als EXCEPTION mit Begründung dokumentieren.
```

---

## Konflikt-Reporting

### Automatisches Logging

Das LLM MUST Konflikte automatisch loggen:

**Datei:** `.llm-rules-conflicts.log` (im Projekt-Root)

**Format:**
```
[YYYY-MM-DD HH:MM:SS] CONFLICT DETECTED
Source 1: standard/git-workflow.md
  Rule: "MUST: Commits nur in Englisch"
Source 2: PROJECT-RULES.md (line 42)
  Rule: "Commits in Deutsch erlaubt"
Status: UNRESOLVED
Action: Asked user for clarification (f1, f2)

---
```

### User-Meldung

Das LLM MUST den Konflikt auch direkt dem User melden (siehe Beispiel oben).

---

## Orthogonalitätsprinzip für Skills

### Design-Anforderung

Skills **MÜSSEN** orthogonal designed werden:
- Keine widersprüchlichen Regeln zwischen Skills
- Keine Tool-Entscheidungen (gehört ins Template)
- Keine Überschneidungen in Workflow-Schritten

### Begründung

Skills sind optional und in Kombination aktivierbar.
Wenn zwei Skills widersprüchliche MUST-Regeln haben,
entsteht ein unauflösbarer Konflikt.

---

### Beispiel: Gutes Design (orthogonal)

**llm-rules.yml:**
```yaml
skills:
  - workflow-standard
  - fortschritt-tracking
  - synergie-analyse
```

**workflow-standard.md:**
- Definiert: Workflow-Schritte (Doku lesen, Dialog, DoD, Review)

**fortschritt-tracking.md:**
- Definiert: Tracking-Mechanismen (Checkboxen, Zusammenfassungen)

**synergie-analyse.md:**
- Definiert: Backlog-Konsultation, Zusammenhänge erkennen

**Status:** ✅ **Orthogonal** (keine Überschneidung, gemeinsam aktivierbar)

---

### Beispiel: Schlechtes Design (nicht orthogonal)

**Hypothetisch:**

**skill-a.md:**
```markdown
### DoD
- MUST: pytest mit Coverage-Report ausführen
```

**skill-b.md:**
```markdown
### DoD
- MUST: unittest ohne Coverage ausführen
```

**Problem:** Beide Skills definieren widersprüchliche Tool-Entscheidungen.

**Status:** ❌ **Nicht orthogonal** (Konflikt bei gemeinsamer Aktivierung)

---

### Lösung: Tool-Entscheidungen ins Template

Tool-Entscheidungen (pytest vs. unittest) gehören ins **Template**, nicht in Skills.

**Korrektur:**

**Template (python-stack.md):**
```markdown
### Testing
- MUST: pytest verwenden
```

**Skill (workflow-standard.md):**
```markdown
### DoD
- MUST: DoD vollständig ausführen (projektspezifisch definiert)
- MUST: Tests ausführen (Tool definiert durch Template)
```

**Status:** ✅ **Korrekt** (Skill referenziert Template-Tool, kein Konflikt)

---

### Validierung

**Status:** Skill-Orthogonalität wird **nicht automatisch validiert**.

**Verantwortung:** Skill-Entwickler müssen Orthogonalität sicherstellen.

**Bei Konflikt:** LLM meldet zur Laufzeit (siehe Konfliktauflösungs-Strategie).

---

## Verhalten bei unauflösbaren Konflikten

### Definition: Unauflösbar

Ein Konflikt ist unauflösbar, wenn:
- Zwei MUST-Regeln widersprechen sich
- Beide auf gleicher Ebene (z.B. zwei Skills)
- Keine klare Präzedenz existiert
- Keine explizite EXCEPTION vorhanden

---

### LLM-Verhalten

#### 1. Konflikt melden

```
KONFLIKT: Unauflösbare Regel-Widersprüche erkannt

Konflikt 1:
- skill-a.md: "MUST: pytest verwenden"
- skill-b.md: "MUST: unittest verwenden"

Diese Skills sind nicht orthogonal und sollten nicht 
gemeinsam aktiviert werden.

f1: Welchen Skill soll ich deaktivieren?
f2: Oder soll die Regel in einem der Skills angepasst werden?
```

#### 2. Nicht fortfahren

Das LLM **MUST NOT** fortfahren ohne Klärung.

#### 3. Konflikt loggen

Konflikt in `.llm-rules-conflicts.log` dokumentieren.

---

## Beispiele

### Beispiel 1: Standard schlägt Template

**Standard (communication.md):**
```markdown
### Frageformat
- MUST: Fragen mit f1, f2, ... fn nummerieren
```

**Template (python-stack.md):**
```markdown
### Frageformat
- SHOULD: Fragen mit Q1, Q2, ... Qn nummerieren
```

**Präzedenz:** Standard (MUST) schlägt Template (SHOULD)

**Ergebnis:** f1, f2, ... fn wird verwendet

---

### Beispiel 2: Template präzisiert Standard

**Standard (code-quality.md):**
```markdown
### Tests
- MUST: Tests bei Verhaltensänderung anpassen
```

**Template (python-stack.md):**
```markdown
### Tests
- MUST: pytest für Tests verwenden
- MUST: Tests bei Verhaltensänderung anpassen
- SHOULD: Coverage > 80%
```

**Präzedenz:** Keine Widerspruch, Template präzisiert

**Ergebnis:** 
- pytest verwenden
- Tests bei Verhaltensänderung anpassen
- Coverage > 80% anstreben

---

### Beispiel 3: Projekt überschreibt mit EXCEPTION

**Standard (git-workflow.md):**
```markdown
### Branch-Naming
- MUST: Feature Branches: claude/<feature-name>
```

**Projekt (CLAUDE.md):**
```markdown
# EXCEPTION: standard/git-workflow.md | Begründung: Legacy-Konvention, bestehende Branches

### Branch-Naming
- MUST: Feature Branches: feature/<feature-name>
```

**Präzedenz:** EXCEPTION erlaubt Überschreibung

**Ergebnis:** feature/<feature-name> wird verwendet

**LLM-Verhalten:**
- Akzeptiert EXCEPTION (explizit und begründet)
- Loggt in `.llm-rules-conflicts.log`
- Informiert User über abweichende Konvention

---

### Beispiel 4: Skills orthogonal kombinieren

**llm-rules.yml:**
```yaml
skills:
  - workflow-standard
  - fortschritt-tracking
```

**workflow-standard.md:**
- MUST: Dokumentation lesen
- MUST: Dialog führen
- MUST: DoD ausführen

**fortschritt-tracking.md:**
- SHOULD: Checkboxen aktualisieren
- SHOULD: Zusammenfassung erstellen

**Präzedenz:** Kein Konflikt (orthogonal)

**Ergebnis:** Beide Skills werden angewendet

---

### Beispiel 5: Unauflösbarer Skill-Konflikt

**Hypothetisch:**

**llm-rules.yml:**
```yaml
skills:
  - workflow-a
  - workflow-b
```

**workflow-a.md:**
```markdown
- MUST: USER-STORY.md lesen
```

**workflow-b.md:**
```markdown
- MUST: FEATURE.md lesen (statt USER-STORY.md)
```

**Präzedenz:** Beide auf gleicher Ebene, beide MUST

**Ergebnis:** Unauflösbar

**LLM-Verhalten:**
```
KONFLIKT: Unauflösbarer Widerspruch zwischen Skills

- workflow-a.md: "MUST: USER-STORY.md lesen"
- workflow-b.md: "MUST: FEATURE.md lesen (statt USER-STORY.md)"

Diese Skills sind nicht orthogonal.

f1: Welchen Skill soll ich deaktivieren?
f2: Oder sollen beide Dateien gelesen werden?
```

---

## Zusammenfassung

### Präzedenz-Hierarchie

```
Standard > Template > Skills > Projekt (mit EXCEPTION)
```

### Grundregeln

1. **Höhere Ebene schlägt niedrigere Ebene**
2. **Präzisierung erlaubt, Widerspruch verboten**
3. **Projekt kann mit EXCEPTION überschreiben** (explizit + begründet)
4. **Skills müssen orthogonal sein** (keine Tool-Entscheidungen)
5. **Bei Konflikt: User fragen, nicht raten**

### MUST/SHOULD/MAY

- **MUST:** Zwingend (nur mit EXCEPTION überschreibbar)
- **SHOULD:** Empfohlen (mit Begründung überschreibbar)
- **MAY:** Optional (immer überschreibbar)

### Konfliktauflösung

1. Konflikt identifizieren
2. Präzedenz anwenden
3. Konflikt loggen (`.llm-rules-conflicts.log`)
4. User informieren
5. Bei Unsicherheit: User fragen

### EXCEPTION-Format

```markdown
# EXCEPTION: <Quelle> | Begründung: <Text>
```

**Pflicht-Elemente:**
- EXCEPTION-Keyword
- Quelle (welche Regel überschrieben wird)
- Begründung (warum nötig)
