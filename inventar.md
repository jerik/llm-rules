# Inventar – Phase 1

Vollständige Erfassung aller Regeln aus dem Rohmaterial.

**Status:** In Arbeit  
**Erstellt:** 2025-01-17  
**Letzte Aktualisierung:** 2025-01-17

---

## Quellen-Übersicht

| Quelle | Typ | Beschreibung |
|--------|-----|--------------|
| `rohmaterial/default-prompt` | Prompt-Template | Generische Vorgaben für LLM-Interaktion |
| `rohmaterial/CLAUDE.md` | Projekt-Dokumentation | Mindbox-Projekt (Vim-Plugin für Journal-Snippets) |
| `rohmaterial/CLAUDE-frauddetection.md` | Projekt-Dokumentation | Finanzanalyse/Frauddetection-System |
| `rohmaterial/CLAUDE-keypy.md` | Projekt-Dokumentation | KeyPi Jira Query Explorer (Keypirinha-Plugin) |

---

## Extrahierte Regeln

### Quelle: default-prompt

#### DP-R01: Datenanalyse durchführen
**Originaltext:**
> Analysiere meine Daten, sofern ich welche für die bereitgestellt habe.

**Kontext:** Eingangsvorgabe

---

#### DP-R02: Rückfragen bei Unklarheiten
**Originaltext:**
> Wenn Dinge unklar sind, stelle Rückfragen, bevor du ausführlich antwortest.

**Kontext:** Eingangsvorgabe

---

#### DP-R03: Rückfragen im TL;DR-Stil
**Originaltext:**
> Rückfragen bitte im TL;DR style.

**Kontext:** Kommunikationsformat

---

#### DP-R04: Rückfragen mit Q1...Qn nummerieren
**Originaltext:**
> Die Rückfragen sind mit Q1...Qn bezeichnet, damit ich bessere darauf antworten kann.

**Kontext:** Kommunikationsformat

---

#### DP-R05: Sachlich, kritisch, auf den Punkt antworten
**Originaltext:**
> Sind die Rückfragen für dich geklärt, dann Antworte sachlich, kritisch und auf den Punkt.

**Kontext:** Antwortformat

---

#### DP-R06: Keine ausführlichen Antworten ohne explizite Anfrage
**Originaltext:**
> Keine ausführlichen Antworten, ausser ich verlange es explizit

**Kontext:** Antwortformat

---

#### DP-R07: Alle bereitgestellten Daten nutzen
**Originaltext (JSON-Struktur):**
> 1. **Datenanalyse** – Nutze alle mir bereitgestellten Daten, um die gestellte Aufgabe zu bearbeiten.

**Kontext:** Systemprompt-Anweisung

---

#### DP-R08: Bei Unklarheiten sofort nachfragen
**Originaltext (JSON-Struktur):**
> 2. **Unklarheiten klären** – Wenn etwas unklar ist, frage sofort nach. Formuliere deine Fragen in einem TL;DR‑Stil (max. 3–4 Zeilen).

**Kontext:** Systemprompt-Anweisung

---

#### DP-R09: Fragen mit Q1...Qn formatieren
**Originaltext (JSON-Struktur):**
> 3. **Fragen formatieren** – Jede Frage trägt die Bezeichnung Q1…Qn, damit ich sie gezielt beantworten kann.

**Kontext:** Systemprompt-Anweisung

**Widerspruch/Duplikat:** Identisch mit DP-R04

---

#### DP-R10: Sachlich, kritisch, präzise antworten
**Originaltext (JSON-Struktur):**
> 4. **Antworten liefern** – Sobald alle Fragen beantwortet sind, antworte sachlich, kritisch und präzise.

**Kontext:** Systemprompt-Anweisung

**Widerspruch/Duplikat:** Sehr ähnlich zu DP-R05 ("präzise" vs. "auf den Punkt")

---

#### DP-R11: Ausführliche Antworten nur auf explizite Anfrage
**Originaltext (JSON-Struktur):**
> 5. **Ausführliche Antworten** – Nur wenn explizit nach einer ausführlichen Erklärung gefragt wird, erstelle du eine längere Antwort.

**Kontext:** Systemprompt-Anweisung

**Widerspruch/Duplikat:** Identisch mit DP-R06

---

#### DP-R12: Klare, verständliche Sprache verwenden
**Originaltext (JSON-Struktur):**
> **Hinweis:** Verwende im gesamten Dialog nur klare, verständliche Sprache.

**Kontext:** Systemprompt-Anweisung

---

### Quelle: CLAUDE.md (Mindbox-Projekt)

#### CL-R01: CLAUDE.md beim Start lesen
**Originaltext:**
> Guidance for Claude Code when working in this repository.

**Kontext:** Implizite Erwartung, diese Datei zu Beginn zu lesen

