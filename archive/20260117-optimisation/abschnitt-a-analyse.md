# Abschnitt A – Analyse: Expliziter Aktivierungsmechanismus

**Status:** Vorschlag  
**Datum:** 2025-01-17

---

## 1. Analyse der bestehenden Struktur

### Aktuelle Komponenten der llm-rules

Aus der Analyse der bestehenden Repository-Struktur ergeben sich folgende Aktivierungsdimensionen:

| Dimension | Verzeichnis | Anzahl | Charakteristik |
|-----------|-------------|--------|----------------|
| **Standard-Regeln** | `standard/` | 5 | Immer gültig, nicht abwählbar |
| **Skills** | `skills/` | 4 | Optional aktivierbar |
| **Templates** | `project-templates/` | 3 | Genau eins pro Projekt |
| **Context** | `context/` | 1 | Information, keine Aktivierung |

### Aktivierungs-Aussagen in bestehenden Regeln

Aus der Analyse der Skill-Dateien:

**workflow-standard.md:**
> "Wird aktiviert durch Projektvorgabe oder explizite Nennung"

**git-tag-management.md:**
> "Projektspezifisch (nicht Standard)"
> "Muss explizit in Projektvorgaben aktiviert werden"

**Erkenntnis:** Es existiert bereits die Erwartung einer "Projektvorgabe", aber kein definiertes Format.

---

## 2. Identifizierte Anforderungen

### Notwendige Informationen im Aktivierungsmechanismus

1. **Standard-Regeln**
   - Status: Immer aktiv (implizit)
   - Aktivierung: Nicht nötig zu deklarieren
   - Opt-out: Nicht vorgesehen

2. **Skills**
   - Status: Optional
   - Aktivierung: Explizit pro Skill
   - Mehrfachauswahl: Ja

3. **Templates**
   - Status: Genau ein Template pro Projekt
   - Aktivierung: Explizit
   - Mehrfachauswahl: Nein (Konflikt, wenn mehrere)

4. **Execution Environment**
   - Status: Kontextinformation
   - Deklaration: Optional (falls abweichend von Default)

### Abgeleitete Mindestanforderungen

Ein Aktivierungsmechanismus MUSS:
- Klarheit schaffen, welche Skills aktiv sind
- Eindeutig ein Template benennen
- Einfach menschenlesbar sein
- Maschinenlesbar sein (strukturiert)
- Erweiterbar sein (für zukünftige Dimensionen)

Ein Aktivierungsmechanismus SOLLTE:
- Execution Environment deklarieren können
- Projektspezifische Ergänzungen ermöglichen

---

## 3. Vorschlag: Activation Manifest Format

### Format-Entscheidung

**Vorschlag:** YAML-Format

**Begründung:**
- Menschenlesbar
- Maschinenlesbar
- Weit verbreitet in DevOps/CI
- Unterstützt Kommentare
- Einfache Struktur ohne Overhead

**Alternative:** Strukturierter Markdown-Abschnitt (z.B. in CLAUDE.md)
- Vorteil: Keine zusätzliche Datei
- Nachteil: Schwieriger maschinell zu parsen, weniger strukturiert

**Empfehlung:** YAML als eigenständige Datei `llm-rules.yml` im Projekt-Root

---

## 4. Vorgeschlagene Struktur

### Minimal-Variante

```yaml
# llm-rules.yml
# Activation Manifest for llm-rules

version: "1.0"

template: python-stack

skills:
  - workflow-standard
  - fortschritt-tracking
```

### Erweiterte Variante

```yaml
# llm-rules.yml
# Activation Manifest for llm-rules

version: "1.0"

# Execution environment: pi (shell) or claude-on-web (browser)
environment: pi

# Exactly one template (stack-specific rules)
template: python-stack

# Optional skills (workflow patterns)
skills:
  - workflow-standard
  - fortschritt-tracking
  - synergie-analyse
  # - git-tag-management  # optional, only if needed

# Project-specific extensions (optional)
project:
  name: "My Project"
  description: "Short project description"
```

