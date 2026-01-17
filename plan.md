# Plan – Aufbau eines anbieterunabhängigen LLM-Regelwerks

## Zieldefinition (verbindlich)
Ziel dieses Vorhabens ist der Aufbau eines anbieterunabhängigen,
versionierbaren Regelwerks zur Nutzung von LLMs.

Das Regelwerk soll:
- Standard-Vorgaben, Skills und Projekt-Vorgaben klar trennen
- reproduzierbar und überprüfbar sein
- ausschließlich über pi-mono als Ausführungsumgebung genutzt werden

Nicht-Ziele:
- keine unmittelbare technische Implementierung
- keine Modell-spezifischen Regeln (Claude, Codex, etc.)
- keine implizite Optimierung ohne explizite Entscheidung

---

## Aktueller Status
- Phase: Implementierung abgeschlossen
- Gate 1: Freigegeben
- Phase 4: Abgeschlossen und implementiert
- Letzte Aktualisierung: 2025-01-17
- Status: Version 1.0.0 bereit

---

## Phasen & Planung

### Phase 1 – Inventarisierung
Ziel:
- Vollständige Erfassung aller vorhandenen Regeln und Vorgaben

Outputs:
- Strukturierte Liste aller Regeln
- Quellenzuordnung
- Markierung von Duplikaten und Widersprüchen

Status: ✅ abgeschlossen (2025-01-17)

Ergebnis:
- 132 Regeln extrahiert aus 4 Quellen
- Alle Regeln mit eindeutiger ID versehen
- Duplikate und Widersprüche markiert (40+)
- Dokumentiert in: inventar.md

---

### Phase 2 – Klassifikation
Ziel:
- Einordnung aller Regeln nach Gültigkeitsbereich

Outputs:
- Zuordnung zu:
  - Standard
  - Skill
  - Projekt-spezifisch
  - Unklar / kontextabhängig

Status: ✅ abgeschlossen (2025-01-17)

Ergebnis:
- Alle 132 Regeln klassifiziert
- ~35 Standard-Vorgaben identifiziert
- ~25 Skills identifiziert
- ~60 Projekt-spezifische Regeln
- ~5 Unklare Regeln
- Dokumentiert in: klassifikation.md

---

### Phase 3 – Intent & Risiko
Ziel:
- Verständnis des Zwecks jeder Regel

Outputs:
- Beschreibung des adressierten Problems
- Risikoanalyse bei Nichtbeachtung
- Einschätzung: grundsätzlich vs. situativ

Status: ✅ abgeschlossen (2025-01-17)

Ergebnis:
- Intent und Risiko für alle Regelkategorien analysiert
- Grundsätzlich vs. situativ eingeordnet
- Risiko-Schwerpunkte identifiziert (DoD, Security, Kommunikation, Git, UI)
- Duplikations-Muster als Wichtigkeits-Indikator erkannt
- 4 offene Fragen für Entscheidungsträger dokumentiert
- Dokumentiert in: intent-risiko.md

---

### Phase 4 – Kandidaten fürs Zielbild
Ziel:
- Ableitung potenzieller Zielbild-Bausteine

Outputs:
- Vorschläge für:
  - Standard-Regeln
  - Skills
  - Projekt-Grenzen
- Alle Punkte als Vorschläge gekennzeichnet

Status: ✅ abgeschlossen (2025-01-17)

Ergebnis:
- 5 Standard-Regelwerke vorgeschlagen (Communication, Git, Code Quality, Error Handling, Versioning)
- 4 Skills vorgeschlagen (Standard-Workflow, Tracking, Synergie-Analyse, Git-Tagging)
- 3 Project Templates vorgeschlagen (Python, Vim, Keypirinha)
- 1 Context-Information (Execution Environment)
- 132 Regeln konsolidiert in strukturierte Bausteine
- Alle als Vorschläge gekennzeichnet
- Dokumentiert in: zielbild-kandidaten.md

---

