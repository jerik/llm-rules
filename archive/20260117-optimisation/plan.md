# Plan - Inkrementelle Härtung der llm-rules (Punkte 3-6)

## Zieldefinition (verbindlich)

Ziel dieses Vorhabens ist die gezielte, inkrementelle Verbesserung der bestehenden
llm-rules in vier klar abgegrenzten Bereichen:

3) Expliziter Aktivierungsmechanismus für llm-rules
4) Präzedenz- und Konfliktmodell
5) Operationalisierte Security- & Safety-Regeln
6) Explizite Testing-Policy (standard + stack-spezifisch)

Der Fokus liegt auf einer soliden, konsistenten Ausgangsbasis.
Vollständigkeit oder maximale Abdeckung sind ausdrücklich nicht Ziel.

Alle Arbeiten beziehen sich ausschließlich auf den aktuellen Stand der llm-rules.
Archivierte oder historische Regeldateien sind explizit nicht Teil des Betrachtungsraums.

---

## Nicht-Ziele

- Keine Analyse oder Nutzung archivierter Prompts oder früherer Projektdateien
- Keine Rekonstruktion historischer Aktivierungslogiken
- Keine vollständige Neustrukturierung des Repositories
- Keine Tool-, Anbieter- oder Modellabhängigkeit

---

## Aktueller Status

- Phase: Alle 4 Abschnitte vollständig abgeschlossen ✅
- Härtungsphase (Punkte 3-6): Abgeschlossen
- Betrachtungsraum: ausschließlich aktuelles llm-rules Repository
- Letzte Aktualisierung: 2025-01-17
- Status: llm-rules v1.1.0 bereit

---

## Arbeitsprinzipien (zusätzlich verbindlich)

- Änderungen sind additive oder präzisierende Ergänzungen
- Bestehende Regeln behalten ihre Gültigkeit
- Neue Regeln dürfen bestehende nicht implizit überschreiben
- Jede Ergänzung muss ihren Zweck explizit benennen

---

## Arbeitsabschnitte

---

## Abschnitt A - Expliziter Aktivierungsmechanismus (Punkt 3)

### Ziel
Entwurf eines **klaren, deklarativen Aktivierungsmechanismus**, der eindeutig festlegt,
welche Teile der llm-rules in einem Projekt gelten.

Der Mechanismus dient ausschließlich der **zukünftigen Nutzung** und wird nicht aus
historischen Dateien abgeleitet.

### Aufgaben
- Ableitung der benötigten Aktivierungsdimensionen aus den bestehenden llm-rules:
  - Standard-Regeln
  - Skills
  - Templates
- Entwurf eines minimalen, menschen- und maschinenlesbaren Formats
- Definition klarer Leseregeln (keine implizite Interpretation)

### Explizite Ausschlüsse
- Keine Analyse archivierter Dateien
- Keine Ableitung aus alten CLAUDE.md oder ähnlichen Artefakten
- Keine Rückwärtskompatibilität

### Output
- Vorschlag für ein Activation-Manifest-Format
- Dokumentierte Semantik der Felder
- Kennzeichnung als empfohlene Struktur

### Status
**Abgeschlossen:** Analyse und Vorschlag erstellt (2025-01-17)

### Durchgeführt
- ✅ Analyse der bestehenden llm-rules Struktur (standard/, skills/, project-templates/, context/)
- ✅ Ableitung der Aktivierungsdimensionen
- ✅ Entwurf YAML-basiertes Activation Manifest Format
- ✅ Dokumentation der Semantik aller Felder (version, template, skills, environment, project)
- ✅ Definition von Leseregeln und Validierungslogik
- ✅ Identifikation offener Entscheidungspunkte

### Ergebnis
- **Datei:** abschnitt-a-analyse.md
- **Format-Vorschlag:** YAML-Datei `llm-rules.yml` im Projekt-Root
- **Kernfelder:**
  - `version` (REQUIRED): Manifest-Version
  - `template` (REQUIRED): Stack-Template (python-stack | vim-plugin | keypirinha-plugin)
  - `skills` (OPTIONAL): Liste aktivierter Skills
  - `environment` (OPTIONAL): pi | claude-on-web
  - `project` (OPTIONAL): Metadaten

### Entscheidungen getroffen (2025-01-17)
1. **Format-Wahl:** ✅ YAML
2. **Dateiname:** ✅ `llm-rules.yml`
3. **Dokumentations-Platzierung:** ✅ `standard/activation-manifest.md`
4. **Rückwärtskompatibilität:** ✅ Nicht nötig (kein altes Beispiel vorhanden)
5. **Fehlendes Manifest:** ✅ Fallback zu Defaults