---

#### CL-R02: User erstellt USER-STORY.md
**Originaltext:**
> 1. **User** creates `USER-STORY.md` with feature description

**Kontext:** Development Workflow

---

#### CL-R03: Dialog zur Verfeinerung der User Story
**Originaltext:**
> 2. **Dialog:** User + Claude refine together
>    - Clarify requirements
>    - Define technical details
>    - Answer open questions

**Kontext:** Development Workflow

---

#### CL-R04: User gibt Go für Implementierung
**Originaltext:**
> 3. **Decision:** User gives go for implementation

**Kontext:** Development Workflow

---

#### CL-R05: Claude liest CLAUDE.md, USER-STORY.md, BACKLOG.md
**Originaltext:**
> 4. **Claude** implements:
>    - Reads: `CLAUDE.md`, `USER-STORY.md`, `BACKLOG.md`

**Kontext:** Development Workflow / Implementierungsphase

---

#### CL-R06: Checkboxen in USER-STORY.md aktualisieren
**Originaltext:**
> - Updates checkboxes in `USER-STORY.md`

**Kontext:** Development Workflow / Implementierungsphase

---

#### CL-R07: Definition of Done (DoD) ausführen
**Originaltext:**
> - Executes DoD

**Kontext:** Development Workflow / Implementierungsphase

---

#### CL-R08: Review zwischen User und Claude
**Originaltext:**
> 5. **Review:** User + Claude discuss result

**Kontext:** Development Workflow

---

#### CL-R09: Python-Linting mit ruff format
**Originaltext (DoD):**
> python -m ruff format .

**Kontext:** Definition of Done

---

#### CL-R10: Python-Linting mit ruff check
**Originaltext (DoD):**
> python -m ruff check .

**Kontext:** Definition of Done

---

#### CL-R11: Python-Tests mit pytest ausführen
**Originaltext (DoD):**
> python -m pytest -q

**Kontext:** Definition of Done

---

#### CL-R12: Vimscript-Linting mit vint
**Originaltext (DoD):**
> vint plugin/

**Kontext:** Definition of Done

---

#### CL-R13: Vim-Tests headless ausführen
**Originaltext (DoD):**
> vim -Nu NONE -n -es -S test/run.vim

**Kontext:** Definition of Done

---

#### CL-R14: Bei Fehler iterieren bis alle Checks grün sind
**Originaltext:**
> On failure: Fix until all checks pass.

**Kontext:** Definition of Done

---

#### CL-R15: Main Branch ist geschützt
**Originaltext:**
> Main branch: `main` (protected, never push directly)

**Kontext:** Git Workflow

---

#### CL-R16: Feature Branches mit Namenskonvention
**Originaltext:**
> Feature branches: `claude/feature-name`

**Kontext:** Git Workflow

---

#### CL-R17: Git-Commits auf Englisch
**Originaltext:**
> Commits: English, clear and descriptive

**Kontext:** Git Workflow

---

#### CL-R18: Code und Kommentare auf Englisch
**Originaltext:**
> Code and comments: **English only**

**Kontext:** Code Standards / General

---

#### CL-R19: Kleine, reviewbare Diffs bevorzugen
**Originaltext:**
> Prefer small, reviewable diffs

**Kontext:** Code Standards / General

---

#### CL-R20: Tests bei Verhaltensänderung anpassen
**Originaltext:**
> Adjust tests with every behavioral change

**Kontext:** Code Standards / General

---

#### CL-R21: Rückwärtskompatibilität wahren
**Originaltext:**
> Maintain backward compatibility (unless explicitly bumping version)

**Kontext:** Code Standards / General

---

#### CL-R22: PEP 8 strikt befolgen
**Originaltext:**
> Follow **PEP 8** strictly

**Kontext:** Code Standards / Python

---

#### CL-R23: ruff für Formatierung und Linting
**Originaltext:**
> **ruff** for formatting and linting

**Kontext:** Code Standards / Python

---

#### CL-R24: pytest für Tests
**Originaltext:**
> **pytest** for tests (use `tmp_path` for file I/O)

**Kontext:** Code Standards / Python

---

#### CL-R25: Type Hints wo sinnvoll
**Originaltext:**
> Type hints where useful

**Kontext:** Code Standards / Python

---

#### CL-R26: Docstrings für öffentliche Methoden
**Originaltext:**
> Docstrings for public methods

**Kontext:** Code Standards / Python

---

#### CL-R27: plugin/ minimal halten
**Originaltext:**
> Keep `plugin/` minimal (wiring only)

**Kontext:** Code Standards / Vimscript

---

#### CL-R28: Komplexe Logik in autoload/
**Originaltext:**
> Complex logic → `autoload/mindbox.vim`

