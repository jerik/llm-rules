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