## Gates & Entscheidungen

### Gate 1 – Rekonstruktionsphase abgeschlossen
Status: ✅ freigegeben (2025-01-17)  
Kommentar:
- Phase 1, 2 und 3 vollständig abgeschlossen
- Alle Regeln inventarisiert, klassifiziert und analysiert
- Keine Umformulierung oder Optimierung erfolgt (wie gefordert)
- Widersprüche markiert, nicht aufgelöst
- Dokumentation: inventar.md, klassifikation.md, intent-risiko.md
- Offene Entscheidungen geklärt und dokumentiert
- Freigabe durch Entscheidungsträger erteilt

---

### Gate 2 – Optimierungsfreigabe
Status: nicht erteilt  
Kommentar:

---

## Offene Entscheidungen

### Entschieden (2025-01-17):

1. **Branch-Namenskonvention:** ✅
   - `claude/*-<session-id>` wenn claude-on-web (Browser)
   - `claude/*` wenn claude-in-shell (pi)
   - Status: Als Kontext-Regel in Zielbild aufnehmen

2. **Frageformat:** ✅
   - Standard: f1...fn
   - Status: Als Standard etablieren

3. **Git Tag Management:** ✅
   - Projektspezifisch
   - Status: Nicht als Standard, sondern als optionaler Skill

4. **FD-R28 ("Vorgehen im Dialog"):** ✅
   - Verwerfen
   - Status: Aus Regelwerk entfernen

---

## Abgeschlossene Schritte
- **2025-01-17:** Phase 1 (Inventarisierung) abgeschlossen
  - 132 Regeln aus 4 Quellen extrahiert
  - Alle Regeln strukturiert und mit IDs versehen
  - Duplikate und Widersprüche markiert
  - Dokumentation: inventar.md

- **2025-01-17:** Phase 2 (Klassifikation) abgeschlossen
  - Alle 132 Regeln nach Gültigkeitsbereich klassifiziert
  - ~35 Standard-Vorgaben, ~25 Skills, ~60 Projekt-spezifisch, ~5 Unklar
  - Technologie-Cluster identifiziert (Python, Vim, Keypirinha)
  - Workflow-Muster erkannt
  - Dokumentation: klassifikation.md

- **2025-01-17:** Phase 3 (Intent & Risiko) abgeschlossen
  - Intent und Risiko für alle Regelkategorien analysiert
  - Grundsätzlich vs. situativ eingeordnet
  - Risiko-Schwerpunkte: DoD, Security, Kommunikation, Git, UI
  - Duplikations-Muster als Wichtigkeits-Indikator
  - 4 offene Entscheidungen dokumentiert
  - Dokumentation: intent-risiko.md

- **2025-01-17:** Gate 1 freigegeben
  - Rekonstruktionsphase (Phase 1-3) vollständig abgeschlossen
  - Keine Optimierung, keine Umformulierung durchgeführt
  - Offene Entscheidungen geklärt
  - Explizite Freigabe erteilt

- **2025-01-17:** Phase 4 (Kandidaten fürs Zielbild) abgeschlossen
  - 5 Standard-Regelwerke vorgeschlagen
  - 4 Skills vorgeschlagen
  - 3 Project Templates vorgeschlagen
  - 1 Context-Information
  - Alle 132 Regeln konsolidiert
  - Alle Vorschläge als solche gekennzeichnet
  - Dokumentiert in: zielbild-kandidaten.md

- **2025-01-17:** Implementierung abgeschlossen
  - Review durch Entscheidungsträger durchgeführt
  - Anpassungen übernommen (uv als MUST, Dokumentation in markdown)
  - Vollständige Struktur implementiert:
    - 5 Standard-Regelwerke in standard/
    - 4 Skills in skills/
    - 3 Project Templates in project-templates/
    - 1 Context-Information in context/
  - README.md erstellt mit Übersicht und Nutzungshinweisen
  - Version 1.0.0 bereit

