# Git Workflow

## Branch-Strategie

### Main Branch
- MUST: Main Branch ist geschützt
- MUST NOT: Niemals direkt auf main pushen oder committen
- MUST: Alle Änderungen über Feature-Branches

### Feature Branches
- MUST: Namenskonvention einhalten:
  - **pi (Shell):** `claude/<feature-name>`
  - **claude-on-web (Browser):** `claude/<feature-name>-<session-id>`
- Context: Session-ID identifiziert Browser-Session bei claude-on-web
- SHOULD: Feature-Name beschreibend wählen

## Commit-Messages

### Sprache
- MUST: Commits nur in Englisch

### Format
- MUST: Klare, beschreibende Messages
- SHOULD: Conventional Commits verwenden (feat:, fix:, docs:, chore:)
- Beispiele:
  - `feat: add user authentication`
  - `fix: resolve null pointer in parser`
  - `docs: update API documentation`
  - `chore: update dependencies`

## Code-Sprache
- MUST: Code und Kommentare nur in Englisch
