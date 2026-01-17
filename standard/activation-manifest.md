# Activation Manifest

Expliziter, deklarativer Mechanismus zur Aktivierung von llm-rules in einem Projekt.

---

## Zweck

Das Activation Manifest legt eindeutig fest:
- Welches Stack-Template aktiv ist
- Welche Skills aktiviert sind
- Welche Execution Environment genutzt wird

Standard-Regeln aus `standard/` sind **immer aktiv** und müssen nicht deklariert werden.

---

## Format

**Datei:** `llm-rules.yml` im Projekt-Root

**Format:** YAML

---

## Struktur

### Minimal-Beispiel

```yaml
# llm-rules.yml
version: "1.0"
template: python-stack
skills:
  - workflow-standard
  - fortschritt-tracking
```

### Vollständiges Beispiel

```yaml
# llm-rules.yml
version: "1.0"

# Execution environment (default: pi)
environment: pi

# Stack template (REQUIRED)
template: python-stack

# Optional skills
skills:
  - workflow-standard
  - fortschritt-tracking
  - synergie-analyse

# Project metadata (optional)
project:
  name: "Example Project"
  description: "Python project with standard workflow"
```

---

## Felder

### `version` (REQUIRED)

- **Typ:** String
- **Format:** `"major.minor"` (z.B. `"1.0"`)
- **Zweck:** Forward-Kompatibilität bei Format-Änderungen
- **Validierung:** MUST match pattern `^\d+\.\d+$`

**Beispiel:**
```yaml
version: "1.0"
```

---

### `template` (REQUIRED)

- **Typ:** String
- **Gültige Werte:**
  - `python-stack`
  - `vim-plugin`
  - `keypirinha-plugin`
- **Zweck:** Aktiviert stack-spezifische Regeln aus `project-templates/`
- **Validierung:**
  - MUST be specified
  - MUST be exactly one
  - MUST match existing template

**Beispiel:**
```yaml
template: python-stack
```

**Effekt:**
Lädt alle Regeln aus `project-templates/python-stack.md`

---

### `skills` (OPTIONAL)

- **Typ:** List of Strings
- **Gültige Werte:**
  - `workflow-standard`
  - `fortschritt-tracking`
  - `synergie-analyse`
  - `git-tag-management`
- **Default:** Empty list (keine Skills aktiv)
- **Zweck:** Aktiviert optionale Workflow-Muster aus `skills/`
- **Validierung:**
  - MAY be empty
  - MUST reference existing skills
  - No duplicates allowed

**Beispiel:**
```yaml
skills:
  - workflow-standard
  - fortschritt-tracking
```

**Effekt:**
- Lädt `skills/workflow-standard.md`
- Lädt `skills/fortschritt-tracking.md`

---

### `environment` (OPTIONAL)

- **Typ:** String
- **Gültige Werte:**
  - `pi` (Shell-Umgebung)
  - `claude-on-web` (Browser)
- **Default:** `pi`
- **Zweck:** Bestimmt Branch-Naming-Konvention (siehe `context/execution-environment.md`)
- **Validierung:** MUST be one of allowed values

**Beispiel:**
```yaml
environment: pi
```

**Effekt:**
- `pi`: Branch-Namen `claude/<feature-name>`
- `claude-on-web`: Branch-Namen `claude/<feature-name>-<session-id>`

---

### `project` (OPTIONAL)

- **Typ:** Object
- **Felder:**
  - `name` (OPTIONAL): String - Projektname
  - `description` (OPTIONAL): String - Kurzbeschreibung
- **Zweck:** Metadaten für Dokumentation und Kontext
- **Validierung:** None (rein informativ)

**Beispiel:**
```yaml
project:
  name: "My Project"
  description: "Python web application"
```

---

## Leseregeln

### Implizite Aktivierungen

**Standard-Regeln:**
- ALWAYS active, kein Eintrag im Manifest nötig
- Automatisch aktiv aus `standard/`:
  - `communication.md`
  - `git-workflow.md`
  - `code-quality.md`
  - `error-handling.md`
  - `versioning.md`

**Context:**
- `execution-environment.md` wird durch `environment`-Feld referenziert

### Explizite Aktivierungen

**Template:**
- MUST be explicitly specified
- Genau ein Template pro Projekt
- Lädt alle Regeln aus `project-templates/{template}.md`

**Skills:**
- MUST be explicitly listed to be active
- Lädt alle Regeln aus `skills/{skill}.md`
- Leere Liste = keine Skills aktiv

---

## Fehlerbehandlung

### Fehlendes Manifest

**Verhalten:** Fallback zu Defaults

**Defaults:**
- `version`: `"1.0"`
- `template`: **Keine** (LLM MUST request clarification)
- `skills`: Empty list
- `environment`: `pi`

**Hinweis:** Da `template` REQUIRED ist, MUST das LLM nach dem Template fragen.

### Ungültiger Template-Name

**Fehler:** Template nicht gefunden

**Verhalten:** LLM MUST request clarification

**Beispiel:**
```yaml
template: unknown-stack  # ERROR: Template existiert nicht
```

### Ungültiger Skill-Name

**Warnung:** Skill wird ignoriert

**Verhalten:** LLM SHOULD notify user, fährt aber fort

