# Intent & Risiko – Phase 3

Analyse des Zwecks und der Risiken für alle extrahierten Regeln.

**Status:** In Arbeit  
**Erstellt:** 2025-01-17  
**Letzte Aktualisierung:** 2025-01-17

---

## Methodik

Für jede Regel wird analysiert:

1. **Intent (Zweck):** Welches Problem adressiert die Regel?
2. **Risiko:** Welches Risiko entsteht ohne diese Regel?
3. **Gültigkeit:** Ist die Regel grundsätzlich oder situativ?

---

## Analyse nach Kategorien

### Standard-Vorgaben

#### Kommunikation

**Regeln:** DP-R01, DP-R02, DP-R03, DP-R04, DP-R05, DP-R06, DP-R12, CL-R42, CL-R43, CL-R44, CL-R45, CL-R46

**Intent:**
- Missverständnisse vermeiden
- Effiziente Kommunikation sicherstellen
- Antworten auf tatsächliche Bedürfnisse zuschneiden
- Informationsüberflutung vermeiden

**Risiko ohne Regel:**
- LLM rät bei Unklarheiten statt nachzufragen → falsche Annahmen
- Unstrukturierte Rückfragen → ineffiziente Klärung
- Zu ausführliche Antworten → Zeitverschwendung, Ablenkung
- Unkritische Übernahme von Vorgaben → Qualitätsverlust

**Gültigkeit:** Grundsätzlich

**Besonderheit:**
- Duplikation über alle Quellen → starkes Signal für Wichtigkeit
- Zwei Varianten bei Frageformat (Q1...Qn vs. f1...fn) → Harmonisierung nötig

---

#### Git-Workflow

**Regeln:** CL-R15, CL-R17, CL-R38, FD-R16, FD-R18, FD-R19, FD-R20, KP-R45, KP-R55, KP-R57, KP-R58, KP-R59

**Intent:**
- Code-Qualität und Nachvollziehbarkeit sichern
- Kollaboration strukturieren
- Produktionscode schützen
- Änderungshistorie semantisch lesbar machen

**Risiko ohne Regel:**
- Direkter Push auf main → unkontrollierte Änderungen in Produktionscode
- Unklare Commit-Messages → schwierige Code-Historie, keine Nachvollziehbarkeit
- Nicht-englische Commits → Kollaborationsbarrieren in internationalen Teams
- Inkonsistente Commit-Struktur → schwierige Automatisierung (Changelog, Release Notes)

**Gültigkeit:** Grundsätzlich

**Besonderheit:**
- Geschützter Main-Branch: mehrfach als "NIEMALS" formuliert → hohe Priorität
- Conventional Commits: spezifisches Format, aber weit verbreitet

---

#### Code-Qualität & Dokumentation

**Regeln:** CL-R18, CL-R19, CL-R20, CL-R21, CL-R26, CL-R40, FD-R08, FD-R11, KP-R21, KP-R24, KP-R54

**Intent:**
- Wartbarkeit sicherstellen
- Internationale Zusammenarbeit ermöglichen
- Review-Prozesse effizient gestalten
- Langfristige Stabilität gewährleisten
- Dokumentation synchron halten

**Risiko ohne Regel:**
- Nicht-englischer Code → Kollaborationsbarrieren
- Große Diffs → schwierige Reviews, höhere Fehlerrate
- Tests nicht angepasst → falsche Sicherheit, unerkannte Regressionen
- Keine Rückwärtskompatibilität → Breaking Changes ohne Ankündigung
- Fehlende Docstrings → undurchsichtige APIs, höherer Einarbeitungsaufwand
- Veraltete Dokumentation → Verwirrung, Fehlannahmen

**Gültigkeit:** Grundsätzlich

**Besonderheit:**
- Englisch als Code-Sprache: stark über Quellen verteilt → Konsens
- Docstrings: Prinzip allgemein, Umsetzung sprachspezifisch

---

#### Error Handling & Security

**Regeln:** KP-R30, KP-R32, KP-R33, KP-R38, KP-R40, KP-R43, KP-R49, KP-R50, KP-R51

**Intent:**
- Robustheit sicherstellen
- Debugging ermöglichen
- Sicherheit gewährleisten
- Benutzerfreundlichkeit bei Fehlern

**Risiko ohne Regel:**
- Keine Error Handling → Crashes, undefiniertes Verhalten
- Fehlendes Logging → schwieriges Debugging in Produktion
- Network Errors nicht abgefangen → schlechte User Experience
- Keine Timeouts → hängende Anwendungen
- Hardcodierte API-Keys → Sicherheitsleck, kompromittierte Credentials
- Auth-Fehler ignoriert → unbefugte Zugriffe, falsche Annahmen
- Credentials in Git → dauerhafte Exposition sensibler Daten