### Implementiert (2025-01-17)
- ✅ `standard/activation-manifest.md` erstellt
  - Vollständige Spezifikation des YAML-Formats
  - Semantik aller Felder (version, template, skills, environment, project)
  - Leseregeln und Fehlerbehandlung
  - Fallback zu Defaults bei fehlendem Manifest
  - Beispiele für alle Templates
- ✅ `README.md` aktualisiert
  - Activation Manifest in Standard-Regeln aufgenommen
  - Nutzungs-Abschnitt mit YAML-Beispielen ersetzt
  - Alte Markdown-Beispiele entfernt

### Abschnitt A: Abgeschlossen ✅

---

## Abschnitt B - Präzedenz- & Konfliktmodell (Punkt 4)

### Ziel
Festlegung eines expliziten, leicht verständlichen Modells zur Regelpriorität
und zum Umgang mit Konflikten innerhalb der llm-rules.

### Aufgaben
- Definition einer klaren Prioritätshierarchie
- Beschreibung zulässiger Überschreibungen
- Festlegung, wann ein Konflikt:
  - aufzulösen ist
  - oder als Entscheidungsbedarf zu markieren ist

### Output
- Kurzes, normatives Präzedenzmodell
- Klare Do-/Don't-Regeln für Konfliktsituationen

### Status
**Abgeschlossen:** Analyse und Vorschlag erstellt (2025-01-17)

### Durchgeführt
- ✅ Analyse bestehender Regel-Ebenen (Standard, Template, Skills, Projekt)
- ✅ Identifikation von 5 typischen Konfliktszenarien
- ✅ Entwurf Präzedenz-Hierarchie: Standard > Template > Skills > Projekt
- ✅ Definition Präzisierungs- vs. Überschreibungs-Regeln
- ✅ EXCEPTION-Mechanismus für explizite Projekt-Überschreibungen
- ✅ Orthogonalitätsprinzip für Skills
- ✅ Konfliktauflösungs-Strategie dokumentiert
- ✅ MUST/SHOULD/MAY-Semantik nach RFC 2119

### Ergebnis
- **Datei:** abschnitt-b-analyse.md
- **Präzedenz-Hierarchie:** Standard > Template > Skills > Projekt
- **Grundprinzipien:**
  - Höhere Ebene schlägt niedrigere Ebene
  - Präzisierung erlaubt, Widerspruch nicht
  - Projekt kann mit EXCEPTION überschreiben (explizit + begründet)
  - Skills müssen orthogonal sein
  - Bei Konflikt: User fragen

### Entscheidungen getroffen (2025-01-17)
1. **EXCEPTION-Syntax:** ✅ Standardisiertes Format: `# EXCEPTION: <Quelle> | Begründung: <Text>`
2. **Konflikt-Reporting:** ✅ Automatisch loggen UND melden
3. **Skill-Orthogonalität:** ✅ Nur als Design-Prinzip dokumentieren (keine Validierung)

### Implementiert (2025-01-17)
- ✅ `standard/precedence-model.md` erstellt
  - Präzedenz-Hierarchie: Standard > Template > Skills > Projekt
  - MUST/SHOULD/MAY-Semantik nach RFC 2119
  - Präzisierung erlaubt, Widerspruch verboten
  - EXCEPTION-Mechanismus: `# EXCEPTION: <Quelle> | Begründung: <Text>`
  - Konfliktauflösungs-Strategie mit automatischem Logging
  - Orthogonalitätsprinzip für Skills (als Design-Regel)
  - Beispiele für alle Konflikttypen
- ✅ `README.md` aktualisiert (Precedence Model in Standard-Regeln)

### Abschnitt B: Abgeschlossen ✅

---

## Abschnitt C - Security & Safety (Punkt 5)

### Ziel
Konkretisierung bestehender Security-Grundsätze in operatives Agentenverhalten
für die relevanten Stacks:
- Python
- Shell / Linux
- Windows CMD / MS-DOS
- Frontend (html/js/css)

### Aufgaben
- Identifikation häufiger, realer Risikoszenarien
- Ableitung weniger, aber harter Verhaltensregeln (MUST / MUST NOT)
- Trennung zwischen generischen und stack-spezifischen Regeln

