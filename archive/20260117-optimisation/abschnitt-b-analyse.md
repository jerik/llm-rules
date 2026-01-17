# Abschnitt B – Analyse: Präzedenz- & Konfliktmodell

**Status:** Vorschlag  
**Datum:** 2025-01-17

---

## 1. Zielsetzung

Festlegung eines expliziten, leicht verständlichen Modells zur:
- **Regelpriorität:** Welche Regel gilt bei Überschneidungen?
- **Konfliktauflösung:** Wie mit widersprüchlichen Regeln umgehen?

---

## 2. Analyse bestehender Regelquellen

### Aktuelle Regel-Ebenen

Aus der bestehenden Struktur ergeben sich folgende Ebenen:

| Ebene | Quelle | Charakteristik | Beispiele |
|-------|--------|----------------|-----------|
| **1. Standard** | `standard/*.md` | Immer gültig, grundlegend | communication.md, git-workflow.md |
| **2. Template** | `project-templates/*.md` | Stack-spezifisch, genau eins | python-stack.md, vim-plugin.md |
| **3. Skills** | `skills/*.md` | Optional, mehrere möglich | workflow-standard.md |
| **4. Projekt** | Projekt-spezifische Dateien | Projektindividuell | CLAUDE.md, PROJECT-RULES.md |

---

## 3. Identifizierte Konfliktszenarien

### Szenario 1: Standard vs. Template

**Beispiel:**
- **Standard** (code-quality.md): "MUST: Dokumentation in markdown"
- **Template** (hypothetisch): "SHOULD: Dokumentation in reStructuredText für Python"

**Frage:** Welche Regel gilt?

**Analyse:**
- Standard-Regeln sind grundlegend und allgemein
- Templates sind stack-spezifisch
- Templates sollten Standards präzisieren, nicht widersprechen

**Vorschlag:** Template DARF Standard präzisieren, aber NICHT widersprechen

---

### Szenario 2: Template vs. Skill

**Beispiel:**
- **Template** (python-stack.md): "MUST: pytest für Tests verwenden"
- **Skill** (hypothetisch): "SHOULD: unittest verwenden"

**Frage:** Welche Regel gilt?

**Analyse:**
- Template definiert stack-spezifische Werkzeuge
- Skills definieren Workflow-Muster
- Skills sollten nicht Tool-Entscheidungen überschreiben

**Vorschlag:** Template hat Vorrang bei Tool-Entscheidungen

---

### Szenario 3: Standard vs. Projekt

**Beispiel:**
- **Standard** (git-workflow.md): "MUST: Commits nur in Englisch"
- **Projekt**: "Commits in Deutsch erlaubt"

**Frage:** Welche Regel gilt?

**Analyse:**
- Standard-Regeln sind anbieterunabhängig und grundlegend
- Projekt-Regeln könnten begründete Ausnahmen benötigen
- Aber: Standards sollten selten überschreibbar sein

**Vorschlag:** Projekt KANN Standard überschreiben, aber MUST explizit begründen

---

### Szenario 4: Skill vs. Skill

**Beispiel:**
- **Skill 1** (workflow-standard): "MUST: USER-STORY.md lesen"
- **Skill 2** (hypothetisch): "MUST: FEATURE.md lesen statt USER-STORY.md"

**Frage:** Welche Regel gilt?

**Analyse:**
- Skills sollten orthogonal sein (keine Überschneidungen)
- Wenn Konflikt: Design-Problem der Skills
- Skills sollten nicht widersprüchlich aktivierbar sein

**Vorschlag:** Skills MÜSSEN orthogonal sein, Konflikte sind Design-Fehler

---

### Szenario 5: MUST vs. SHOULD innerhalb einer Ebene

**Beispiel:**
- Regel A: "MUST: Dokumentation aktualisieren"
- Regel B: "SHOULD: Dokumentation aktualisieren"

**Frage:** Welche Priorität gilt?

**Analyse:**
- MUST ist stärker als SHOULD
- Wenn beide in gleicher Quelle: MUST überschreibt SHOULD
- Wenn in verschiedenen Quellen: Präzedenz nach Ebene