### Vollständige Variante mit allen Optionen

```yaml
# llm-rules.yml
# Activation Manifest for llm-rules

version: "1.0"

# Execution environment (default: pi)
# Options: pi | claude-on-web
environment: pi

# Exactly one template (REQUIRED)
# Options: python-stack | vim-plugin | keypirinha-plugin
template: python-stack

# Optional skills (multiple allowed)
# Available: workflow-standard, fortschritt-tracking, synergie-analyse, git-tag-management
skills:
  - workflow-standard
  - fortschritt-tracking

# Project metadata (optional)
project:
  name: "Example Project"
  description: "Python project with standard workflow"
  
# Project-specific rules (optional)
# Reference to additional project rules file
project_rules: "CLAUDE.md"
```

---

## 5. Semantik der Felder

### `version` (REQUIRED)
- **Typ:** String
- **Format:** Semantic Versioning (z.B. "1.0")
- **Zweck:** Forward-Kompatibilität, falls Manifest-Format sich ändert
- **Validierung:** MUST match pattern `^\d+\.\d+$`

### `environment` (OPTIONAL)
- **Typ:** String
- **Options:** `pi` | `claude-on-web`
- **Default:** `pi`
- **Zweck:** Branch-Naming-Konvention (siehe context/execution-environment.md)
- **Validierung:** MUST be one of allowed values

### `template` (REQUIRED)
- **Typ:** String
- **Options:** `python-stack` | `vim-plugin` | `keypirinha-plugin`
- **Zweck:** Aktiviert stack-spezifische Regeln
- **Validierung:** 
  - MUST be specified
  - MUST be exactly one
  - MUST match existing template in project-templates/

### `skills` (OPTIONAL)
- **Typ:** List of Strings
- **Options:** `workflow-standard` | `fortschritt-tracking` | `synergie-analyse` | `git-tag-management`
- **Default:** Empty list
- **Zweck:** Aktiviert optionale Workflow-Muster
- **Validierung:**
  - MAY be empty
  - MUST reference existing skills in skills/
  - No duplicates allowed

### `project` (OPTIONAL)
- **Typ:** Object
- **Felder:**
  - `name` (OPTIONAL): String
  - `description` (OPTIONAL): String
- **Zweck:** Projekt-Metadaten für Dokumentation

### `project_rules` (OPTIONAL)
- **Typ:** String (Dateipfad)
- **Zweck:** Referenz zu projektspezifischen Ergänzungen
- **Validierung:** SHOULD be a valid file path

---

## 6. Interpretation und Leseregeln

### Implizite Aktivierungen

**Standard-Regeln:**
- ALWAYS active, kein Eintrag im Manifest nötig
- Gelten automatisch aus `standard/`:
  - communication.md
  - git-workflow.md
  - code-quality.md
  - error-handling.md
  - versioning.md

**Context:**
- Information, keine Aktivierung
- execution-environment.md wird durch `environment`-Feld referenziert

### Explizite Aktivierungen

**Template:**
- MUST be explicitly specified
- Genau ein Template pro Projekt
- Lädt alle Regeln aus `project-templates/{template}.md`

**Skills:**
- MUST be explicitly listed to be active
- Lädt alle Regeln aus `skills/{skill}.md`

### Fehlende oder ungültige Einträge

**Fehlendes `template`:**
- ERROR: Manifest ist ungültig
- LLM MUST request clarification

**Ungültiger Template-Name:**
- ERROR: Template nicht gefunden
- LLM MUST request clarification

**Ungültiger Skill-Name:**
- WARNING: Skill wird ignoriert
- LLM SHOULD notify user

**Fehlendes `version`:**
- WARNING: Assume version "1.0"

---

## 7. Platzierung im Repository

### Vorschlag: Projekt-Root

**Dateiname:** `llm-rules.yml`

**Begründung:**
- Zentral und leicht auffindbar
- Analog zu anderen Config-Dateien (`.gitignore`, `pyproject.toml`)
- Kein Konflikt mit existierenden Dateien