### Output
- Ergänzungen zu bestehenden Security-Regeln
- Fokus auf praktische Risikominimierung

### Status
**Abgeschlossen:** Analyse und Vorschlag erstellt (2025-01-17)

### Durchgeführt
- ✅ Analyse bestehender Security-Regeln (standard/error-handling.md)
- ✅ Identifikation übergreifender Risiken (Secrets, Command Injection, Destructive Ops, Path Traversal)
- ✅ Stack-spezifische Risiken identifiziert (Python: 5, Shell: 5, Windows: 4, Frontend: 5)
- ✅ Operationalisierte Regeln mit Code-Beispielen entworfen
- ✅ Strukturvorschlag: Zentral in einem Dokument

### Ergebnis
- **Datei:** abschnitt-c-analyse.md
- **Risiko-Schwerpunkte:** Hardcoded Secrets, Command Injection, Destructive Ops, XSS, SQL Injection
- **Regeln für:** Generisch + Python + Shell/Linux + Windows CMD + Frontend

### Entscheidungen getroffen (2025-01-17)
1. **Dokumenten-Struktur:** ✅ Ein zentrales `standard/security-safety.md`
2. **error-handling.md:** ✅ Erweitern um Security-Details
3. **Stack-Coverage:** ✅ 4 Stacks (Python, Shell/Linux, Windows CMD, Frontend) sind ausreichend
4. **Neue Templates:** ✅ Erstellen: `project-templates/shell-scripting.md` und `project-templates/frontend.md`

### Implementiert (2025-01-17)
- ✅ `standard/error-handling.md` erweitert → `standard/error-handling-security.md`
  - Generische Security-Regeln (Secrets, Command Injection, Destructive Ops, Path Traversal)
  - Security Checklist für LLM-Agenten
  - Logging Security, Error Messages Security
  - Verweise auf stack-spezifische Details
- ✅ `project-templates/shell-scripting.md` erstellt
  - Shell/Linux (Bash, sh): Quoting, Destructive Commands, Command Injection, sudo, Wildcards
  - Windows CMD: Quoting, Path mit Leerzeichen, del /s /q
  - PowerShell Basics
  - shellcheck Integration
- ✅ `project-templates/frontend.md` erstellt
  - JavaScript: XSS-Prevention, Secrets & API-Keys, Data Storage, eval()
  - HTML: Form Security, Link Security
  - CSS: Best Practices
  - Framework-Specifics (React, Vue, Angular)
  - Security Checklist
- ✅ `project-templates/python-stack.md` erweitert um Security-Sektion
  - Code Execution (eval, exec, subprocess)
  - SQL Injection Prevention
  - File Operations & Path Traversal
  - Secrets & Credentials
  - Dependencies (pip-audit)
- ✅ `README.md` aktualisiert
  - error-handling-security.md in Standard-Regeln
  - shell-scripting.md und frontend.md in Templates

### Abschnitt C: Abgeschlossen ✅

---

## Abschnitt D - Testing-Policy (Punkt 6)

### Ziel
Einführung einer klaren, minimalen Testing-Policy als Bestandteil der Standard-Regeln.

### Aufgaben
- Definition einer allgemeinen Test-Philosophie
- Ableitung einer minimalen Definition of Done für:
  - Python
  - Frontend (html/js/css)
  - Shell / Scripting

### Output
- Zentrales Testing-Grundlagendokument
- Ergänzende, knappe stack-spezifische Regeln

### Status
**Abgeschlossen:** Analyse und Vorschlag erstellt (2025-01-17)

### Durchgeführt
- ✅ Analyse bestehender Test-Regeln (code-quality.md, Templates)
- ✅ Entwurf Test-Philosophie (Sicherheitsnetz, Test-Pyramide)
- ✅ Definition minimaler MUST/SHOULD/MAY-Regeln
- ✅ Stack-spezifische Testing-Patterns (Python: pytest, Shell: bats, Frontend: Jest/Cypress)
- ✅ Test-Isolation & Fixtures-Prinzipien
- ✅ "Genug getestet" Kriterien (Happy Path + Edge Cases + Errors)
- ✅ Was NICHT testen (Trivial, Third-Party, Private)

### Ergebnis
- **Datei:** abschnitt-d-analyse.md
- **Test-Philosophie:** Sicherheitsnetz, nicht 100% Coverage-Ziel
- **Minimale Policy:** Tests anpassen, DoD, Reproduzierbarkeit
- **Stack-Tools:** pytest (Python), bats (Shell), Jest+Cypress (Frontend)