**Beispiel:**
```yaml
skills:
  - workflow-standard  # OK
  - unknown-skill      # WARNING: Ignoriert
```

### Fehlendes `version`-Feld

**Warnung:** Assume version "1.0"

**Verhalten:** LLM SHOULD notify user, fährt aber fort

### Mehrere Templates

**Fehler:** Nur ein Template erlaubt

**Verhalten:** LLM MUST request clarification

**Beispiel:**
```yaml
template: 
  - python-stack     # ERROR: Nur ein Template erlaubt
  - vim-plugin
```

### Duplikate in Skills

**Fehler:** Duplikate nicht erlaubt

**Verhalten:** LLM SHOULD deduplizieren und notify user

**Beispiel:**
```yaml
skills:
  - workflow-standard
  - workflow-standard  # WARNING: Duplikat wird ignoriert
```

---

## Beispiele

### Python-Projekt mit Standard-Workflow

```yaml
version: "1.0"
template: python-stack
skills:
  - workflow-standard
  - fortschritt-tracking
```

**Aktiv:**
- Alle Standard-Regeln
- Python Stack (PEP 8, ruff, pytest, uv)
- Standard-Workflow (Doku lesen, Dialog, DoD, Review)
- Fortschritt-Tracking (Checkboxen, Zusammenfassungen)

---

### Vim-Plugin ohne Skills

```yaml
version: "1.0"
template: vim-plugin
```

**Aktiv:**
- Alle Standard-Regeln
- Vim Plugin (vint, Plugin-Struktur, Tests)
- Keine Skills

---

### Keypirinha-Plugin mit Synergie-Analyse (Browser)

```yaml
version: "1.0"
environment: claude-on-web
template: keypirinha-plugin
skills:
  - workflow-standard
  - synergie-analyse
project:
  name: "Jira Query Explorer"
  description: "Keypirinha plugin for Jira JQL"
```

**Aktiv:**
- Alle Standard-Regeln
- Keypirinha Plugin (API, UI-Responsiveness)
- Standard-Workflow
- Synergie-Analyse (Backlog konsultieren)
- Branch-Namen mit Session-ID

---

### Minimal (nur Template)

```yaml
version: "1.0"
template: python-stack
```

**Aktiv:**
- Alle Standard-Regeln
- Python Stack
- Keine Skills
- Default Environment (pi)

---

## Integration in Workflow

### 1. Projekt-Setup

Erstelle `llm-rules.yml` im Projekt-Root:

```bash
cd /path/to/project
cat > llm-rules.yml << 'EOF'
version: "1.0"
template: python-stack
skills:
  - workflow-standard
  - fortschritt-tracking
EOF
```

### 2. LLM liest Manifest

Das LLM MUST beim Start:
1. Prüfen ob `llm-rules.yml` existiert
2. Falls ja: Manifest lesen und gemäß Leseregeln interpretieren
3. Falls nein: Fallback zu Defaults, **aber** nach Template fragen

### 3. Aktivierte Regeln anwenden

Das LLM MUST:
- Alle Standard-Regeln befolgen
- Template-spezifische Regeln befolgen
- Aktivierte Skills befolgen

---

## Validierung (informativ)

Zukünftig könnte ein Validator so aussehen:

```python
def validate_manifest(manifest):
    errors = []
    warnings = []
    
    # Required fields
    if 'version' not in manifest:
        warnings.append("Field 'version' missing, assuming 1.0")
    
    if 'template' not in manifest:
        errors.append("Field 'template' is REQUIRED")
    
    # Valid template
    valid_templates = ['python-stack', 'vim-plugin', 'keypirinha-plugin']
    if manifest.get('template') not in valid_templates:
        errors.append(f"Invalid template: {manifest.get('template')}")
    
    # Valid skills
    valid_skills = ['workflow-standard', 'fortschritt-tracking', 
                    'synergie-analyse', 'git-tag-management']
    for skill in manifest.get('skills', []):
        if skill not in valid_skills:
            warnings.append(f"Unknown skill: {skill}")
    
    # Valid environment
    valid_envs = ['pi', 'claude-on-web']
    if manifest.get('environment') and manifest.get('environment') not in valid_envs:
        errors.append(f"Invalid environment: {manifest.get('environment')}")
    
    return errors, warnings
```

**Status:** Nur Konzept, keine Implementierung erforderlich

---

## Erweiterbarkeit

Das Format ist erweiterbar für zukünftige Dimensionen:

```yaml
version: "1.0"
template: python-stack
skills:
  - workflow-standard

# Zukünftige Erweiterungen möglich:
# plugins: [...]
# hooks: [...]
# custom_rules: path/to/rules.md
```

Neue Felder SHOULD ignoriert werden, wenn nicht erkannt (graceful degradation).

---

## Zusammenfassung

**Datei:** `llm-rules.yml` im Projekt-Root

**Pflichtfelder:**
- `version`: Manifest-Version (empfohlen "1.0")
- `template`: Genau ein Stack-Template

**Optionale Felder:**
- `skills`: Liste aktivierter Skills
- `environment`: Execution Environment
- `project`: Metadaten

**Verhalten bei Fehlen:** Fallback zu Defaults, aber Template MUST erfragt werden

**Standard-Regeln:** Immer aktiv, keine Deklaration nötig
