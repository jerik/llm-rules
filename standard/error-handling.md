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