### Entscheidungen getroffen (2025-01-17)
1. **Coverage-Targets:** ✅ Sinnvoll wenn einfach, bei Komplexität User fragen
2. **TDD-Empfehlung:** ✅ Neutral (MAY)
3. **Test-Dokumentations-Stil:** ✅ Offen lassen (keine Vorgabe)
4. **CI/CD-Integration:** ✅ Nicht Teil der Testing-Policy (erstmal nicht nötig)
5. **Performance-Tests:** ✅ Fokus auf Functional-Tests

### Implementiert (2025-01-17)
- ✅ `standard/testing.md` erstellt
  - Test-Philosophie (Sicherheitsnetz, nicht 100% Coverage-Ziel)
  - Test-Pyramide (Unit > Integration > E2E)
  - Minimale MUST/SHOULD/MAY-Regeln
  - "Genug getestet" Kriterien (Happy Path + Edge Cases + Errors)
  - Was NICHT testen (Trivial, Third-Party, Private)
  - Test-Isolation & Fixtures
  - Komplexität-Handling: Bei Unsicherheit User fragen
  - Stack-spezifische Verweise
- ✅ Templates erweitert um Testing-Sektionen:
  - `python-stack.md`: pytest, Fixtures, Mocking, Coverage
  - `shell-scripting.md`: bats, Setup/Teardown, Assertions
  - `frontend.md`: Jest, React Testing Library, Cypress, Playwright
- ✅ `README.md` aktualisiert (Testing in Standard-Regeln)

### Abschnitt D: Abgeschlossen ✅

---

## Offene Entscheidungen

- Konkretes Format des Activation Manifests
- Platzierung neuer Dokumente im Repository
- Abgrenzung zwischen generischen und stack-spezifischen Security-Regeln

---

## Abgeschlossene Schritte

- **2025-01-17:** Abschnitt A – Expliziter Aktivierungsmechanismus ✅
  - Analyse: Aktivierungsdimensionen aus bestehenden llm-rules abgeleitet
  - Vorschlag: YAML-basiertes Activation Manifest Format
  - Entscheidungen: Format (YAML), Dateiname (llm-rules.yml), Platzierung (standard/), Fehlerbehandlung (Fallback)
  - Implementierung: standard/activation-manifest.md erstellt
  - Aktualisierung: README.md mit YAML-Beispielen
  - Outputs: abschnitt-a-analyse.md, standard/activation-manifest.md

- **2025-01-17:** Abschnitt B – Präzedenz- & Konfliktmodell ✅
  - Analyse: Regel-Ebenen und 5 Konfliktszenarien identifiziert
  - Vorschlag: Präzedenz-Hierarchie (Standard > Template > Skills > Projekt)
  - Entscheidungen: EXCEPTION-Format, automatisches Logging, Skill-Orthogonalität nur dokumentieren
  - Implementierung: standard/precedence-model.md erstellt
  - Aktualisierung: README.md mit Precedence Model
  - Outputs: abschnitt-b-analyse.md, standard/precedence-model.md

- **2025-01-17:** Abschnitt C – Security & Safety ✅
  - Analyse: Übergreifende + stack-spezifische Risiken (19 Szenarien)
  - Vorschlag: Zentrale + stack-spezifische Security-Regeln
  - Entscheidungen: Zentral in einem Dokument, error-handling.md erweitern, 4 Stacks ausreichend, neue Templates erstellen
  - Implementierung: standard/error-handling-security.md erweitert
  - Neue Templates: shell-scripting.md, frontend.md
  - Python-Stack erweitert um Security-Sektion
  - Aktualisierung: README.md
  - Outputs: abschnitt-c-analyse.md, error-handling-security.md, shell-scripting.md, frontend.md

- **2025-01-17:** Abschnitt D – Testing-Policy ✅
  - Analyse: Bestehende Test-Regeln, Test-Philosophie, Stack-Patterns
  - Vorschlag: Minimale Testing-Policy, Test-Pyramide, "Genug getestet" Kriterien
  - Entscheidungen: Tests sinnvoll wenn einfach (sonst User fragen), TDD neutral, Stil offen, kein CI/CD, Fokus Functional
  - Implementierung: standard/testing.md erstellt
  - Templates erweitert: python-stack.md, shell-scripting.md, frontend.md (Testing-Sektionen)
  - Aktualisierung: README.md
  - Outputs: abschnitt-d-analyse.md, testing.md, erweiterte Templates