**Vorschlag:** MUST hat Vorrang vor SHOULD

---

## 4. Vorgeschlagene Präzedenz-Hierarchie

### Hierarchie (höchste zuerst)

```
1. Standard-Regeln (standard/)
   ↓
2. Template-Regeln (project-templates/)
   ↓
3. Skill-Regeln (skills/)
   ↓
4. Projekt-Regeln (projektspezifische Dateien)
```

### Interpretation

**Regel höherer Ebene schlägt Regel niedrigerer Ebene**

**Ausnahme:** Projekt-Regeln KÖNNEN explizit überschreiben, aber MÜSSEN begründen

---

## 5. Präzisierungs- vs. Überschreibungs-Regeln

### Erlaubte Präzisierungen

Niedrigere Ebenen DÜRFEN höhere Ebenen **präzisieren**:

**Beispiel: Standard → Template**
- **Standard:** "MUST: Docstrings für öffentliche APIs"
- **Template (Python):** "MUST: Docstrings im Google-Style-Format"

**Status:** ✅ Erlaubt (Präzisierung, kein Widerspruch)

**Beispiel: Standard → Projekt**
- **Standard:** "SHOULD: Kleine Diffs"
- **Projekt:** "SHOULD: Diffs max. 200 Zeilen"

**Status:** ✅ Erlaubt (Konkretisierung)

---

### Unzulässige Überschreibungen

Niedrigere Ebenen DÜRFEN NICHT widersprechen:

**Beispiel: Standard → Template**
- **Standard:** "MUST: Code nur in Englisch"
- **Template:** "MAY: Code in Deutsch"

**Status:** ❌ Unzulässig (Widerspruch)

**Beispiel: Template → Skill**
- **Template:** "MUST: pytest verwenden"
- **Skill:** "MUST: unittest verwenden"

**Status:** ❌ Unzulässig (Widerspruch)

---

### Explizite Überschreibungen (Projekt-Ebene)

Projekt-Regeln KÖNNEN überschreiben, aber MÜSSEN:
- Explizit kennzeichnen: "Überschreibt: standard/git-workflow.md"
- Begründung liefern: "Weil: Interne Team-Konvention"
- Als Ausnahme markieren: "EXCEPTION:"

**Beispiel:**
```markdown
# EXCEPTION: Überschreibt standard/git-workflow.md
# Begründung: Team arbeitet in Deutsch, englische Commits erschweren Kommunikation

## Git Workflow
- MUST: Commits in Deutsch erlaubt
```

**Status:** ⚠️ Zulässig, aber explizit und begründet

---

## 6. Konfliktauflösungs-Strategie

### Bei erkanntem Konflikt

Das LLM MUSS:

1. **Konflikt identifizieren**
   - Welche Regeln widersprechen sich?
   - Auf welcher Ebene?

2. **Präzedenz anwenden**
   - Regel höherer Ebene hat Vorrang
   - AUSSER: Explizite Überschreibung auf Projekt-Ebene

3. **Bei Unsicherheit**
   - Konflikt dem User melden
   - Frage stellen (f1, f2, ...)
   - NICHT raten oder implizit entscheiden

### Beispiel-Ablauf

**Konflikt:**
- Standard: "MUST: Commits in Englisch"
- Projekt: "Commits in Deutsch"

**LLM-Verhalten:**
```
Konflikt erkannt:
- standard/git-workflow.md: "MUST: Commits nur in Englisch"
- PROJECT-RULES.md: "Commits in Deutsch erlaubt"

f1: Soll die Projekt-Regel die Standard-Regel überschreiben?
f2: Falls ja, bitte Begründung für EXCEPTION dokumentieren.
```

---

## 7. MUST / SHOULD / MAY Semantik

### Verbindlichkeit nach RFC 2119

| Keyword | Bedeutung | Überschreibbar? |
|---------|-----------|-----------------|
| **MUST** | Zwingend erforderlich | Nur mit EXCEPTION |
| **MUST NOT** | Absolut verboten | Nur mit EXCEPTION |
| **SHOULD** | Dringend empfohlen | Ja, mit Begründung |
| **SHOULD NOT** | Nicht empfohlen | Ja, mit Begründung |
| **MAY** | Optional | Immer |