**Kontext:** Code Standards / Vimscript

---

#### CL-R29: Interne Helfer mit s:-Scope
**Originaltext:**
> Internal helpers: `s:`-scoped

**Kontext:** Code Standards / Vimscript

---

#### CL-R30: Globaler State nur für g:mindbox_* Config
**Originaltext:**
> Global state only for `g:mindbox_*` config

**Kontext:** Code Standards / Vimscript

---

#### CL-R31: Autocmds in dedizierter augroup
**Originaltext:**
> Autocmds in dedicated augroup with `autocmd!`

**Kontext:** Code Standards / Vimscript

---

#### CL-R32: vint ohne Warnings
**Originaltext:**
> **vint** for linting (zero warnings)

**Kontext:** Code Standards / Vimscript

---

#### CL-R33: Vim-Tests headless ausführen
**Originaltext:**
> Headless: `vim -Nu NONE -n -es -S test/run.vim`

**Kontext:** Code Standards / Vim Tests

**Widerspruch/Duplikat:** Identisch mit CL-R13

---

#### CL-R34: Assertions für Vim-Tests
**Originaltext:**
> Assertions: `assert_equal()`, `assert_match()`, `assert_true()`, `assert_fails()`

**Kontext:** Code Standards / Vim Tests

---

#### CL-R35: Test-Layout mit test/run.vim + test/test_*.vim
**Originaltext:**
> Layout: `test/run.vim` + `test/test_*.vim`

**Kontext:** Code Standards / Vim Tests

---

#### CL-R36: Test-Isolation mit Temp-Verzeichnissen
**Originaltext:**
> Isolation: temp directories, no user vimrc

**Kontext:** Code Standards / Vim Tests

---

#### CL-R37: Generierte Dateien niemals editieren
**Originaltext:**
> 1. **Never edit generated files** (`mindboxes/*.mb`)

**Kontext:** Rules for Claude

---

#### CL-R38: Niemals direkt auf main pushen
**Originaltext:**
> 2. **Never push directly to main**

**Kontext:** Rules for Claude

**Widerspruch/Duplikat:** Identisch mit CL-R15

---

#### CL-R39: DoD befolgen – alle Checks grün vor Commit
**Originaltext:**
> 3. **Follow DoD** - all checks green before commit

**Kontext:** Rules for Claude

**Widerspruch/Duplikat:** Ähnlich zu CL-R14

---

#### CL-R40: README bei neuem User-facing Behavior aktualisieren
**Originaltext:**
> 4. **On new user-facing behavior:** Update README

**Kontext:** Rules for Claude

---

#### CL-R41: Bei Fehler iterieren bis grün
**Originaltext:**
> 5. **On failures:** Iterate until green, don't abort

**Kontext:** Rules for Claude

**Widerspruch/Duplikat:** Identisch mit CL-R14

---

#### CL-R42: Inhalte und Prompts kritisch analysieren
**Originaltext:**
> Analyze content and prompts critically

**Kontext:** Communication

---

#### CL-R43: Fragen stellen bei unklaren Informationen
**Originaltext:**
> Ask questions when information is unclear

**Kontext:** Communication

---

#### CL-R44: Fragen mit f1, f2, ... fn benennen
**Originaltext:**
> Label questions as **f1, f2, ... fn**

**Kontext:** Communication

---

#### CL-R45: Kommunikationsstil: tl;dr – präzise und kurz
**Originaltext:**
> Style: **tl;dr** - precise and short

**Kontext:** Communication

---

#### CL-R46: Ausführliche Antworten nur auf explizite Anfrage
**Originaltext:**
> Verbose answers only on explicit request

**Kontext:** Communication

**Widerspruch/Duplikat:** Identisch mit DP-R06 und DP-R11

---

### Quelle: CLAUDE-frauddetection.md

#### FD-R01: idee.md lesen
**Originaltext:**
> Lies idee.md

**Kontext:** Vorgehen

---

#### FD-R02: user-story.md lesen (sofern vorhanden)
**Originaltext:**
> Lies user-story.md sofern vorhanden

**Kontext:** Vorgehen

---

#### FD-R03: backlog.md lesen (sofern vorhanden)
**Originaltext:**
> Lies backlog.md sofern vorhanden

**Kontext:** Vorgehen

---

#### FD-R04: Archive für bisherige Schritte konsultieren
**Originaltext:**
> Im archive sind die umgesetzten anforderungen enthalten, dort kannst du dich über die bisherigen Schritte informieren

**Kontext:** Vorgehen

---

#### FD-R05: Python verwenden
**Originaltext:**
> - python verwenden

**Kontext:** Tooling

---

#### FD-R06: uv verwenden
**Originaltext:**
> - uv verwenden

**Kontext:** Tooling

---

