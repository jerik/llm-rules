# Zielbild-Kandidaten – Phase 4

Vorschläge für Zielbild-Bausteine basierend auf Inventarisierung, Klassifikation und Intent-Analyse.

**Status:** Vorschlag  
**Erstellt:** 2025-01-17  
**Letzte Aktualisierung:** 2025-01-17

---

## Wichtiger Hinweis

**Alle Inhalte in diesem Dokument sind Vorschläge.**

Es wurden keine stillschweigenden Entscheidungen getroffen.
Jeder Baustein benötigt explizite Freigabe durch Entscheidungsträger.

---

## Struktur des Zielbilds (Vorschlag)

```
llm-rules/
├── standard/              # Grundsätzlich geltende Regeln
│   ├── communication.md   # Kommunikationsregeln
│   ├── git-workflow.md    # Git-Workflow
│   ├── code-quality.md    # Code-Qualität & Dokumentation
│   ├── error-handling.md  # Error Handling & Security
│   └── versioning.md      # Semantic Versioning
│
├── skills/                # Wiederverwendbare Workflow-Muster
│   ├── workflow-standard.md        # Standard-Workflow (Doku lesen, Dialog, DoD)
│   ├── fortschritt-tracking.md     # Checkboxen, Zusammenfassungen
│   ├── synergie-analyse.md         # Backlog konsultieren, Zusammenhänge
│   └── git-tag-management.md       # Optionaler Skill für Tagging
│
├── project-templates/     # Technologie-spezifische Templates
│   ├── python-stack.md    # Python, ruff, pytest
│   ├── vim-plugin.md      # Vimscript, vint, Plugin-Struktur
│   └── keypirinha-plugin.md  # Keypirinha-API
│
└── context/               # Kontextuelle Informationen
    └── execution-environment.md  # claude-on-web vs. pi
```

---

## Standard-Regeln (Vorschläge)

### 1. Communication (communication.md)

**Vorschlag: Konsolidierte Kommunikationsregeln**

```markdown
# Kommunikationsregeln

## Grundprinzipien

### Bei Unklarheiten
- MUST: Rückfragen stellen, bevor du rätst oder annimmst
- MUST: Fragen sofort stellen, nicht nach Implementierung

### Frageformat
- MUST: Fragen mit f1, f2, ... fn nummerieren
- SHOULD: Fragen im TL;DR-Stil formulieren (max. 3-4 Zeilen)
- Beispiel: `f1: Soll Feature X auch Szenario Y abdecken?`

### Antwortstil
- MUST: Sachlich, kritisch und präzise antworten
- MUST: Keine ausführlichen Antworten ohne explizite Anfrage
- SHOULD: TL;DR-Stil bevorzugen
- MUST: Klare, verständliche Sprache verwenden

### Kritische Analyse
- MUST: Inhalte und Prompts kritisch analysieren
- MUST: Schwächen oder Widersprüche benennen
- SHOULD: Verbesserungsvorschläge machen (als Vorschläge kennzeichnen)
```

**Quelle:** DP-R01-R12, CL-R42-R46, FD-R24-R27 (konsolidiert)

**Begründung:**
- Über alle 4 Quellen identisch → hohe Wichtigkeit
- Verhindert Missverständnisse und Fehlimplementierungen
- Frageformat f1...fn wurde als Standard festgelegt

---

### 2. Git Workflow (git-workflow.md)

**Vorschlag: Git-Workflow-Regeln**

```markdown
# Git Workflow

## Branch-Strategie

### Main Branch
- MUST: Main Branch ist geschützt
- MUST NOT: Niemals direkt auf main pushen oder committen
- MUST: Alle Änderungen über Feature-Branches

### Feature Branches
- MUST: Namenskonvention einhalten:
  - **pi (Shell):** `claude/<feature-name>`
  - **claude-on-web (Browser):** `claude/<feature-name>-<session-id>`
- Context: Session-ID identifiziert Browser-Session bei claude-on-web
- SHOULD: Feature-Name beschreibend wählen

## Commit-Messages

### Sprache
- MUST: Commits nur in Englisch

### Format
- MUST: Klare, beschreibende Messages
- SHOULD: Conventional Commits verwenden (feat:, fix:, docs:, chore:)
- Beispiele:
  - `feat: add user authentication`
  - `fix: resolve null pointer in parser`
  - `docs: update API documentation`
  - `chore: update dependencies`

## Code-Sprache
- MUST: Code und Kommentare nur in Englisch
```