### Präzedenz bei gleicher Ebene

Wenn in gleicher Regel-Datei:
- **MUST** überschreibt **SHOULD**
- **SHOULD** überschreibt **MAY**

Wenn in verschiedenen Dateien gleicher Ebene:
- Konflikt → User fragen

---

## 8. Orthogonalitätsprinzip für Skills

### Design-Anforderung

Skills MÜSSEN orthogonal sein:
- Keine widersprüchlichen Regeln
- Keine Tool-Entscheidungen (das ist Template-Aufgabe)
- Keine Überschneidungen in Workflow-Schritten

### Beispiel guter Orthogonalität

**Skill 1: workflow-standard**
- Definiert: Workflow-Schritte (Doku lesen, Dialog, DoD)

**Skill 2: fortschritt-tracking**
- Definiert: Tracking-Mechanismen (Checkboxen, Zusammenfassungen)

**Status:** ✅ Orthogonal (keine Überschneidung)

### Beispiel schlechter Orthogonalität

**Skill 1: workflow-standard**
- "MUST: DoD ausführen mit pytest"

**Skill 2: alternative-testing**
- "MUST: DoD ausführen mit unittest"

**Status:** ❌ Nicht orthogonal (Konflikt bei gemeinsamer Aktivierung)

**Lösung:** Tool-Entscheidungen gehören ins Template, nicht in Skills

---

## 9. Verhalten bei unauflösbaren Konflikten

### Kriterium: Unauflösbar

Ein Konflikt ist unauflösbar, wenn:
- Zwei MUST-Regeln widersprechen sich
- Keine klare Präzedenz existiert
- Keine explizite EXCEPTION vorhanden

### LLM-Verhalten

1. **Konflikt dem User melden**
   ```
   KONFLIKT: Unauflösbare Regel-Widersprüche erkannt
   
   Konflikt 1:
   - standard/communication.md: "MUST: Fragen mit f1...fn"
   - projekt/rules.md: "MUST: Fragen mit Q1...Qn"
   
   f1: Welche Konvention soll gelten?
   f2: Soll eine der Regeln als EXCEPTION markiert werden?
   ```

2. **NICHT fortfahren** ohne Klärung
3. **NICHT implizit entscheiden**

---

## 10. Vorschlag: Präzedenzmodell-Dokument

### Struktur

**Datei:** `standard/precedence-model.md`

**Inhalt:**
1. Präzedenz-Hierarchie (Standard > Template > Skills > Projekt)
2. Präzisierung erlaubt, Widerspruch nicht
3. EXCEPTION-Mechanismus für Projekt-Ebene
4. Konfliktauflösungs-Strategie
5. MUST/SHOULD/MAY-Semantik
6. Orthogonalitätsprinzip für Skills
7. Verhalten bei unauflösbaren Konflikten

---

## 11. Beispiele

### Beispiel 1: Zulässige Präzisierung

**Standard (code-quality.md):**
```markdown
### Tests
- MUST: Tests bei Verhaltensänderung anpassen
```

**Template (python-stack.md):**
```markdown
### Tests
- MUST: pytest für Tests verwenden
- MUST: Tests bei Verhaltensänderung anpassen
- SHOULD: Coverage > 80%
```

**Analyse:** ✅ Zulässig (Präzisierung + Ergänzung, kein Widerspruch)

---

### Beispiel 2: Unzulässiger Widerspruch

**Standard (git-workflow.md):**
```markdown
### Sprache
- MUST: Commits nur in Englisch
```

**Template (python-stack.md):**
```markdown
### Sprache
- MAY: Commits in beliebiger Sprache
```

**Analyse:** ❌ Unzulässig (Widerspruch zu MUST)

**Lösung:** Template muss Widerspruch entfernen oder als EXCEPTION kennzeichnen

---

### Beispiel 3: Explizite Projekt-Exception

**Standard (git-workflow.md):**
```markdown
- MUST: Commits nur in Englisch
```

