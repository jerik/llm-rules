# Vim Plugin

Template für Vim-Plugin-Entwicklung.

## Plugin-Struktur

### Verzeichnis-Layout
- `plugin/` - Minimal halten (nur Wiring)
- `autoload/` - Komplexe Logik hier ablegen
- `test/` - Test-Dateien

### Scoping
- MUST: Interne Helfer mit `s:`-Scope
- MUST: Globaler State nur für `g:plugin_name_*` Config-Variablen
- Begründung: Namespace-Kollisionen vermeiden

### Autocmds
- MUST: Autocmds in dedizierter augroup mit `autocmd!`
- Beispiel:
  ```vim
  augroup MyPlugin
    autocmd!
    autocmd BufRead *.txt call myfunction()
  augroup END
  ```

## Tooling

### vint
- MUST: vint für Linting verwenden
- MUST: Zero warnings akzeptiert
- Command:
  ```bash
  vint plugin/
  ```

### Tests
- MUST: Headless Tests ausführen
- Command:
  ```bash
  vim -Nu NONE -n -es -S test/run.vim
  ```
- Assertions: `assert_equal()`, `assert_match()`, `assert_true()`, `assert_fails()`
- Layout: `test/run.vim` + `test/test_*.vim`
- Isolation: Temp-Verzeichnisse, kein User-vimrc

## Definition of Done (Beispiel)

```bash
# 1. Vimscript Linting
vint plugin/

# 2. Tests
vim -Nu NONE -n -es -S test/run.vim

# Bei Fehler: Iterieren bis alle grün!
```

## Projekt-spezifische Regeln

### Generierte Dateien
- MUST NOT: Generierte Dateien editieren
- Beispiel: `mindboxes/*.mb` (projektspezifisch)
- Begründung: Änderungen gehen bei Regenerierung verloren