**Quelle:** CL-R15-R18, CL-R38, FD-R16-R20, KP-R45, KP-R55-R59 (konsolidiert)

**Begründung:**
- Geschützter Main mehrfach als "NIEMALS" betont
- Branch-Konvention: Entscheidung claude-on-web vs. pi getroffen
- Conventional Commits: weit verbreiteter Standard
- Englisch: Kollaboration, internationale Teams

---

### 3. Code Quality (code-quality.md)

**Vorschlag: Code-Qualitätsregeln**

```markdown
# Code-Qualität & Dokumentation

## Code-Änderungen

### Diff-Größe
- SHOULD: Kleine, reviewbare Diffs bevorzugen
- SHOULD: Große Features in mehrere Commits aufteilen
- Begründung: Einfachere Reviews, niedrigere Fehlerrate

### Tests
- MUST: Tests bei Verhaltensänderung anpassen
- MUST NOT: Tests ignorieren oder "later" markieren
- Begründung: Verhindert unerkannte Regressionen

### Rückwärtskompatibilität
- MUST: Rückwärtskompatibilität wahren (außer bei explizitem Version-Bump)
- MUST: Breaking Changes dokumentieren und ankündigen
- Begründung: Stabilität für Nutzer

## Dokumentation

### Inline-Dokumentation
- MUST: Docstrings/Comments für öffentliche APIs
- SHOULD: Komplexe Logik kommentieren
- Sprache: Englisch

### User-Dokumentation
- MUST: Dokumentation bei neuem user-facing Behavior aktualisieren
- MUST: README, CHANGELOG synchron halten
- Begründung: Verhindert Verwirrung, Fehlannahmen
- MUST: Dokumentation in markdown 


## Sprache
- MUST: Code und Kommentare nur in Englisch
- Begründung: Internationale Zusammenarbeit, Ökosystem-Standard
```

**Quelle:** CL-R18-R21, CL-R26, CL-R40, FD-R08, FD-R11, KP-R21, KP-R24, KP-R54 (konsolidiert)

**Begründung:**
- Wartbarkeit langfristig sicherstellen
- Review-Prozesse effizient gestalten
- Dokumentation als lebendiges Artefakt

---

### 4. Error Handling & Security (error-handling.md)

**Vorschlag: Error Handling & Security-Regeln**

```markdown
# Error Handling & Security

## Error Handling

### Allgemein
- MUST: Umfassendes Error Handling für alle externen Aufrufe (API, File I/O, Network)
- MUST: Benutzerfreundliche Fehlermeldungen bereitstellen
- MUST NOT: Exceptions unbehandelt durchreichen

### Logging
- SHOULD: Logging für Debugging implementieren
- SHOULD: Log-Level sinnvoll nutzen (info, warn, error)
- Begründung: Debugging in Produktion ermöglichen

### Spezifische Fehlertypen
- MUST: Network Errors abfangen (Timeout, Offline, Connection Refused)
- MUST: Auth-Fehler behandeln und klar kommunizieren (401, 403)
- MUST NOT: Auth-Fehler ignorieren (Security-Risiko)

### Timeouts
- MUST: Timeouts für API-Requests und externe Aufrufe setzen
- SHOULD: Sinnvolle Default-Timeouts (z.B. 10 Sekunden)
- Begründung: Verhindert hängende Anwendungen

## Security

### Credentials
- MUST NOT: API-Keys, Tokens oder Passwörter im Code hardcoden
- MUST NOT: Credentials in Git committen
- MUST: Credentials in .gitignore eintragen
- MUST: Externe Config-Dateien oder Umgebungsvariablen nutzen
- Begründung: Verhindern permanenter Credential-Exposition

### Configuration
- SHOULD: Sinnvolle Defaults in Beispiel-Configs bereitstellen
- MUST: Beispiel-Configs ohne echte Credentials committen
- SHOULD: Template-Dateien mit Platzhaltern bereitstellen
```

**Quelle:** KP-R30, KP-R32-R35, KP-R38, KP-R40, KP-R43, KP-R49-R51 (konsolidiert)

**Begründung:**
- Robustheit und Sicherheit sind nicht-verhandelbar
- Hardcodierte Credentials: reales, häufiges Problem
- Logging: essentiell für Wartung

---

### 5. Versioning (versioning.md)

