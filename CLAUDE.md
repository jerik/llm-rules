# Kommunikation
- MUST: Rückfragen stellen, bevor du rätst oder annimmst
- MUST: Fragen sofort stellen, nicht nach Implementierung
- MUST: Sachlich, kritisch und präzise antworten
- MUST: Keine ausführlichen Antworten ohne explizite Anfrage
- MUST: Fragen mit f1, f2, ... fn nummerieren, damit ich effizienter antworten kann
- SHOULD: Fragen im TL;DR-Stil formulieren (max. 3-4 Zeilen)
- MUST: Stelle maximal 2 Fragen in derselben runde
- MUST: Jede Frage muss afu meiner vorherigen Antwort aufbauen und den Sachverhalt verfeinern

## Kritische Analyse
- MUST: Inhalte und Prompts kritisch analysieren
- MUST: Schwächen oder Widersprüche benennen
- SHOULD: Verbesserungsvorschläge machen (als Vorschläge kennzeichnen)

# Workflow
- MUST: Halte dich IMMER an die einzelnen Phase des Workflows, ausser der Nutzer gibt dir explizit anderen Anweisungen
- Phase 1: Erkunden und Auftragsklärung 
    - MUST: Lies die userstory.md sofern vorhanden oder frage nach den Nutzer nach dem Auftrag
    - MUST: Lies die readme.md, documentation.md, lessonslearned.md sofern vorhanden um das Applikation zu verstehen
    - MUST: Kläre im Dialog mit dem Nutzer den Auftrag und erweitere die userstory.md mit dem Auftrag
    - GATE: Diese Phase ist abgeschlossen wenn der Auftrag fertiggestellt ist und der Nutzer diesen freigegeben hat. 
- Phase 2: Solutiondesign und Plan
    - MUST: Lies alle notwendigen Dateien um dich mit der Applikation vertraut zu machen
    - MUST: Erstelle anhand der Inforamtionen die du zur Applikatin hast und der userstory.md ein solutiondesign.md auf hohem nivea
    - SHOULD: Das solutiondesign.md beschreibt: 
        - strukturierten Anforderung
        - Ist-Analysse und Kontext
        - Lösungsoptinen (2-3), grob skizziert mit Entscheidungsmatrix (Kriterien, gewichtung, Klare empfehlung)
        - Architektur und Lösungsansatz
        - Securityaspekte
        - Umsetzungs- und Testaspekte 
    - MUST: Kläre im Dialog das solutiondesign mit dem Nutzer bis es final ist
    - MUST: Auf Basis des solutiondesign.md wird ein plan.md mit dem umsetzungsphasen- und milestones erstellt. 
    - GATE: Diese Phase ist abgeschlossen wenn der plan.md fertig ist und der Nutzer diesen freigegeben hat
- Phase 3: Implementierung und Test
    - MUST: Befolge den plan.md bei der Implmentierung
    - MUST: Aktualsiere den plan.md während der Implementierung, damit der Nutzer den Status nachverfolgen kann
    - MUST: Befolge folgenden Prinzipien bei der Implementierung: KISS, DRY, YAGNI
    - MUST: Entwickle in kleinen Schritten und checken den code oft ein, mit aussagekräftiger commit message
    - MUST: Erstelle Testfälle die den implementierten Code testen
    - MAY: TDD, wenn sinnvoll erscheint
    - MUST: Iteriere bei der Implementierung bis alle Tests ohne Fehler durchlaufen (grün sind)
    - MUST: Ist der plan.md abgeschlossen, verbessere den Code durch Linting und Formatierung. Führe danach alle Test aus uns interiere bis alle Tests grün sind 
    - GATE: Informiere den Nutzer das die Implmentierung beendet ist. Diese Phase ist abgeschlossen wenn der Nutzer diesen freigegeben hat
- Phase 4: Reporting, Review und Freigabe 
    - MUST: Aktualisiere die Projektdokumentation: readme.md, documentation.md, lessonslearned.md
    - MUST: Aktualisiere die solutiondesign.md wenn sich Änderungen während der Implmementierung ergeben haben


## Progressive Disclosure Anweisungen
Die folgenden Dokumente enthalten aufgaben- oder domänenspezifische Anleitungen.
Lies keien Dateien ungeprüft oder pauschal
Du MUST entscheiden, welche davon für die aktuelle Aufgabe relevant sind.
 
### Anweisungsindex
#### Test-Richtlinien
Datei: agent-docs/testing-policy.md

Lese diese Datei wenn: 
    - Tests geschrieben werden 
    - Tests verbessert werden sollen

#### Python-Richtlinien
Datei: agent-docs/python-policy.md

Lese diese Datei: 
    - wenn python code erstellt oder aktualisiert wird

#### Git-Richtlinien
Datei: agent-docs/git-policy.md

Lese diese Datei: 
    - wenn python code erstellt oder aktualisiert wird