**Gültigkeit:** Grundsätzlich

**Besonderheit:**
- API-Keys nicht hardcoden: Security-kritisch
- Timeouts: Performance- und UX-relevant
- Logging: essentiell für Wartung

---

#### Versioning

**Regeln:** KP-R07, KP-R60

**Intent:**
- Änderungen semantisch kommunizieren
- Kompatibilitätserwartungen setzen
- Release-Management standardisieren

**Risiko ohne Regel:**
- Willkürliche Versionsnummern → keine Kompatibilitätsgarantien
- Breaking Changes ohne erkennbare Markierung → unerwartete Probleme bei Updates

**Gültigkeit:** Grundsätzlich (bei versionierten Artefakten)

**Besonderheit:**
- Semantic Versioning: weit verbreiteter De-facto-Standard

---

### Skills (Workflow-Muster)

#### Dokumentation lesen vor Start

**Regeln:** CL-R01, CL-R05, FD-R01, FD-R02, FD-R03, FD-R04, KP-R01, KP-R02, KP-R03, KP-R04

**Intent:**
- Kontext verstehen vor Implementierung
- Projektspezifische Vorgaben berücksichtigen
- Doppelarbeit vermeiden (Archive)
- Best Practices aus früheren Iterationen nutzen (Learnings)

**Risiko ohne Regel:**
- Implementierung ohne Kontext → falsche Annahmen, inkonsistente Lösungen
- Projektregeln ignoriert → nachträgliche Korrekturen nötig
- Doppelte Arbeit → Zeitverschwendung
- Fehler wiederholen → keine Lernkurve

**Gültigkeit:** Grundsätzlich (wenn entsprechende Dokumentation existiert)

**Besonderheit:**
- Konsistentes Muster über alle Projekte
- BACKLOG.md für Synergien → proaktives Denken

---

#### Dialog-basierte Verfeinerung

**Regeln:** CL-R02, CL-R03, CL-R04, CL-R08

**Intent:**
- Anforderungen gemeinsam schärfen
- Missverständnisse früh aufdecken
- Explizite Freigabe sicherstellen (kein autonomes Handeln)
- Qualität durch Review

**Risiko ohne Regel:**
- Implementierung basierend auf unklaren Requirements → Nacharbeit
- Keine Freigabe → LLM implementiert zu früh oder falsch
- Kein Review → Fehler gehen in Produktion

**Gültigkeit:** Grundsätzlich (besonders bei komplexen Features)

**Besonderheit:**
- Explizite User-Freigabe: verhindert autonomes Handeln
- Review-Phase: Qualitätssicherung

---

#### Definition of Done (DoD)

**Regeln:** CL-R07, CL-R14, CL-R39, CL-R41, FD-R14, KP-R11, KP-R18, KP-R52

**Intent:**
- Qualitätsstandards durchsetzen
- Automatisierbare Checks sicherstellen
- Keine halbfertigen Commits
- Iterieren statt abbrechen

**Risiko ohne Regel:**
- Code ohne Checks committed → technische Schulden
- Linting-Fehler in Codebase → schleichende Qualitätsverschlechterung
- Tests schlagen fehl → unbemerkte Regressionen
- Abbruch bei Fehler → unvollständige Arbeit

**Gültigkeit:** Grundsätzlich (bei definierten Quality Gates)

**Besonderheit:**
- "Iterieren bis grün" mehrfach betont → keine Abkürzungen akzeptiert
- Starke Duplikation → hohe Wichtigkeit

---

#### Fortschrittstracking

**Regeln:** CL-R06, FD-R15, KP-R09, KP-R12, KP-R19

**Intent:**
- Transparenz über Arbeitsstand
- Nachvollziehbarkeit für User
- Abschlusskriterien dokumentieren
- Wissenstransfer (Zusammenfassung)

**Risiko ohne Regel:**
- Intransparenter Fortschritt → User weiß nicht, wo LLM steht
- Keine Zusammenfassung → schwierige Nachvollziehbarkeit später
- Dokumentation veraltet → Diskrepanz zwischen Code und Doku

**Gültigkeit:** Grundsätzlich (bei strukturierten Workflows)

**Besonderheit:**
- Checkboxen: einfaches, visuelles Tracking
- Zusammenfassung: Wissensartefakt

---

#### Git Tag Management & Versioning

**Regeln:** KP-R05, KP-R06, KP-R08, KP-R61, KP-R62, KP-R63, KP-R64, KP-R65, KP-R66

**Intent:**
- Release-Punkte markieren
- Nachvollziehbarkeit welches Feature in welcher Version
- Automatisierung von Release-Notes
- Versioning-Strategie durchsetzen

**Risiko ohne Regel:**
- Features ohne Tag → keine klare Release-Historie
- Inkonsistente Versionierung → Verwirrung über Kompatibilität
- Fehlende Tags → schwierige Rollbacks, unklar welcher Stand deployed ist

