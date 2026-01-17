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
- claude-on-web: Datenzugriff auf das freigegebene git repository

### Persistenz
- pi: Session-übergreifend persistent
- claude-on-web: Session-lokal
