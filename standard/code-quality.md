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
- MUST: Dokumentation in markdown
- Begründung: Verhindert Verwirrung, Fehlannahmen

## Sprache
- MUST: Code und Kommentare nur in Englisch
- Begründung: Internationale Zusammenarbeit, Ökosystem-Standard