**Gültigkeit:** Situativ (projektabhängig, ob Tags gewünscht)

**Besonderheit:**
- Automatische Tag-Erstellung durch LLM → proaktiv
- Minor +1 pro Feature → klare Regel
- Sehr detailliert beschrieben → spezifisch für ein Projekt

---

#### Synergie-Analyse

**Regeln:** KP-R13, KP-R14

**Intent:**
- Zusammenhänge zwischen Features erkennen
- Architekturentscheidungen mit Blick auf Zukunft treffen
- User auf Implikationen hinweisen

**Risiko ohne Regel:**
- Isolierte Implementierung → spätere Refactorings nötig
- Synergien nicht genutzt → Doppelarbeit, inkonsistente Lösungen
- User übersieht Zusammenhänge → suboptimale Entscheidungen

**Gültigkeit:** Grundsätzlich (wenn Backlog existiert)

**Besonderheit:**
- Proaktives Denken gefordert
- Strategische Dimension

---

### Projekt-spezifisch

#### Python-Stack

**Regeln:** CL-R22, CL-R23, CL-R24, CL-R25, FD-R05, FD-R07, FD-R10, FD-R12, FD-R13, FD-R21, FD-R22, KP-R20, KP-R23

**Intent:**
- Python-Ökosystem-Standards einhalten
- Konsistenten Code-Stil sicherstellen
- Automatisierte Qualitätschecks
- Typsicherheit erhöhen (wo sinnvoll)

**Risiko ohne Regel:**
- Inkonsistenter Code-Stil → schwierige Wartung
- Keine Linting → schleichende Qualitätsprobleme
- Keine Tests → Regressionen unbemerkt
- Fehlende Type Hints → schwierigeres Verständnis, mehr Laufzeitfehler

**Gültigkeit:** Situativ (nur in Python-Projekten)

**Besonderheit:**
- PEP 8: Python-Community-Standard
- ruff: modernes, schnelles Tooling
- pytest: De-facto-Standard für Python-Tests
- uv: neueres Tool, projektspezifisch gewählt

---

#### Vim/Vimscript-Stack

**Regeln:** CL-R12, CL-R13, CL-R27, CL-R28, CL-R29, CL-R30, CL-R31, CL-R32, CL-R33, CL-R34, CL-R35, CL-R36, CL-R37

**Intent:**
- Vim-Plugin-Best-Practices einhalten
- Testbarkeit sicherstellen
- Namespace-Kollisionen vermeiden
- Keine Änderungen an generierten Dateien

**Risiko ohne Regel:**
- Monolithischer Plugin-Code → schwierige Wartung
- Namespace-Kollisionen → Konflikte mit anderen Plugins
- Fehlende Tests → Regressionen bei Vim-Updates
- Generierte Dateien editiert → Änderungen gehen bei Regenerierung verloren

**Gültigkeit:** Situativ (nur in Vim-Projekten)

**Besonderheit:**
- Sehr spezifisch für Vim-Ökosystem
- s:-Scope: Vim-spezifisches Konzept
- Generierte Dateien: projektspezifisches Risiko

---

#### Keypirinha-Stack

**Regeln:** KP-R25, KP-R26, KP-R27, KP-R28, KP-R29, KP-R31, KP-R41, KP-R44, KP-R47, KP-R48

**Intent:**
- Keypirinha-API korrekt nutzen
- UI-Responsiveness sicherstellen
- Plugin-Lifecycle korrekt implementieren

**Risiko ohne Regel:**
- Plugin funktioniert nicht → Laufzeitfehler
- UI blockiert → schlechte User Experience, Keypirinha friert ein
- Cleanup fehlt → Memory Leaks, Ressourcen nicht freigegeben

**Gültigkeit:** Situativ (nur in Keypirinha-Projekten)

**Besonderheit:**
- Sehr spezifische API-Anforderungen
- UI-Blocking mehrfach betont → kritisch für Responsiveness
- Windows-only (Keypirinha ist Windows-only)

---

#### Jira/API-spezifisch

**Regeln:** KP-R34, KP-R35, KP-R36, KP-R37, KP-R39, KP-R42, KP-R46

**Intent:**
- Jira API korrekt nutzen
- Rate Limiting respektieren
- JQL-Syntax validieren
- Aktuelle Endpunkte verwenden

**Risiko ohne Regel:**
- Ungültiges JQL → API-Fehler, schlechte UX
- Rate Limiting ignoriert → API-Bann
- Veraltete Endpunkte → 410 Gone Fehler
- Direkte API-Calls im Plugin → schwierige Wartung, Testbarkeit

**Gültigkeit:** Situativ (nur bei Jira-Integration)

