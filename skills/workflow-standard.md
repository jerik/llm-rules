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