#### FD-R07: PEP 8 strikt befolgen
**Originaltext:**
> - PEP 8 Style Guide strikt befolgen

**Kontext:** Coding Standards

**Widerspruch/Duplikat:** Identisch mit CL-R22

---

#### FD-R08: Code und Kommentare auf Englisch
**Originaltext:**
> - Code und Kommentare im Quellcode nur in Englisch

**Kontext:** Coding Standards

**Widerspruch/Duplikat:** Identisch mit CL-R18

---

#### FD-R09: Git-Commits auf Englisch
**Originaltext:**
> - Git-Commits nur in Englisch

**Kontext:** Coding Standards

**Widerspruch/Duplikat:** Identisch mit CL-R17

---

#### FD-R10: Type Hints wo sinnvoll
**Originaltext:**
> - Type Hints wo sinnvoll verwenden

**Kontext:** Coding Standards

**Widerspruch/Duplikat:** Identisch mit CL-R25

---

#### FD-R11: Docstrings für öffentliche Methoden
**Originaltext:**
> - Docstrings für alle öffentlichen Methoden

**Kontext:** Coding Standards

**Widerspruch/Duplikat:** Identisch mit CL-R26

---

#### FD-R12: Linting mit ruff
**Originaltext:**
> - Linting mittels ruff

**Kontext:** Definition of Done

---

#### FD-R13: Pytest-Testfälle erstellen und ausführen
**Originaltext:**
> - Pytest passende Testfälle erstellen und ausführen

**Kontext:** Definition of Done

---

#### FD-R14: Bei fehlgeschlagenen Checks iterieren
**Originaltext:**
> - Wenn checks fehlschlagen: iteriere bis alle grün sind!

**Kontext:** Definition of Done

**Widerspruch/Duplikat:** Identisch mit CL-R14 und CL-R41

---

#### FD-R15: Kurze Zusammenfassung erstellen
**Originaltext:**
> - kurze Zusammenfassung erstellen (was wurde getan, was hat sich geändert, wie getestet)

**Kontext:** Definition of Done

---

#### FD-R16: Main Branch ist geschützt
**Originaltext:**
> - Main Branch: main (geschützt)

**Kontext:** Git Workflow

**Widerspruch/Duplikat:** Identisch mit CL-R15

---

#### FD-R17: Feature Branches mit Namenskonvention
**Originaltext:**
> - Feature Branches: claude/<feature-name>-<session-id>

**Kontext:** Git Workflow

**Anmerkung:** Abweichung von CL-R16 (zusätzlich `<session-id>`)

---

#### FD-R18: Niemals direkt auf main pushen
**Originaltext:**
> - NIEMALS direkt auf main pushen

**Kontext:** Git Workflow

**Widerspruch/Duplikat:** Identisch mit CL-R15 und CL-R38

---

#### FD-R19: Klare, beschreibende Commit-Messages (Englisch)
**Originaltext:**
> - Commits: Klare, beschreibende Messages (Englisch)

**Kontext:** Git Workflow

**Widerspruch/Duplikat:** Identisch mit CL-R17

---

#### FD-R20: Conventional Commits verwenden
**Originaltext:**
> - Conventional Commits verwenden (feat:, fix:, docs:, chore:)

**Kontext:** Git Workflow

---

#### FD-R21: Testing mit uv run pytest -v
**Originaltext:**
> - Testing: `uv run pytest -v` für Unit-Tests

**Kontext:** Development Workflow

---

#### FD-R22: Vor Commits ruff check ausführen
**Originaltext:**
> - Vor Commits: `uv run ruff check analyze_transactions.py` ausführen

**Kontext:** Development Workflow

---

#### FD-R23: Hauptdatenbank ist transactions.db
**Originaltext:**
> - Hauptdatenbank: `analysis_output/transactions.db`

**Kontext:** Database Management (projektspezifisch)

---

#### FD-R24: Inhalte und Prompts kritisch analysieren
**Originaltext:**
> - Inhalte und prompts kritsch analysieren.

**Kontext:** Kommunikation

**Widerspruch/Duplikat:** Identisch mit CL-R42

---

#### FD-R25: Fragen stellen bei unklaren Informationen
**Originaltext:**
> - Frage stellen sofern unklare Informationen vorliegen

**Kontext:** Kommunikation

**Widerspruch/Duplikat:** Identisch mit CL-R43

---

#### FD-R26: Fragen mit f1...fn benennen
**Originaltext:**
> - Fragen mit f1...fn bennen

**Kontext:** Kommunikation

**Widerspruch/Duplikat:** Identisch mit CL-R44

---

#### FD-R27: Präzise und kurz halten (tl;dr)
**Originaltext:**
> - In der Kommunikation Präzise und kurz halten, ausser user fragt explizit nach ausführlicher Erklärung oder Antwort nach. style tl;dr