**Projekt (PROJECT-RULES.md):**
```markdown
# EXCEPTION: Überschreibt standard/git-workflow.md
# Begründung: Internes Team, alle deutschsprachig

### Git Workflow
- MUST: Commits in Deutsch
```

**Analyse:** ⚠️ Zulässig (explizite, begründete Exception)

---

### Beispiel 4: Skills orthogonal

**llm-rules.yml:**
```yaml
skills:
  - workflow-standard
  - fortschritt-tracking
  - synergie-analyse
```

**Analyse:** ✅ Kein Konflikt (Skills sind orthogonal)

---

### Beispiel 5: MUST vs. SHOULD (gleiche Ebene)

**Standard (code-quality.md):**
```markdown
### Dokumentation
- MUST: Dokumentation bei neuem Behavior aktualisieren
- SHOULD: CHANGELOG synchron halten
```

**Analyse:** ✅ Kein Konflikt (MUST ist stärker, aber unterschiedliche Aspekte)

---

## 12. Offene Fragen

### Entscheidungsbedarf

1. **EXCEPTION-Syntax:**
   - Soll es ein standardisiertes Format für EXCEPTION geben?
   - Beispiel: `# EXCEPTION: <Quelle> | Begründung: <Text>`

2. **Konflikt-Reporting:**
   - Soll das LLM automatisch Konflikte in eine Datei loggen?
   - Oder nur beim User melden?

3. **Skill-Orthogonalität:**
   - Soll es eine Validierung geben (z.B. Checker-Tool)?
   - Oder nur als Design-Prinzip dokumentieren?

4. **Template-Hierarchie:**
   - Können Templates sich gegenseitig überschreiben?
   - Oder ist nur ein Template pro Projekt erlaubt? (bereits festgelegt: nur eins)

5. **Context-Präzedenz:**
   - Auf welcher Ebene steht `context/execution-environment.md`?
   - Antwort: Kontextinformation, keine aktive Regel (bereits geklärt)

---

## 13. Vorschlag für plan.md Update

### Status Abschnitt B

```markdown
## Abschnitt B – Präzedenz- & Konfliktmodell (Punkt 4)

Status: Analyse abgeschlossen, Vorschlag erstellt

### Durchgeführt
- Analyse bestehender Regel-Ebenen (Standard, Template, Skills, Projekt)
- Identifikation typischer Konfliktszenarien (5 Szenarien)
- Entwurf Präzedenz-Hierarchie (Standard > Template > Skills > Projekt)
- Definition Präzisierungs- vs. Überschreibungs-Regeln
- Konfliktauflösungs-Strategie
- Orthogonalitätsprinzip für Skills
- EXCEPTION-Mechanismus für Projekt-Ebene

### Output
- abschnitt-b-analyse.md (diese Datei)

### Offene Entscheidungen
1. EXCEPTION-Syntax standardisieren?
2. Konflikt-Reporting automatisieren?
3. Skill-Orthogonalität validieren?

### Nächste Schritte
- Entscheidungen durch Stakeholder einholen
- Nach Freigabe: Dokumentation erstellen (standard/precedence-model.md)
```

---

## 14. Zusammenfassung

**Präzedenz-Hierarchie:**
```
Standard > Template > Skills > Projekt
```

**Grundprinzipien:**
- Höhere Ebene schlägt niedrigere Ebene
- Präzisierung erlaubt, Widerspruch nicht
- Projekt kann mit EXCEPTION überschreiben (explizit + begründet)
- Skills müssen orthogonal sein
- Bei Konflikt: User fragen, nicht raten

**MUST/SHOULD/MAY:**
- MUST: Zwingend (nur mit EXCEPTION überschreibbar)
- SHOULD: Dringend empfohlen (mit Begründung überschreibbar)
- MAY: Optional (immer überschreibbar)

**Konfliktauflösung:**
1. Konflikt identifizieren
2. Präzedenz anwenden
3. Bei Unsicherheit: User fragen

**Nächster Schritt:**
- Entscheidung über offene Fragen
- Danach: Implementierung `standard/precedence-model.md`

---

**Status:** Vorschlag bereit für Review