**Vorschlag: Versioning-Regeln**

```markdown
# Versioning

## Semantic Versioning

### Format
- MUST: Semantic Versioning verwenden: `v{major}.{minor}.{patch}`
- Beispiele: v1.0.0, v1.2.3, v2.0.0

### Versionierung
- MUST: Major +1 bei Breaking Changes
- SHOULD: Minor +1 bei neuen Features (rückwärtskompatibel)
- SHOULD: Patch +1 bei Bugfixes (rückwärtskompatibel)

### Git Tags
- Context: Git Tag Management ist projektspezifisch (siehe skills/)
- SHOULD: Bei versionierten Artefakten Semantic Versioning nutzen
```

**Quelle:** KP-R07, KP-R60 (konsolidiert)

**Begründung:**
- Weit verbreiteter De-facto-Standard
- Kommuniziert Kompatibilitätserwartungen klar
- Git Tag Management wurde als optionaler Skill eingestuft

---

## Skills (Vorschläge)

### 1. Standard-Workflow (workflow-standard.md)

**Vorschlag: Standard-Workflow-Skill**

```markdown
# Standard-Workflow

Wiederverwendbares Workflow-Muster für Feature-Entwicklung.

## Aktivierung
- Wird aktiviert durch Projektvorgabe oder explizite Nennung

## Workflow-Schritte

### 1. Dokumentation lesen
- MUST: CLAUDE.md (oder äquivalente Projektdatei) lesen
- MUST: USER-STORY.md lesen (falls vorhanden)
- SHOULD: BACKLOG.md lesen (falls vorhanden)
- SHOULD: Archive konsultieren für bisherige Schritte
- Optional: LEARNINGS.md oder ähnliche Dateien lesen

### 2. Dialog & Verfeinerung
- MUST: Anforderungen mit User klären
- MUST: Technische Details gemeinsam festlegen
- MUST: Offene Fragen beantworten (siehe standard/communication.md)
- MUST: Explizite Freigabe durch User einholen vor Implementierung

### 3. Implementierung
- MUST: Code-Standards einhalten (siehe standard/)
- SHOULD: Checkboxen in USER-STORY.md während Arbeit aktualisieren
- MUST: Commits gemäß Git-Workflow (siehe standard/git-workflow.md)

### 4. Definition of Done (DoD)
- MUST: DoD vollständig ausführen (projektspezifisch definiert)
- MUST: Bei fehlgeschlagenen Checks iterieren bis alle grün
- MUST NOT: Bei Fehler abbrechen oder Checks überspringen
- Begründung: Qualitätsstandards sind nicht verhandelbar

### 5. Review & Abschluss
- SHOULD: Review mit User durchführen
- SHOULD: Dokumentation aktualisieren (README, CHANGELOG)
- SHOULD: Kurze Zusammenfassung erstellen (was, warum, wie getestet)

## DoD-Prinzip

Das DoD ist projektspezifisch definiert, aber folgende Prinzipien gelten:
- MUST: Alle definierten Checks ausführen
- MUST: Iterieren bis alle Checks grün
- MUST NOT: Abbrechen bei Fehler
- Typische Checks: Linting, Formatierung, Tests, Builds
```

**Quelle:** CL-R01-R08, CL-R14, CL-R39, CL-R41, FD-R01-R04, FD-R14, KP-R01-R03, KP-R09-R11, KP-R18 (konsolidiert)

**Begründung:**
- Konsistentes Muster über alle 3 Projekte
- "Iterieren bis grün" mehrfach betont
- Dialog-basierte Verfeinerung verhindert Fehlimplementierungen
- Explizite Freigabe verhindert autonomes Handeln

---

### 2. Fortschritt-Tracking (fortschritt-tracking.md)

**Vorschlag: Fortschritt-Tracking-Skill**

```markdown
# Fortschritt-Tracking

Wiederverwendbares Muster für Transparenz und Nachvollziehbarkeit.

## Aktivierung
- Wird aktiviert durch Projektvorgabe oder explizite Nennung

## Mechanismen

### Checkboxen in USER-STORY.md
- SHOULD: Checkboxen während Implementierung aktualisieren
- SHOULD: Erledigte Punkte abhaken, nicht löschen
- Begründung: User kann Fortschritt live verfolgen

### Zusammenfassung am Ende
- SHOULD: Kurze Zusammenfassung erstellen nach Abschluss
- Inhalte:
  - Was wurde getan?
  - Was hat sich geändert?
  - Wie wurde getestet?
- Begründung: Wissensartefakt für spätere Nachvollziehbarkeit

### Dokumentation aktualisieren
- MUST: User-facing Dokumentation bei neuem Behavior aktualisieren
- Typische Dateien: README.md, documentation.md, CHANGELOG.md
- Begründung: Dokumentation als lebendiges Artefakt
```

