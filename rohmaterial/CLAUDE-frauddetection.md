# Project 
Finanzanalyse und Frauddetection System

## Vorgehen
Lies idee.md
Lies user-story.md sofern vorhanden
Lies backlog.md sofern vorhanden
Im archive sind die umgesetzten anforderungen enthalten, dort kannst du dich über die bisherigen Schritte informieren

## Tooling
- python verwenden
- uv verwenden

## Coding Standards 
- PEP 8 Style Guide strikt befolgen
- Code und Kommentare im Quellcode nur in Englisch
- Git-Commits nur in Englisch
- Type Hints wo sinnvoll verwenden
- Docstrings für alle öffentlichen Methoden

## Definition of Done
- Linting mittels ruff
- Pytest passende Testfälle erstellen und ausführen 
- Wenn checks fehlschlagen: iteriere bis alle grün sind!
- kurze Zusammenfassung erstellen (was wurde getan, was hat sich geändert, wie getestet)

## Git Workflow
- Main Branch: main (geschützt)
- Feature Branches: claude/<feature-name>-<session-id>
- NIEMALS direkt auf main pushen
- Commits: Klare, beschreibende Messages (Englisch)
- Conventional Commits verwenden (feat:, fix:, docs:, chore:)

## Development Workflow
- Testing: `uv run pytest -v` für Unit-Tests
- Vor Commits: `uv run ruff check analyze_transactions.py` ausführen

## Database Management
- Hauptdatenbank: `analysis_output/transactions.db`

## Kommunikation
- Inhalte und prompts kritsch analysieren.
- Frage stellen sofern unklare Informationen vorliegen
- Fragen mit f1...fn bennen
- In der Kommunikation Präzise und kurz halten, ausser user fragt explizit nach ausführlicher Erklärung oder Antwort nach. style tl;dr
- Vorgehen im Dialog 