**Kontext:** Kommunikation

**Widerspruch/Duplikat:** Identisch mit CL-R45 und DP-R06/DP-R11/CL-R46

---

#### FD-R28: Vorgehen im Dialog
**Originaltext:**
> - Vorgehen im Dialog

**Kontext:** Kommunikation

**Anmerkung:** Unklare Regel, möglicherweise unvollständig

---

### Quelle: CLAUDE-keypy.md

#### KP-R01: CLAUDE.md beim Feature-Start lesen
**Originaltext:**
> 3. **Claude** liest:
>    - `CLAUDE.md` (diese Datei) → Projektregeln

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

**Widerspruch/Duplikat:** Ähnlich zu CL-R05

---

#### KP-R02: USER-STORY.md lesen
**Originaltext:**
> - `USER-STORY.md` → Aktuelle Feature-Spec

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

**Widerspruch/Duplikat:** Teil von CL-R05

---

#### KP-R03: BACKLOG.md lesen
**Originaltext:**
> - `BACKLOG.md` → Kontext & Synergien

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

**Widerspruch/Duplikat:** Teil von CL-R05

---

#### KP-R04: KEYPIRINHA-LEARNINGS.md lesen
**Originaltext:**
> - `KEYPIRINHA-LEARNINGS.md` → Best Practices & Lessons Learned

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

---

#### KP-R05: Git Tags prüfen
**Originaltext:**
> - **Prüft Git Tags**: Checkt ob nach letztem Merge auf `main` ein Git Tag fehlt

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

---

#### KP-R06: Fehlende Tags erstellen
**Originaltext:**
> - Falls Tag fehlt: Erstellt Tag mit nächster Minor-Version (z.B. v1.1.0 → v1.2.0)

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

---

#### KP-R07: Tag-Format einhalten
**Originaltext:**
> - Tag-Format: `v{major}.{minor}.{patch}` (z.B. v1.2.0)

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

---

#### KP-R08: Minor-Version pro Feature-Release erhöhen
**Originaltext:**
> - Regel: Mittlere Zahl (minor) wird +1 erhöht pro Feature-Release

**Kontext:** Entwicklungs-Workflow / Neues Feature starten

---

#### KP-R09: Checkboxen in USER-STORY.md während Arbeit aktualisieren
**Originaltext:**
> - Aktualisiert Checkboxen in `USER-STORY.md` während Arbeit

**Kontext:** Entwicklungs-Workflow / Feature implementieren

**Widerspruch/Duplikat:** Identisch mit CL-R06

---

#### KP-R10: Code-Standards einhalten
**Originaltext:**
> - Hält sich an Code-Standards

**Kontext:** Entwicklungs-Workflow / Feature implementieren

---

#### KP-R11: DoD durchführen
**Originaltext:**
> - Führt DoD durch (siehe unten)

**Kontext:** Entwicklungs-Workflow / Feature implementieren

**Widerspruch/Duplikat:** Identisch mit CL-R07

---

#### KP-R12: documentation.md mit neuen Features aktualisieren
**Originaltext:**
> 6. **Claude** aktualisiert `documentation.md` mit neuen Features

**Kontext:** Entwicklungs-Workflow

---

#### KP-R13: BACKLOG.md immer mitlesen
**Originaltext:**
> - **Claude liest BACKLOG.md IMMER mit**, um Implikationen & Synergien zu erkennen

**Kontext:** Feature-Backlog

---

#### KP-R14: Synergien aus Backlog aktiv ansprechen
**Originaltext:**
> - Bei Diskussionen: Synergien aus Backlog aktiv ansprechen

**Kontext:** Feature-Backlog

---

#### KP-R15: Code-Style mit ruff check prüfen
**Originaltext:**
> # 1. Code-Style prüfen
> ruff check .

**Kontext:** Definition of Done

**Widerspruch/Duplikat:** Ähnlich zu CL-R10

---

#### KP-R16: Code-Formatierung mit ruff format prüfen
**Originaltext:**
> # 2. Code-Formatierung prüfen
> ruff format --check .

**Kontext:** Definition of Done

**Widerspruch/Duplikat:** Ähnlich zu CL-R09

---

#### KP-R17: Tests ausführen (falls vorhanden)
**Originaltext:**
> # 3. Tests ausführen (falls vorhanden)
> pytest

**Kontext:** Definition of Done

**Widerspruch/Duplikat:** Ähnlich zu CL-R11

---

#### KP-R18: Bei fehlgeschlagenen Checks iterieren
**Originaltext:**
> # Wenn Checks fehlschlagen: Iteriere bis alle grün sind!

**Kontext:** Definition of Done

