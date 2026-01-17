# LLM Rules - Anbieterunabhängiges Regelwerk für LLM-Nutzung

Versionierbares, strukturiertes Regelwerk für die Nutzung von Large Language Models in Softwareprojekten.

**Ausführungsumgebung:** pi-mono (pi coding agent)

---

## Überblick

Dieses Regelwerk definiert Standards, Skills und Templates für die Arbeit mit LLMs in Entwicklungsprojekten. Es wurde durch systematische Analyse bestehender Projektvorgaben erstellt und ist anbieterunabhängig gestaltet.

### Aufbau

```
llm-rules/
├── standard/              # Grundsätzlich geltende Regeln
├── skills/                # Wiederverwendbare Workflow-Muster
├── project-templates/     # Technologie-spezifische Templates
├── context/               # Kontextuelle Informationen
└── archive/               # Artefakter älterer iterationen
```

---

## Standard-Regeln

Grundsätzlich geltende Regeln, unabhängig von Projekt oder Technologie.

| Regelwerk | Beschreibung | Datei |
|-----------|--------------|-------|
| **Communication** | Rückfragen, Frageformat (f1...fn), Antwortstil (tl;dr), kritische Analyse | [standard/communication.md](standard/communication.md) |
| **Git Workflow** | Branch-Strategie, geschützter Main, Commit-Messages, Conventional Commits | [standard/git-workflow.md](standard/git-workflow.md) |
| **Code Quality** | Diff-Größe, Tests, Rückwärtskompatibilität, Dokumentation | [standard/code-quality.md](standard/code-quality.md) |
| **Error Handling & Security** | Error Handling, Logging, Timeouts, Credentials-Management | [standard/error-handling.md](standard/error-handling.md) |
| **Versioning** | Semantic Versioning | [standard/versioning.md](standard/versioning.md) |

---

## Skills

Wiederverwendbare Workflow-Muster, die projektspezifisch aktiviert werden können.

| Skill | Beschreibung | Datei |
|-------|--------------|-------|
| **Standard-Workflow** | Dokumentation lesen, Dialog & Verfeinerung, DoD, Review | [skills/workflow-standard.md](skills/workflow-standard.md) |
| **Fortschritt-Tracking** | Checkboxen aktualisieren, Zusammenfassungen erstellen | [skills/fortschritt-tracking.md](skills/fortschritt-tracking.md) |
| **Synergie-Analyse** | BACKLOG.md konsultieren, Zusammenhänge erkennen | [skills/synergie-analyse.md](skills/synergie-analyse.md) |
| **Git Tag Management** | Automatisches Tag-Management für Releases (optional) | [skills/git-tag-management.md](skills/git-tag-management.md) |

---

## Project Templates

Technologie-spezifische Templates für verschiedene Stacks.

| Template | Beschreibung | Datei |
|----------|--------------|-------|
| **Python Stack** | PEP 8, ruff, pytest, uv, Type Hints | [project-templates/python-stack.md](project-templates/python-stack.md) |
| **Vim Plugin** | Plugin-Struktur, vint, Tests, Scoping | [project-templates/vim-plugin.md](project-templates/vim-plugin.md) |
| **Keypirinha Plugin** | Keypirinha-API, UI-Responsiveness, Error Handling | [project-templates/keypirinha-plugin.md](project-templates/keypirinha-plugin.md) |

---

## Context

Kontextuelle Informationen für spezifische Ausführungsumgebungen.

| Context | Beschreibung | Datei |
|---------|--------------|-------|
| **Execution Environment** | pi (Shell) vs. claude-on-web (Browser), Branch-Naming | [context/execution-environment.md](context/execution-environment.md) |

---

## Nutzung

### In Projekten

1. **Standard-Regeln:** Gelten automatisch für alle Projekte
2. **Skills:** Explizit in Projektvorgaben aktivieren (z.B. in `CLAUDE.md`)
3. **Templates:** Entsprechendes Template für Technologie-Stack referenzieren

### Beispiel: Python-Projekt mit Standard-Workflow

In `CLAUDE.md` des Projekts:

```markdown
# Project Rules

## LLM Rules
- Standard: Alle Regeln aus llm-rules/standard/
- Skills: workflow-standard, fortschritt-tracking
- Template: python-stack

## Project-specific
- [Projektspezifische Ergänzungen hier]
```

---

## Dokumentation des Entstehungsprozesses

Das Regelwerk wurde durch einen strukturierten Prozess entwickelt:

1. **Phase 1 - Inventarisierung:** 132 Regeln aus 4 Quellen extrahiert
2. **Phase 2 - Klassifikation:** Regeln nach Gültigkeitsbereich kategorisiert
3. **Phase 3 - Intent & Risiko:** Zweck und Risiken jeder Regel analysiert
4. **Phase 4 - Zielbild:** Regeln konsolidiert und strukturiert

Detaillierte Dokumentation:
- [inventar.md](inventar.md) - Vollständige Regelextraktion
- [klassifikation.md](klassifikation.md) - Kategorisierung
- [intent-risiko.md](intent-risiko.md) - Zweck- und Risikoanalyse
- [zielbild-kandidaten.md](zielbild-kandidaten.md) - Konsolidierung
- [plan.md](plan.md) - Projektsteuerung

---

## Prinzipien

### Anbieterunabhängigkeit
- Keine Annahmen über spezifische LLM-Anbieter (Claude, GPT, etc.)
- Fokus auf Verhaltensregeln, nicht auf Modell-Features

### Versionierbarkeit
- Klare Struktur in Git
- Nachvollziehbare Änderungen
- Semantic Versioning für Releases

### Modularität
- Standard-Regeln: Immer gültig
- Skills: Optional aktivierbar
- Templates: Technologie-spezifisch

### Explizitheit
- MUST / MUST NOT / SHOULD / SHOULD NOT
- Keine impliziten Annahmen
- Begründungen für kritische Regeln

---

## Mitwirkende

Dieses Regelwerk wurde durch systematische Analyse und Konsolidierung bestehender Projektvorgaben entwickelt.

---

## Lizenz

[Noch zu definieren]

---

## Version

**Aktuell:** 1.0.0 (2025-01-17)

Erster Release nach Abschluss der Rekonstruktionsphase.
