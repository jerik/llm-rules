# Backlog

## validierungstool entwickeln
Was bedeutet "Skill-Orthogonalität"?                                                                                                                                      
 Orthogonal = Skills überschneiden sich nicht, haben keine Konflikte                                                                                                       
 ### Das Problem:                                                                                                                                                          
 Skills können so designed werden, dass sie widersprüchliche Regeln enthalten, wenn sie zusammen aktiviert werden.                                                         
 Beispiel eines NICHT-orthogonalen Designs:                                                                                                                                
 ```yaml                                                                                                                                                                   
   # llm-rules.yml                                                                                                                                                         
   skills:                                                                                                                                                                 
     - workflow-standard                                                                                                                                                   
     - alternative-workflow  # Hypothetisch                                                                                                                                
 ```                                                                                                                                                                       
 workflow-standard.md:                                                                                                                                                     
 ```markdown                                                                                                                                                               
   ### DoD                                                                                                                                                                 
   - MUST: DoD vollständig ausführen                                                                                                                                       
   - MUST: pytest ausführen                                                                                                                                                
 ```                                                                                                                                                                       
 alternative-workflow.md (hypothetisch):                                                                                                                                   
 ```markdown                                                                                                                                                               
   ### DoD                                                                                                                                                                 
   - MUST: DoD vollständig ausführen                                                                                                                                       
   - MUST: unittest ausführen statt pytest                                                                                                                                 
 ```                                                                                                                                                                       
                                                                                                                                                                           
 Konflikt: Beide Skills sagen "MUST" für unterschiedliche Test-Tools!                                                                                                      
 ────────────────────────────────────────────────────────────────────────────────                                                                                          
 Die Frage an dich (Punkt 3):                                                                                                                                              
 Soll es eine automatische Validierung geben, die prüft, ob aktivierte Skills orthogonal sind?                                                                             
 ### Option A: Nur dokumentieren (Design-Prinzip)                                                                                                                          
 - Skill-Entwickler müssen darauf achten, orthogonal zu sein                                                                                                               
 - Keine technische Prüfung                                                                                                                                                
 - Bei Konflikt: LLM meldet es zur Laufzeit                                                                                                                                
                                                                                                                                                                           
 Vorteil: Einfach, kein Overhead                                                                                                                                           
 Nachteil: Fehler werden erst bei Nutzung erkannt                                                                                                                          
 ────────────────────────────────────────────────────────────────────────────────                                                                                          
 ### Option B: Validierung implementieren                                                                                                                                  
 Ein Tool oder Checker prüft:                                                                                                                                              
 - Ob aktivierte Skills widersprüchliche MUST-Regeln haben                                                                                                                 
 - Ob Skills sich in Tool-Entscheidungen einmischen (gehört ins Template)                                                                                                  
 - Warnung vor Aktivierung                                                                                                                                                 
                                                                                                                                                                           
 Beispiel:                                                                                                                                                                 
 ```bash                                                                                                                                                                   
   $ validate-llm-rules llm-rules.yml                                                                                                                                      
                                                                                                                                                                           
   WARNING: Potential conflict between skills:                                                                                                                             
   - workflow-standard.md: "MUST: pytest"                                                                                                                                  
   - alternative-workflow.md: "MUST: unittest"                                                                                                                             
                                                                                                                                                                           
   These skills should not be activated together.                                                                                                                          
 ```                                                                                                                                                                       
 Vorteil: Fehler werden früh erkannt                                                                                                                                       
 Nachteil: Zusätzliche Komplexität, Tool muss entwickelt werden                                                                                                            
