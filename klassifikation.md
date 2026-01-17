# Klassifikation – Phase 2

Einordnung aller extrahierten Regeln nach Gültigkeitsbereich.

**Status:** In Arbeit  
**Erstellt:** 2025-01-17  
**Letzte Aktualisierung:** 2025-01-17

---

## Klassifikationskategorien

### Standard-Vorgabe
Regeln, die grundsätzlich für alle LLM-Interaktionen gelten sollen.
Unabhängig von Projekt, Technologie oder spezifischem Kontext.

### Skill
Regeln, die ein wiederverwendbares Verhaltensmuster beschreiben,
das in verschiedenen Projekten aktiviert werden kann.

### Projekt-spezifisch
Regeln, die nur in einem bestimmten Projekt oder für eine bestimmte Technologie gelten.
Nicht übertragbar auf andere Kontexte.

### Unklar / Kontextabhängig
Regeln, deren Gültigkeitsbereich nicht eindeutig bestimmbar ist
oder die weitere Klärung benötigen.

---

## Klassifikation der Regeln

### Quelle: default-prompt

| Regel-ID | Kategorie | Begründung |
|----------|-----------|------------|
| DP-R01 | Standard | Datenanalyse ist grundsätzliches Verhalten |
| DP-R02 | Standard | Rückfragen bei Unklarheiten ist grundsätzlich |
| DP-R03 | Standard | Kommunikationsformat – allgemein anwendbar |
| DP-R04 | Standard | Strukturierung von Fragen – allgemein anwendbar |
| DP-R05 | Standard | Antwortformat – grundsätzlich |
| DP-R06 | Standard | Antwortstil – grundsätzlich |
| DP-R07 | Standard | Identisch mit DP-R01 (Duplikat) |
| DP-R08 | Standard | Identisch mit DP-R02 (Duplikat) |
| DP-R09 | Standard | Identisch mit DP-R04 (Duplikat) |
| DP-R10 | Standard | Identisch mit DP-R05 (Duplikat) |
| DP-R11 | Standard | Identisch mit DP-R06 (Duplikat) |
| DP-R12 | Standard | Klare Sprache – grundsätzlich |

---

### Quelle: CLAUDE.md (Mindbox-Projekt)

| Regel-ID | Kategorie | Begründung |
|----------|-----------|------------|
| CL-R01 | Skill | Workflow: CLAUDE.md lesen ist wiederverwendbares Muster |
| CL-R02 | Skill | Workflow: User Story Erstellung ist Prozessschritt |
| CL-R03 | Skill | Workflow: Dialog-basierte Verfeinerung |
| CL-R04 | Skill | Workflow: Explizite Freigabe durch User |
| CL-R05 | Skill | Workflow: Dokumentation lesen vor Implementierung |
| CL-R06 | Skill | Workflow: Checkboxen aktualisieren ist Prozessschritt |
| CL-R07 | Skill | Workflow: DoD ausführen ist Prozessschritt |
| CL-R08 | Skill | Workflow: Review-Phase |
| CL-R09 | Projekt-spezifisch | Python + ruff – technologiespezifisch |
| CL-R10 | Projekt-spezifisch | Python + ruff – technologiespezifisch |
| CL-R11 | Projekt-spezifisch | Python + pytest – technologiespezifisch |
| CL-R12 | Projekt-spezifisch | Vimscript + vint – technologiespezifisch |
| CL-R13 | Projekt-spezifisch | Vim-Tests – technologiespezifisch |
| CL-R14 | Skill | Iterieren bis Checks grün – wiederverwendbares Muster |
| CL-R15 | Standard | Git-Workflow: geschützter Main-Branch ist Best Practice |
| CL-R16 | Unklar | Branch-Namenskonvention – könnte Standard oder Projekt sein |
| CL-R17 | Standard | Englische Commits – allgemein anwendbar |
| CL-R18 | Standard | Englischer Code – allgemein anwendbar |
| CL-R19 | Standard | Kleine Diffs – allgemein anwendbar |
| CL-R20 | Standard | Tests anpassen bei Änderung – allgemein anwendbar |
| CL-R21 | Standard | Rückwärtskompatibilität – allgemein anwendbar |
| CL-R22 | Projekt-spezifisch | PEP 8 – Python-spezifisch |
| CL-R23 | Projekt-spezifisch | ruff – technologiespezifisch |
| CL-R24 | Projekt-spezifisch | pytest – technologiespezifisch |
| CL-R25 | Projekt-spezifisch | Type Hints – Python-spezifisch |
| CL-R26 | Standard | Docstrings Prinzip ist allgemein, Umsetzung sprachspezifisch |
| CL-R27 | Projekt-spezifisch | Vim-Plugin-Struktur – technologiespezifisch |
| CL-R28 | Projekt-spezifisch | Vim-Plugin-Struktur – technologiespezifisch |
| CL-R29 | Projekt-spezifisch | Vimscript-Scope – technologiespezifisch |
| CL-R30 | Projekt-spezifisch | Vimscript-Config – technologiespezifisch |
| CL-R31 | Projekt-spezifisch | Vimscript-Autocmds – technologiespezifisch |
| CL-R32 | Projekt-spezifisch | vint – technologiespezifisch |
| CL-R33 | Projekt-spezifisch | Duplikat von CL-R13 |
| CL-R34 | Projekt-spezifisch | Vim-Test-Assertions – technologiespezifisch |
| CL-R35 | Projekt-spezifisch | Vim-Test-Layout – technologiespezifisch |
| CL-R36 | Projekt-spezifisch | Vim-Test-Isolation – technologiespezifisch |
| CL-R37 | Projekt-spezifisch | Generierte Dateien nicht editieren – projektspezifisch |
| CL-R38 | Standard | Duplikat von CL-R15 |
| CL-R39 | Skill | Duplikat von CL-R07 und CL-R14 |
| CL-R40 | Standard | Dokumentation aktualisieren – allgemein anwendbar |
| CL-R41 | Skill | Duplikat von CL-R14 |
| CL-R42 | Standard | Kritische Analyse – grundsätzlich |
| CL-R43 | Standard | Rückfragen – grundsätzlich (Duplikat DP-R02) |
| CL-R44 | Standard | Frageformat mit fn – allgemein anwendbar (Variante zu DP-R04) |
| CL-R45 | Standard | tl;dr Stil – allgemein anwendbar |
| CL-R46 | Standard | Ausführlichkeit nur auf Anfrage – Duplikat DP-R06 |

