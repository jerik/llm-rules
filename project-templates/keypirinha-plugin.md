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