**Besonderheit:**
- Sehr spezifisch für Jira Cloud API
- Custom Exceptions: saubere Fehlerbehandlung
- API-Endpunkte deprecation: reales Problem im Projekt

---

#### Projektspezifische Dateien/Pfade

**Regeln:** FD-R23, CL-R37

**Intent:**
- Datenintegrität wahren
- Klare Verantwortlichkeiten

**Risiko ohne Regel:**
- Generierte Dateien manuell editiert → Änderungen gehen verloren
- Falsche Datenbank → Datenverlust, Verwirrung

**Gültigkeit:** Situativ (nur in spezifischem Projekt)

**Besonderheit:**
- Sehr projektgebunden
- CL-R37: explizit als "Rules for Claude" markiert

---

### Unklar / Kontextabhängig

#### Branch-Namenskonventionen

**Regeln:** CL-R16, FD-R17, KP-R53, KP-R56

**Intent:**
- Konsistente Branch-Namen
- Nachvollziehbarkeit wer Branch erstellt hat
- Zuordnung zu Sessions (optional)

**Risiko ohne Regel:**
- Inkonsistente Branch-Namen → Unordnung
- Unklare Zuordnung → wer hat was gemacht?

**Gültigkeit:** Unklar

**Warum unklar:**
- Varianten existieren (claude/*, claude/*-<session-id>)
- Session-ID nicht in allen Projekten vorhanden
- Könnte Standard sein, könnte projektspezifisch sein

**Entscheidungsbedarf:**
- Soll Session-ID Teil der Konvention sein?
- Oder ist "claude/*" als Präfix ausreichend?

---

#### FD-R28: "Vorgehen im Dialog"

**Intent:** Unklar (unvollständige Regel)

**Risiko ohne Regel:** Unklar

**Gültigkeit:** Unklar

**Warum unklar:**
- Keine weitere Beschreibung in Quelle
- Möglicherweise unvollständig erfasst
- Könnte Duplikat zu Dialog-Workflow-Regeln sein

**Entscheidungsbedarf:**
- Originalquelle prüfen
- Regel vervollständigen oder verwerfen

---

## Übergreifende Erkenntnisse

### Risiko-Schwerpunkte

1. **Qualitätsverlust ohne DoD:**
   - Mehrfach betont über alle Projekte
   - Iterieren bis grün → keine Abkürzungen
   - Kritisch für Codebase-Gesundheit

2. **Sicherheit:**
   - Hardcodierte Credentials → hohes Risiko
   - Auth-Fehler ignoriert → Sicherheitslücken
   - In Git committete Secrets → dauerhaftes Problem

3. **Kommunikation:**
   - Raten statt fragen → falsche Implementierung
   - Keine strukturierte Klärung → ineffizient
   - Zu ausführlich → Ablenkung

4. **Git-Workflow:**
   - Direkter Main-Push → Produktionscode gefährdet
   - Unklare Historie → Wartbarkeit leidet

5. **UI-Responsiveness (Keypirinha):**
   - Blockierte UI → schlechte UX
   - Mehrfach als kritisch markiert

### Grundsätzlich vs. Situativ

**Grundsätzlich (immer gültig):**
- Kommunikation (Rückfragen, tl;dr, strukturiert)
- Git-Workflow (geschützter Main, englisch, Conventional Commits)
- Code-Qualität (englisch, Diffs, Tests, Doku)
- Error Handling & Security
- DoD-Ausführung
- Dialog-basierte Verfeinerung
- Fortschrittstracking

**Situativ (kontextabhängig):**
- Technologie-Stack-Regeln (Python, Vim, Keypirinha)
- API-spezifische Regeln (Jira)
- Projektspezifische Dateien/Pfade
- Git Tag Management (wenn gewünscht)

### Duplikations-Muster

Stark duplizierte Regeln → hohe Wichtigkeit:
- Kommunikation (über alle 4 Quellen)
- Git-Workflow (über 3 Quellen)
- DoD-Ausführung (über 3 Quellen)
- Python-Standards (über 3 Quellen)

Diese sollten definitiv als **Standard** etabliert werden.

---

## Offene Fragen für Entscheidungsträger

1. **Branch-Namenskonvention:**
   - Soll session-id Teil der Standard-Konvention werden?
   - Oder reicht "claude/*" als Präfix?

2. **Frageformat:**
   - Q1...Qn oder f1...fn?
   - Oder beide als äquivalent zulassen?

3. **Git Tag Management:**
   - Soll automatische Tag-Erstellung Standard sein?
   - Oder projektspezifisch belassen?

4. **FD-R28:**
   - Regel vervollständigen oder verwerfen?

---

**Status:** Intent & Risiko-Analyse abgeschlossen  
**Datum:** 2025-01-17

**Nächster Schritt:** Gate 1 – Abschluss der Rekonstruktionsphase