---

### Quelle: CLAUDE-frauddetection.md

| Regel-ID | Kategorie | Begründung |
|----------|-----------|------------|
| FD-R01 | Skill | Workflow: Projektdokumentation lesen |
| FD-R02 | Skill | Workflow: User Story lesen |
| FD-R03 | Skill | Workflow: Backlog lesen |
| FD-R04 | Skill | Workflow: Archive konsultieren |
| FD-R05 | Projekt-spezifisch | Python – technologiespezifisch |
| FD-R06 | Projekt-spezifisch | uv – technologiespezifisch |
| FD-R07 | Projekt-spezifisch | PEP 8 – Python-spezifisch (Duplikat CL-R22) |
| FD-R08 | Standard | Englischer Code – Duplikat CL-R18 |
| FD-R09 | Standard | Englische Commits – Duplikat CL-R17 |
| FD-R10 | Projekt-spezifisch | Type Hints – Python-spezifisch (Duplikat CL-R25) |
| FD-R11 | Standard | Docstrings – Duplikat CL-R26 |
| FD-R12 | Projekt-spezifisch | ruff – technologiespezifisch |
| FD-R13 | Projekt-spezifisch | pytest – technologiespezifisch |
| FD-R14 | Skill | Iterieren bis grün – Duplikat CL-R14 |
| FD-R15 | Skill | Zusammenfassung erstellen – wiederverwendbar |
| FD-R16 | Standard | Geschützter Main – Duplikat CL-R15 |
| FD-R17 | Unklar | Branch-Naming mit session-id – Variante zu CL-R16 |
| FD-R18 | Standard | Nicht auf main pushen – Duplikat CL-R15/CL-R38 |
| FD-R19 | Standard | Commit-Messages – Duplikat CL-R17 |
| FD-R20 | Standard | Conventional Commits – allgemein anwendbar |
| FD-R21 | Projekt-spezifisch | uv run pytest – technologiespezifisch |
| FD-R22 | Projekt-spezifisch | uv run ruff – technologiespezifisch |
| FD-R23 | Projekt-spezifisch | Spezifische Datenbankdatei – projektspezifisch |
| FD-R24 | Standard | Kritische Analyse – Duplikat CL-R42 |
| FD-R25 | Standard | Rückfragen – Duplikat DP-R02/CL-R43 |
| FD-R26 | Standard | Frageformat fn – Duplikat CL-R44 |
| FD-R27 | Standard | tl;dr Stil – Duplikat CL-R45 |
| FD-R28 | Unklar | "Vorgehen im Dialog" – unklar, möglicherweise unvollständig |

---

### Quelle: CLAUDE-keypy.md