**Widerspruch/Duplikat:** Identisch mit CL-R14, CL-R41, FD-R14

---

#### KP-R19: Abschluss-Checklist durchgehen
**Originaltext:**
> Am Ende jeder Feature-Implementierung:
> - [ ] Alle Checkboxen in `USER-STORY.md` abgehakt
> - [ ] DoD durchgeführt (ruff, pytest)
> - [ ] `documentation.md` aktualisiert
> - [ ] Changelog in `keypi_jqe/res/changelog/` aktualisiert
> - [ ] `KEYPIRINHA-LEARNINGS.md` mit neuen Erkenntnissen aktualisiert
> - [ ] Manuelle Tests durchgeführt
> - [ ] Kurze Zusammenfassung erstellt (was geändert, wie getestet)

**Kontext:** Definition of Done

---

#### KP-R20: PEP 8 strikt befolgen
**Originaltext:**
> - **PEP 8** Style Guide strikt befolgen

**Kontext:** Code-Standards / Python-Konventionen

**Widerspruch/Duplikat:** Identisch mit CL-R22 und FD-R07

---

#### KP-R21: Code und Kommentare auf Englisch
**Originaltext:**
> - Code und Kommentare im Quellcode **nur in Englisch**

**Kontext:** Code-Standards / Python-Konventionen

**Widerspruch/Duplikat:** Identisch mit CL-R18 und FD-R08

---

#### KP-R22: Git-Commits auf Englisch
**Originaltext:**
> - Git-Commits **nur in Englisch**

**Kontext:** Code-Standards / Python-Konventionen

**Widerspruch/Duplikat:** Identisch mit CL-R17 und FD-R09

---

#### KP-R23: Type Hints wo sinnvoll
**Originaltext:**
> - Type Hints wo sinnvoll verwenden

**Kontext:** Code-Standards / Python-Konventionen

**Widerspruch/Duplikat:** Identisch mit CL-R25 und FD-R10

---

#### KP-R24: Docstrings für öffentliche Methoden
**Originaltext:**
> - Docstrings für alle öffentlichen Methoden

**Kontext:** Code-Standards / Python-Konventionen

**Widerspruch/Duplikat:** Identisch mit CL-R26 und FD-R11

---

#### KP-R25: Plugin-Klasse muss von keypirinha.plugin.Plugin erben
**Originaltext:**
> - Plugin-Klasse **muss** von `keypirinha.plugin.Plugin` erben

**Kontext:** Code-Standards / Keypirinha-Spezifisch (projektspezifisch)

---

#### KP-R26: Pflicht-Methoden implementieren
**Originaltext:**
> - Pflicht-Methoden implementieren:
>   - `on_catalog()` - Catalog Items registrieren
>   - `on_suggest()` - User Input verarbeiten
>   - `on_execute()` - Aktionen ausführen
>   - `on_events()` - Config-Changes behandeln

**Kontext:** Code-Standards / Keypirinha-Spezifisch (projektspezifisch)

---

#### KP-R27: Niemals UI blockieren – Async/Non-blocking für API-Calls
**Originaltext:**
> - **NIEMALS** UI blockieren - Async/Non-blocking für API-Calls

**Kontext:** Code-Standards / Keypirinha-Spezifisch

---

#### KP-R28: kp.shell_execute() für Browser-URLs nutzen
**Originaltext:**
> - `kp.shell_execute()` für Browser-URLs nutzen

**Kontext:** Code-Standards / Keypirinha-Spezifisch (projektspezifisch)

---

#### KP-R29: Keypirinha übernimmt Live-Filtering
**Originaltext:**
> - Keypirinha übernimmt Live-Filtering - nur Daten bereitstellen

**Kontext:** Code-Standards / Keypirinha-Spezifisch (projektspezifisch)

---

#### KP-R30: Umfassendes Error Handling für API-Requests
**Originaltext:**
> - Umfassendes Error Handling für API-Requests

**Kontext:** Code-Standards / Error Handling

---

#### KP-R31: Benutzerfreundliche Fehlermeldungen in Keypirinha anzeigen
**Originaltext:**
> - Benutzerfreundliche Fehlermeldungen in Keypirinha anzeigen

**Kontext:** Code-Standards / Error Handling (projektspezifisch)

---

#### KP-R32: Logging für Debugging implementieren
**Originaltext:**
> - Logging für Debugging implementieren (`self.info()`, `self.warn()`, `self.err()`)

**Kontext:** Code-Standards / Error Handling

---

#### KP-R33: Network Errors abfangen
**Originaltext:**
> - Network Errors abfangen (Timeout, Offline)

**Kontext:** Code-Standards / Error Handling

---

#### KP-R34: Rate Limiting behandeln
**Originaltext:**
> - Rate Limiting behandeln (429 Fehler)