**Quelle:** CL-R06, FD-R15, KP-R09, KP-R12, KP-R19 (konsolidiert)

**Begründung:**
- Transparenz für User
- Wissenstransfer
- Dokumentation synchron halten

---

### 3. Synergie-Analyse (synergie-analyse.md)

**Vorschlag: Synergie-Analyse-Skill**

```markdown
# Synergie-Analyse

Proaktive Analyse von Zusammenhängen zwischen Features.

## Aktivierung
- Wird aktiviert durch Projektvorgabe oder explizite Nennung

## Vorgehen

### BACKLOG.md mitlesen
- MUST: BACKLOG.md IMMER mit konsultieren (falls vorhanden)
- Ziel: Implikationen und Synergien für zukünftige Features erkennen

### Synergien aktiv ansprechen
- SHOULD: Bei Diskussionen Synergien aus Backlog ansprechen
- SHOULD: Architekturentscheidungen mit Blick auf Zukunft treffen
- SHOULD: User auf Implikationen hinweisen

### Nutzen
- Vermeidet isolierte Implementierungen
- Nutzt Synergien frühzeitig
- Reduziert spätere Refactorings
- Verbessert strategische Entscheidungen
```

**Quelle:** KP-R13, KP-R14 (konsolidiert)

**Begründung:**
- Proaktives, strategisches Denken
- Verhindert Architekturfallen
- Langfristige Codequalität

---

### 4. Git Tag Management (git-tag-management.md)

**Vorschlag: Git Tag Management (optionaler Skill)**

```markdown
# Git Tag Management

Optionaler Skill für automatisiertes Git-Tagging bei Releases.

## Aktivierung
- Projektspezifisch (nicht Standard)
- Muss explizit in Projektvorgaben aktiviert werden

## Workflow

### Bei Start einer neuen User Story
- SHOULD: Git Tags prüfen: `git tag --list`
- SHOULD: Prüfen ob Tag für letzten Merge auf main fehlt
- Falls fehlend: Tag erstellen

### Tag-Format
- MUST: Semantic Versioning: `v{major}.{minor}.{patch}`

### Versionierung
- Projektspezifisch definiert
- Beispiel (aus keypy): Minor +1 pro Feature-Release

### Tag-Erstellung
```bash
git tag -a v1.X.0 -m "Release v1.X.0: Feature Name"
git push origin v1.X.0
```

## Zweck
- Nachvollziehbarkeit welches Feature in welcher Version
- Release-Punkte markieren
- Automatisierung von Release-Notes möglich
- Einfache Rollbacks
```

**Quelle:** KP-R05-R08, KP-R61-R66 (konsolidiert)

**Begründung:**
- Als projektspezifisch eingestuft (Entscheidung getroffen)
- Sehr detailliert in keypy beschrieben
- Nicht in anderen Projekten vorhanden
- Nützlich für Release-Management, aber nicht universell nötig

---

## Project Templates (Vorschläge)

### 1. Python Stack (python-stack.md)

**Vorschlag: Python-Stack-Template**

```markdown
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
- command:
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
```

**Quelle:** CL-R09-R11, CL-R22-R25, FD-R05-R07, FD-R10, FD-R12-R13, FD-R21-R22, KP-R15-R17, KP-R20, KP-R23 (konsolidiert)

**Begründung:**
- Über 3 Projekte konsistent
- PEP 8: Python-Community-Standard
- ruff: modernes, schnelles Tooling
- pytest: De-facto-Standard

---

### 2. Vim Plugin (vim-plugin.md)

**Vorschlag: Vim-Plugin-Template**