| Regel-ID | Kategorie | Begründung |
|----------|-----------|------------|
| KP-R01 | Skill | Workflow: CLAUDE.md lesen – Duplikat CL-R01 |
| KP-R02 | Skill | Workflow: USER-STORY.md lesen – Duplikat CL-R05 |
| KP-R03 | Skill | Workflow: BACKLOG.md lesen – Duplikat CL-R05 |
| KP-R04 | Skill | Workflow: Learnings-Datei lesen – projektspezifisches Muster |
| KP-R05 | Skill | Workflow: Git Tags prüfen |
| KP-R06 | Skill | Workflow: Tags erstellen |
| KP-R07 | Standard | Tag-Format – allgemein anwendbar (Semantic Versioning) |
| KP-R08 | Skill | Versioning-Strategie – wiederverwendbar |
| KP-R09 | Skill | Checkboxen aktualisieren – Duplikat CL-R06 |
| KP-R10 | Skill | Code-Standards einhalten – allgemein, aber unspezifisch |
| KP-R11 | Skill | DoD durchführen – Duplikat CL-R07 |
| KP-R12 | Skill | Dokumentation aktualisieren – wiederverwendbar |
| KP-R13 | Skill | Backlog mitlesen für Synergien |
| KP-R14 | Skill | Synergien ansprechen |
| KP-R15 | Projekt-spezifisch | ruff check – technologiespezifisch (Duplikat CL-R10) |
| KP-R16 | Projekt-spezifisch | ruff format – technologiespezifisch (Duplikat CL-R09) |
| KP-R17 | Projekt-spezifisch | pytest – technologiespezifisch (Duplikat CL-R11) |
| KP-R18 | Skill | Iterieren bis grün – Duplikat CL-R14 |
| KP-R19 | Skill | Abschluss-Checklist – detaillierte Variante von CL-R07 |
| KP-R20 | Projekt-spezifisch | PEP 8 – Python-spezifisch (Duplikat CL-R22/FD-R07) |
| KP-R21 | Standard | Englischer Code – Duplikat CL-R18/FD-R08 |
| KP-R22 | Standard | Englische Commits – Duplikat CL-R17/FD-R09 |
| KP-R23 | Projekt-spezifisch | Type Hints – Python-spezifisch (Duplikat CL-R25/FD-R10) |
| KP-R24 | Standard | Docstrings – Duplikat CL-R26/FD-R11 |
| KP-R25 | Projekt-spezifisch | Keypirinha Plugin-Klasse – technologiespezifisch |
| KP-R26 | Projekt-spezifisch | Keypirinha Pflicht-Methoden – technologiespezifisch |
| KP-R27 | Projekt-spezifisch | UI nicht blockieren – Keypirinha-spezifisch |
| KP-R28 | Projekt-spezifisch | kp.shell_execute – Keypirinha-spezifisch |
| KP-R29 | Projekt-spezifisch | Keypirinha Filtering – technologiespezifisch |
| KP-R30 | Standard | Error Handling – allgemein anwendbar |
| KP-R31 | Projekt-spezifisch | Keypirinha Fehlermeldungen – technologiespezifisch |
| KP-R32 | Standard | Logging für Debugging – allgemein anwendbar |
| KP-R33 | Standard | Network Errors abfangen – allgemein anwendbar |
| KP-R34 | Projekt-spezifisch | Rate Limiting für Jira API – projektspezifisch |
| KP-R35 | Projekt-spezifisch | Auth-Fehler für Jira – projektspezifisch |
| KP-R36 | Projekt-spezifisch | jira_client.py – projektspezifisch |
| KP-R37 | Projekt-spezifisch | JQL-Validierung – projektspezifisch |
| KP-R38 | Standard | API Timeouts – allgemein anwendbar |
| KP-R39 | Projekt-spezifisch | Custom Exceptions für Jira – projektspezifisch |
| KP-R40 | Standard | API-Keys nicht hardcoden – allgemein anwendbar |
| KP-R41 | Projekt-spezifisch | UI nicht blockieren – Duplikat KP-R27 |
| KP-R42 | Projekt-spezifisch | JQL validieren – Duplikat KP-R37 |
| KP-R43 | Standard | Auth-Fehler nicht ignorieren – allgemein anwendbar |
| KP-R44 | Projekt-spezifisch | on_deactivate Cleanup – Keypirinha-spezifisch |
| KP-R45 | Standard | Nicht auf main pushen – Duplikat CL-R15/CL-R38/FD-R18 |
| KP-R46 | Projekt-spezifisch | Aktuelle API-Endpunkte – projektspezifisch |
| KP-R47 | Projekt-spezifisch | Plugin erben – Duplikat KP-R25 |
| KP-R48 | Projekt-spezifisch | Methoden implementieren – Duplikat KP-R26 |
| KP-R49 | Standard | Netzwerkfehler behandeln – Duplikat KP-R33/KP-R34 |
| KP-R50 | Standard | Sinnvolle Defaults – allgemein anwendbar |
| KP-R51 | Standard | Credentials in .gitignore – allgemein anwendbar |
| KP-R52 | Skill | DoD vor Commit – Duplikat CL-R39 |
| KP-R53 | Unklar | Branch-Naming claude/* – Variante zu CL-R16 |
| KP-R54 | Standard | Englischer Code – Duplikat CL-R18/FD-R08/KP-R21 |
| KP-R55 | Standard | Geschützter Main – Duplikat CL-R15/FD-R16 |
| KP-R56 | Unklar | Branch-Naming mit session-id – Duplikat FD-R17 |
| KP-R57 | Standard | Nicht auf main pushen – Duplikat (mehrfach) |
| KP-R58 | Standard | Commit-Messages – Duplikat CL-R17/FD-R19 |
| KP-R59 | Standard | Conventional Commits – Duplikat FD-R20 |
| KP-R60 | Standard | Semantic Versioning – allgemein anwendbar |
| KP-R61 | Skill | Tag pro Feature-Release – Versioning-Strategie |
| KP-R62 | Skill | Minor +1 pro Feature – Duplikat KP-R08 |
| KP-R63 | Skill | Tags automatisch erstellen – Workflow |
| KP-R64 | Skill | Git Tags prüfen – Duplikat KP-R05 |
| KP-R65 | Skill | Tag erstellen – Duplikat KP-R06 |
| KP-R66 | Skill | Tag pushen – Workflow-Detail |

---

## Zusammenfassung nach Kategorien

### Standard-Vorgaben (ca. 35 unique)
Regeln, die grundsätzlich gelten:
- Kommunikation (Rückfragen, tl;dr, Frageformat)
- Git-Workflow (geschützter Main, englische Commits, Conventional Commits)
- Code-Qualität (englischer Code, Dokumentation, kleine Diffs, Rückwärtskompatibilität)
- Error Handling (Network Errors, Timeouts, Auth-Fehler)
- Security (API-Keys nicht hardcoden, Credentials in .gitignore)
- Versioning (Semantic Versioning)

### Skills (ca. 25 unique)
Wiederverwendbare Workflow-Muster:
- Dokumentation lesen vor Start (CLAUDE.md, USER-STORY.md, BACKLOG.md)
- Dialog-basierte Verfeinerung
- DoD ausführen
- Iterieren bis Checks grün
- Checkboxen aktualisieren
- Zusammenfassung erstellen
- Git Tag Management
- Synergien aus Backlog erkennen

### Projekt-spezifisch (ca. 60)
Technologie- oder projektgebundene Regeln:
- Python-spezifisch (PEP 8, Type Hints, pytest, ruff, uv)
- Vim/Vimscript-spezifisch (vint, Plugin-Struktur, Scopes)
- Keypirinha-spezifisch (Plugin-API, Methoden, UI-Handling)
- Jira-spezifisch (JQL, API-Endpunkte, Custom Exceptions)
- Projektspezifische Dateien/Pfade

### Unklar / Kontextabhängig (ca. 5)
Regeln, die weitere Klärung benötigen:
- Branch-Namenskonventionen (claude/*, mit/ohne session-id)
- FD-R28 ("Vorgehen im Dialog" – unvollständig)

---

## Anmerkungen

### Duplikate
Viele Regeln wiederholen sich über Quellen hinweg:
- Kommunikationsregeln (tl;dr, Rückfragen, Frageformat)
- Git-Workflow (Main geschützt, englische Commits)
- Python-Standards (PEP 8, Type Hints, Docstrings)
- DoD-Ausführung und Iterieren

Dies deutet darauf hin, dass diese Regeln als **Standard** etabliert werden sollten.

### Technologie-Cluster
Klare Cluster erkennbar:
- **Python-Stack:** PEP 8, ruff, pytest, Type Hints
- **Vim-Stack:** vint, Plugin-Struktur, Scopes
- **Keypirinha-Stack:** Plugin-API, UI-Handling

Diese sollten als **Skills** oder **Projekt-Templates** organisiert werden.

### Workflow-Muster
Wiederkehrende Workflow-Schritte:
- Dokumentation lesen
- Dialog führen
- Implementieren
- DoD ausführen
- Review

Dies könnte als **Standard-Workflow-Skill** definiert werden.

---

**Status:** Klassifikation abgeschlossen  
**Datum:** 2025-01-17

**Nächster Schritt:** Phase 3 – Intent & Risiko