**Kontext:** Code-Standards / Error Handling (projektspezifisch)

---

#### KP-R35: Auth-Fehler klar kommunizieren
**Originaltext:**
> - Auth-Fehler klar kommunizieren (401/403)

**Kontext:** Code-Standards / Error Handling (projektspezifisch)

---

#### KP-R36: Nur über lib/jira_client.py API-Kommunikation
**Originaltext:**
> - **NUR** über `lib/jira_client.py`, **NICHT** direkt in Plugin-Klasse

**Kontext:** Code-Standards / API-Kommunikation (projektspezifisch)

---

#### KP-R37: JQL-Syntax vor API-Request validieren
**Originaltext:**
> - JQL-Syntax validieren vor API-Request

**Kontext:** Code-Standards / API-Kommunikation (projektspezifisch)

---

#### KP-R38: Timeouts für API-Requests setzen
**Originaltext:**
> - Timeouts für API-Requests setzen (aktuell: 10 Sekunden)

**Kontext:** Code-Standards / API-Kommunikation (projektspezifisch)

---

#### KP-R39: Custom Exceptions nutzen
**Originaltext:**
> - Custom Exceptions nutzen (`JiraAuthError`, `JiraAPIError`, `JiraNetworkError`)

**Kontext:** Code-Standards / API-Kommunikation (projektspezifisch)

---

#### KP-R40: API-Keys nicht im Code hardcoden
**Originaltext:**
> - ❌ API-Keys im Code hardcoden oder unverschlüsselt commiten

**Kontext:** Häufige Fehler vermeiden / NICHT tun (projektspezifisch)

---

#### KP-R41: UI während API-Calls nicht blockieren
**Originaltext:**
> - ❌ UI während API-Calls blockieren

**Kontext:** Häufige Fehler vermeiden / NICHT tun

**Widerspruch/Duplikat:** Identisch mit KP-R27

---

#### KP-R42: JQL vor API-Request validieren
**Originaltext:**
> - ❌ Ohne Validierung JQL an API senden

**Kontext:** Häufige Fehler vermeiden / NICHT tun (projektspezifisch)

**Widerspruch/Duplikat:** Identisch mit KP-R37

---

#### KP-R43: 401/403 Fehler nicht ignorieren
**Originaltext:**
> - ❌ 401/403 Fehler ignorieren

**Kontext:** Häufige Fehler vermeiden / NICHT tun

---

#### KP-R44: on_deactivate() Cleanup nicht vergessen
**Originaltext:**
> - ❌ `on_deactivate()` Cleanup vergessen

**Kontext:** Häufige Fehler vermeiden / NICHT tun (projektspezifisch)

---

#### KP-R45: Nicht direkt auf main committen/pushen
**Originaltext:**
> - ❌ Direkt auf `main` committen/pushen

**Kontext:** Häufige Fehler vermeiden / NICHT tun

**Widerspruch/Duplikat:** Identisch mit CL-R15, CL-R38, FD-R18

---

#### KP-R46: Veraltete API-Endpunkte nicht nutzen
**Originaltext:**
> - ❌ Veraltete API-Endpunkte nutzen

**Kontext:** Häufige Fehler vermeiden / NICHT tun (projektspezifisch)

---

#### KP-R47: Von keypirinha.plugin.Plugin erben
**Originaltext:**
> - ✅ Von `keypirinha.plugin.Plugin` erben

**Kontext:** Häufige Fehler vermeiden / IMMER tun

**Widerspruch/Duplikat:** Identisch mit KP-R25

---

#### KP-R48: Alle erforderlichen Methoden implementieren
**Originaltext:**
> - ✅ Alle erforderlichen Methoden implementieren

**Kontext:** Häufige Fehler vermeiden / IMMER tun

**Widerspruch/Duplikat:** Identisch mit KP-R26

---

#### KP-R49: Netzwerkfehler und Rate Limiting behandeln
**Originaltext:**
> - ✅ Netzwerkfehler und Rate Limiting behandeln

**Kontext:** Häufige Fehler vermeiden / IMMER tun

**Widerspruch/Duplikat:** Kombination aus KP-R33 und KP-R34

---

#### KP-R50: Sinnvolle Defaults in Beispiel-Config bereitstellen
**Originaltext:**
> - ✅ Sinnvolle Defaults in Beispiel-Config bereitstellen

**Kontext:** Häufige Fehler vermeiden / IMMER tun (projektspezifisch)

---

#### KP-R51: .ini Dateien mit Credentials in .gitignore eintragen
**Originaltext:**
> - ✅ `.ini` Dateien mit Credentials in `.gitignore` eintragen

**Kontext:** Häufige Fehler vermeiden / IMMER tun (projektspezifisch)

---