```markdown
# Vim Plugin

Template für Vim-Plugin-Entwicklung.

## Plugin-Struktur

### Verzeichnis-Layout
- `plugin/` - Minimal halten (nur Wiring)
- `autoload/` - Komplexe Logik hier ablegen
- `test/` - Test-Dateien

### Scoping
- MUST: Interne Helfer mit `s:`-Scope
- MUST: Globaler State nur für `g:plugin_name_*` Config-Variablen
- Begründung: Namespace-Kollisionen vermeiden

### Autocmds
- MUST: Autocmds in dedizierter augroup mit `autocmd!`
- Beispiel:
  ```vim
  augroup MyPlugin
    autocmd!
    autocmd BufRead *.txt call myfunction()
  augroup END
  ```

## Tooling

### vint
- MUST: vint für Linting verwenden
- MUST: Zero warnings akzeptiert
- Command:
  ```bash
  vint plugin/
  ```

### Tests
- MUST: Headless Tests ausführen
- Command:
  ```bash
  vim -Nu NONE -n -es -S test/run.vim
  ```
- Assertions: `assert_equal()`, `assert_match()`, `assert_true()`, `assert_fails()`
- Layout: `test/run.vim` + `test/test_*.vim`
- Isolation: Temp-Verzeichnisse, kein User-vimrc

## Definition of Done (Beispiel)

```bash
# 1. Vimscript Linting
vint plugin/

# 2. Tests
vim -Nu NONE -n -es -S test/run.vim

