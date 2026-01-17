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
- Phase: Phase 1 abgeschlossen
- Letzte Aktualisierung: 2025-01-17

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

Status: offen

---

### Phase 3 – Intent & Risiko
Ziel:
- Verständnis des Zwecks jeder Regel

Outputs:
- Beschreibung des adressierten Problems
- Risikoanalyse bei Nichtbeachtung
- Einschätzung: grundsätzlich vs. situativ

Status: offen

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

Status: gesperrt (Gate 1 nicht abgeschlossen)

---

## Gates & Entscheidungen

### Gate 1 – Rekonstruktionsphase abgeschlossen
Status: offen  
Kommentar:

---

### Gate 2 – Optimierungsfreigabe
Status: nicht erteilt  
Kommentar:

---

## Offene Entscheidungen
- <noch keine>

---

## Abgeschlossene Schritte
- **2025-01-17:** Phase 1 (Inventarisierung) abgeschlossen
  - 132 Regeln aus 4 Quellen extrahiert
  - Alle Regeln strukturiert und mit IDs versehen
  - Duplikate und Widersprüche markiert
  - Dokumentation: inventar.md