#### KP-R52: DoD vor jedem Commit durchführen
**Originaltext:**
> - ✅ DoD durchführen vor jedem Commit

**Kontext:** Häufige Fehler vermeiden / IMMER tun

**Widerspruch/Duplikat:** Identisch mit CL-R39

---

#### KP-R53: In neuem Branch arbeiten (Format: claude/*)
**Originaltext:**
> - ✅ In neuem Branch arbeiten (Format: `claude/*`)

**Kontext:** Häufige Fehler vermeiden / IMMER tun

**Widerspruch/Duplikat:** Ähnlich zu CL-R16

---

#### KP-R54: Code und Kommentare in Englisch
**Originaltext:**
> - ✅ Code und Kommentare in Englisch

**Kontext:** Häufige Fehler vermeiden / IMMER tun

**Widerspruch/Duplikat:** Identisch mit CL-R18, FD-R08, KP-R21

---

#### KP-R55: Main Branch ist geschützt
**Originaltext:**
> - **Main Branch**: `main` (geschützt)

**Kontext:** Git Workflow / Branch-Strategie

**Widerspruch/Duplikat:** Identisch mit CL-R15 und FD-R16

---

#### KP-R56: Feature Branches mit Namenskonvention
**Originaltext:**
> - **Feature Branches**: `claude/<feature-name>-<session-id>`

**Kontext:** Git Workflow / Branch-Strategie

**Widerspruch/Duplikat:** Identisch mit FD-R17

---

#### KP-R57: Niemals direkt auf main pushen
**Originaltext:**
> - **NIEMALS** direkt auf `main` pushen

**Kontext:** Git Workflow / Branch-Strategie

**Widerspruch/Duplikat:** Identisch mit CL-R15, CL-R38, FD-R18, KP-R45

---

#### KP-R58: Klare, beschreibende Commit-Messages (Englisch)
**Originaltext:**
> - Commits: Klare, beschreibende Messages (Englisch)

**Kontext:** Git Workflow / Branch-Strategie

**Widerspruch/Duplikat:** Identisch mit CL-R17 und FD-R19

---

#### KP-R59: Conventional Commits verwenden
**Originaltext:**
> - Conventional Commits verwenden (feat:, fix:, docs:, chore:)

**Kontext:** Git Workflow / Branch-Strategie

**Widerspruch/Duplikat:** Identisch mit FD-R20

---

#### KP-R60: Semantic Versioning einhalten
**Originaltext:**
> - **Semantic Versioning**: `v{major}.{minor}.{patch}` (z.B. v1.2.0)

**Kontext:** Git Workflow / Versioning & Git Tags

---

#### KP-R61: Jedes Feature-Release auf main bekommt einen Tag
**Originaltext:**
> - Jedes Feature-Release auf `main` bekommt einen Tag

**Kontext:** Git Workflow / Versioning & Git Tags

---

#### KP-R62: Minor-Version +1 pro Feature
**Originaltext:**
> - Minor-Version wird +1 erhöht pro Feature (v1.1.0 → v1.2.0)

**Kontext:** Git Workflow / Versioning & Git Tags

**Widerspruch/Duplikat:** Identisch mit KP-R08

---

#### KP-R63: Tags automatisch von Claude erstellen
**Originaltext:**
> - Tags werden automatisch von Claude erstellt (bei Beginn einer neuen User Story)

**Kontext:** Git Workflow / Versioning & Git Tags

---

#### KP-R64: Git Tags prüfen bei neuer User Story
**Originaltext:**
> # Claude prüft bei jeder neuen User Story:
> git tag --list  # Checkt existierende Tags

**Kontext:** Git Workflow / Versioning & Git Tags / Tag-Erstellung

**Widerspruch/Duplikat:** Ähnlich zu KP-R05

---

#### KP-R65: Tag erstellen falls fehlend
**Originaltext:**
> # Falls Tag für letzten Merge fehlt:
> git tag -a v1.X.0 -m "Release v1.X.0: Feature Name"

**Kontext:** Git Workflow / Versioning & Git Tags / Tag-Erstellung

**Widerspruch/Duplikat:** Ähnlich zu KP-R06

---

#### KP-R66: Tag zu Remote pushen
**Originaltext:**
> git push origin v1.X.0  # Push to remote

**Kontext:** Git Workflow / Versioning & Git Tags / Tag-Erstellung

---

---

## Zusammenfassung

**Gesamtzahl extrahierter Regeln:** 132

**Duplikate/Widersprüche markiert:** 40+

**Quellen:** 4 Dateien

**Status:** Vollständige Inventarisierung abgeschlossen

---

## Nächste Schritte

Nach Abschluss von Phase 1 folgt:
- **Phase 2:** Klassifikation der Regeln
- **Phase 3:** Intent & Risiko-Analyse