# Bei Fehler: Iterieren bis alle grün!
```

## Projekt-spezifische Regeln

### Generierte Dateien
- MUST NOT: Generierte Dateien editieren
- Beispiel: `mindboxes/*.mb` (projektspezifisch)
- Begründung: Änderungen gehen bei Regenerierung verloren
```

**Quelle:** CL-R12-R13, CL-R27-R37 (konsolidiert)

**Begründung:**
- Vim-spezifische Best Practices
- Namespace-Management essentiell
- Tests in Vim: spezielle Infrastruktur nötig

---

### 3. Keypirinha Plugin (keypirinha-plugin.md)

**Vorschlag: Keypirinha-Plugin-Template**

```markdown
# Keypirinha Plugin

Template für Keypirinha-Plugin-Entwicklung.

**Platform:** Windows-only

## Plugin-Klasse

### Inheritance
- MUST: Plugin-Klasse von `keypirinha.plugin.Plugin` erben

### Pflicht-Methoden
- MUST: Folgende Methoden implementieren:
  - `on_catalog()` - Catalog Items registrieren
  - `on_suggest()` - User Input verarbeiten
  - `on_execute()` - Aktionen ausführen
  - `on_events()` - Config-Changes behandeln
  - `on_deactivate()` - Cleanup durchführen

## Performance & UX

### UI-Responsiveness
- MUST NOT: UI blockieren
- MUST: Async/Non-blocking für API-Calls oder lange Operationen
- Begründung: Keypirinha friert sonst ein → schlechte UX

### Keypirinha-Funktionen
- SHOULD: `kp.shell_execute()` für Browser-URLs nutzen
- Context: Keypirinha übernimmt Live-Filtering → Plugin liefert nur Daten

## Error Handling

### Logging
- SHOULD: `self.info()`, `self.warn()`, `self.err()` für Logging nutzen
- Begründung: Keypirinha-Konsole (F2) für Debugging

### Fehlermeldungen
- MUST: Benutzerfreundliche Fehlermeldungen in Keypirinha anzeigen
- MUST: Network Errors abfangen
- MUST: API-Fehler klar kommunizieren

## Konfiguration

### Config-Dateien
- Format: INI (`keypi_plugin.ini`)
- Location: User-Verzeichnis (`%APPDATA%\Keypirinha\User\`)
- MUST NOT: Credentials im Code
- MUST: Template mit leeren Werten committen
- MUST: `.ini` mit Credentials in `.gitignore`

## Typische Fehler vermeiden

### NICHT tun:
- ❌ UI während API-Calls blockieren
- ❌ `on_deactivate()` Cleanup vergessen
- ❌ Veraltete API-Endpunkte nutzen

### IMMER tun:
- ✅ Von `keypirinha.plugin.Plugin` erben
- ✅ Alle Pflicht-Methoden implementieren
- ✅ Sinnvolle Defaults in Beispiel-Config

## Definition of Done (Beispiel)

```bash
# 1. Code-Style
ruff check .

# 2. Formatierung
ruff format --check .

# 3. Tests (falls vorhanden)
pytest

# Bei Fehler: Iterieren bis alle grün!
```

## Manuelle Tests
- Plugin nach `%APPDATA%\Keypirinha\InstalledPackages\` kopieren
- Keypirinha neu starten (Ctrl + Alt + R)
- Plugin testen
```

**Quelle:** KP-R25-R29, KP-R31-R32, KP-R40-R54 (konsolidiert)

**Begründung:**
- Sehr spezifische API-Anforderungen
- UI-Blocking mehrfach betont → kritisch
- Windows-only Platform

---

## Context-Informationen (Vorschlag)

### Execution Environment (execution-environment.md)

**Vorschlag: Kontext-Regel für Ausführungsumgebung**

```markdown
# Execution Environment

Kontext-Information über Ausführungsumgebung.

## Varianten

### pi (Shell)
- Ausführung in Terminal/Shell
- Tool: pi-mono oder ähnlich
- Branch-Konvention: `claude/<feature-name>`

### claude-on-web (Browser)
- Ausführung im Browser (claude.ai)
- Keine persistente Umgebung zwischen Sessions
- Branch-Konvention: `claude/<feature-name>-<session-id>`
- Session-ID: Identifiziert Browser-Session für Nachvollziehbarkeit

## Erkennung

Die Ausführungsumgebung wird vom Projekt definiert oder vom User kommuniziert.

## Implikationen

### Git Workflow
- Branch-Naming unterschiedlich (siehe standard/git-workflow.md)
- Session-ID nur bei claude-on-web nötig

### Dateizugriff
- pi: Vollständiger Dateizugriff
- claude-on-web: Datenzugriff auf das freigegegben git repository

### Persistenz
- pi: Session-übergreifend persistent
- claude-on-web: Session-lokal
```

**Quelle:** Entscheidung zu Branch-Namenskonvention (CL-R16, FD-R17, KP-R53, KP-R56)

**Begründung:**
- Unterschied claude-on-web vs. pi ist relevant
- Session-ID nötig bei Browser-Nutzung
- Kontext-Information, keine aktive Regel

---

## Verworfene Regeln

### FD-R28: "Vorgehen im Dialog"
- **Status:** Verworfen (Entscheidung 2025-01-17)
- **Grund:** Unvollständig in Quelle, keine klare Bedeutung
- **Alternative:** Dialog-Workflow ist in skills/workflow-standard.md abgedeckt

---

## Nächste Schritte (Vorschlag)

### Für Gate 2 (Optimierungsfreigabe)

Falls Gate 2 freigegeben wird, könnten folgende Optimierungen erfolgen:

1. **Sprachliche Vereinheitlichung:**
   - MUST / MUST NOT / SHOULD / SHOULD NOT konsequent nutzen
   - Konsistente Formulierungen über Dokumente

2. **Strukturierung:**
   - YAML oder JSON für maschinenlesbare Regeln
   - Metadaten (Priorität, Quelle, Version)

3. **Externe Best Practices:**
   - RFC 2119 für Keyword-Definitionen (MUST, SHOULD, etc.)
   - Vergleich mit anderen LLM-Governance-Frameworks

4. **Tooling:**
   - Validator für Regelkonformität
   - Template-Generator für neue Projekte

**Wichtig:** Diese Optimierungen sind erst nach Gate 2 zulässig.

---

## Zusammenfassung

### Standard-Regeln (5)
1. Communication (15+ Regeln konsolidiert)
2. Git Workflow (12+ Regeln konsolidiert)
3. Code Quality (11+ Regeln konsolidiert)
4. Error Handling & Security (9+ Regeln konsolidiert)
5. Versioning (2+ Regeln konsolidiert)

### Skills (4)
1. Standard-Workflow (20+ Regeln konsolidiert)
2. Fortschritt-Tracking (5+ Regeln konsolidiert)
3. Synergie-Analyse (2 Regeln konsolidiert)
4. Git Tag Management (10+ Regeln konsolidiert, optional)

### Project Templates (3)
1. Python Stack (13+ Regeln konsolidiert)
2. Vim Plugin (13+ Regeln konsolidiert)
3. Keypirinha Plugin (30+ Regeln konsolidiert)

### Context (1)
1. Execution Environment (Branch-Naming-Konvention)

### Verworfene Regeln (1)
1. FD-R28

**Gesamt:** 132 Regeln konsolidiert, strukturiert und als Zielbild-Kandidaten aufbereitet.

---

**Status:** Vorschlag abgeschlossen  
**Datum:** 2025-01-17

**Nächster Schritt:** Freigabe durch Entscheidungsträger für konkrete Umsetzung