**Alternative:** `.llm-rules.yml` (versteckte Datei)
- Vorteil: Weniger "clutter" im Root
- Nachteil: Weniger sichtbar, könnte übersehen werden

**Empfehlung:** `llm-rules.yml` (sichtbar)

---

## 8. Integration in bestehende Struktur

### Änderungen an bestehenden Dateien

**README.md:**
- Abschnitt "Nutzung" aktualisieren
- Beispiel mit `llm-rules.yml` ergänzen
- Altes Beispiel (Markdown in CLAUDE.md) als deprecated markieren oder entfernen

**skills/*.md:**
- Aktivierungs-Hinweise einheitlich gestalten
- Verweis auf `llm-rules.yml` ergänzen

### Neue Dateien

**activation-manifest.md** (neue Dokumentation):
- Vollständige Spezifikation des Manifest-Formats
- Beispiele
- Validierungsregeln
- Fehlerbehandlung

**Platzierung:** `standard/activation-manifest.md` oder separat im Root

---

## 9. Validierung (optional, zukünftig)

### Mögliche Validierungslogik

Ein zukünftiges Tool könnte `llm-rules.yml` validieren:

```python
# Beispiel-Validierung (nicht implementiert)
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
    
    return errors, warnings
```

**Status:** Nur als Konzept, keine Implementierung gefordert

---

## 10. Offene Fragen

### Entscheidungsbedarf

1. **Format-Wahl:**
   - YAML (empfohlen) oder strukturierter Markdown?

2. **Dateiname:**
   - `llm-rules.yml` (empfohlen) oder `.llm-rules.yml`?

3. **Platzierung der Dokumentation:**
   - `standard/activation-manifest.md` oder `activation-manifest.md` im Root?

4. **Rückwärtskompatibilität:**
   - Soll das alte Beispiel (Markdown in CLAUDE.md) weiterhin dokumentiert werden?
   - Oder klarer Schnitt zu neuem Format?

5. **Optionalität des Manifests:**
   - Was passiert, wenn `llm-rules.yml` fehlt?
   - ERROR oder Fallback zu Defaults?

---

## 11. Vorschlag für plan.md Update

### Status Abschnitt A

```markdown
## Abschnitt A – Expliziter Aktivierungsmechanismus (Punkt 3)

Status: Analyse abgeschlossen, Vorschlag erstellt

### Durchgeführt
- Analyse der bestehenden llm-rules Struktur
- Ableitung der Aktivierungsdimensionen (Standard, Skills, Templates, Context)
- Entwurf eines YAML-basierten Activation Manifest
- Dokumentation der Semantik und Leseregeln
- Identifikation offener Entscheidungspunkte

### Output
- abschnitt-a-analyse.md (diese Datei)

### Offene Entscheidungen
1. Format-Wahl: YAML vs. Markdown
2. Dateiname: llm-rules.yml vs. .llm-rules.yml
3. Platzierung der Dokumentation
4. Rückwärtskompatibilität mit altem Beispiel
5. Verhalten bei fehlendem Manifest

### Nächste Schritte
- Entscheidungen durch Stakeholder einholen
- Nach Freigabe: Dokumentation erstellen (activation-manifest.md)
- README.md aktualisieren
```

---

## 12. Zusammenfassung

**Vorgeschlagenes Format:** YAML-Datei `llm-rules.yml` im Projekt-Root

**Kernfelder:**
- `version` (REQUIRED): Manifest-Version
- `template` (REQUIRED): Genau ein Stack-Template
- `skills` (OPTIONAL): Liste aktivierter Skills
- `environment` (OPTIONAL): Execution Environment
- `project` (OPTIONAL): Metadaten

**Vorteile:**
- Klar strukturiert und maschinenlesbar
- Einfach manuell zu erstellen und zu warten
- Erweiterbar für zukünftige Dimensionen
- Keine implizite Logik, explizite Deklaration

**Nächster Schritt:**
- Entscheidung über offene Fragen
- Danach: Implementierung der Dokumentation

---

**Status:** Vorschlag bereit für Review
