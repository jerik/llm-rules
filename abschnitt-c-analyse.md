# Abschnitt C – Analyse: Security & Safety

**Status:** Vorschlag  
**Datum:** 2025-01-17

---

## 1. Zielsetzung

Operationalisierung bestehender Security-Grundsätze in **konkretes Agentenverhalten** für:
- Python
- Shell / Linux
- Windows CMD / MS-DOS
- Frontend (html/js/css)

**Fokus:** Praktische Risikominimierung, keine erschöpfende Security-Guideline

---

## 2. Analyse bestehender Security-Regeln

### Aktuelle Regeln (standard/error-handling.md)

**Credentials:**
- MUST NOT: API-Keys, Tokens oder Passwörter im Code hardcoden
- MUST NOT: Credentials in Git committen
- MUST: Credentials in .gitignore eintragen
- MUST: Externe Config-Dateien oder Umgebungsvariablen nutzen

**Auth-Fehler:**
- MUST: Auth-Fehler behandeln (401, 403)
- MUST NOT: Auth-Fehler ignorieren

**Status:** ✅ Gut, aber generisch

**Bedarf:** Stack-spezifische Konkretisierung

---

## 3. Identifizierte Risikoszenarien

### Übergreifende Risiken (alle Stacks)

| Risiko | Beschreibung | Häufigkeit | Schwere |
|--------|--------------|------------|---------|
| **Hardcodierte Secrets** | API-Keys, Passwords im Code | Sehr hoch | Kritisch |
| **Secrets in Logs** | Credentials in Logfiles | Hoch | Kritisch |
| **Command Injection** | User-Input in Shell-Commands | Mittel | Kritisch |
| **Path Traversal** | Unsichere Dateipfade | Mittel | Hoch |
| **Destructive Commands** | rm -rf, DROP TABLE ohne Safeguards | Niedrig | Kritisch |

---

### Python-spezifische Risiken

| Risiko | Beschreibung | Beispiel | Schwere |
|--------|--------------|----------|---------|
| **eval/exec Missbrauch** | Code-Injection via eval() | `eval(user_input)` | Kritisch |
| **SQL Injection** | Unsichere SQL-Queries | `f"SELECT * FROM users WHERE id={user_id}"` | Kritisch |
| **Pickle Deserialization** | Unsichere Deserialisierung | `pickle.loads(untrusted_data)` | Kritisch |
| **subprocess ohne Shell** | Shell-Injection via subprocess | `subprocess.run(f"ls {user_input}", shell=True)` | Kritisch |
| **Unsichere Dependencies** | Bekannte Vulnerabilities in Packages | Alte Library-Versionen | Mittel |

---

### Shell / Linux-spezifische Risiken

| Risiko | Beschreibung | Beispiel | Schwere |
|--------|--------------|----------|---------|
| **rm -rf Missbrauch** | Versehentliches Löschen | `rm -rf /` | Kritisch |
| **Unquoted Variables** | Command Injection | `rm $file` statt `rm "$file"` | Hoch |
| **curl piped to sh** | Unsicheres Script-Execution | `curl url | sh` | Kritisch |
| **sudo ohne Validierung** | Privilegien-Escalation | `sudo rm -rf /var` | Kritisch |
| **Wildcard Injection** | Unerwartete Expansion | `rm *.txt` wenn File -rf.txt existiert | Hoch |

---

### Windows CMD / MS-DOS-spezifische Risiken

| Risiko | Beschreibung | Beispiel | Schwere |
|--------|--------------|----------|---------|
| **del /s /q Missbrauch** | Versehentliches Löschen | `del /s /q C:\` | Kritisch |
| **Command Injection** | Unsichere Variable-Expansion | `del %userfile%` | Hoch |
| **PowerShell Execution** | Unsichere Script-Ausführung | `powershell -c "script"` | Hoch |
| **Path mit Leerzeichen** | Unquoted Paths | `del C:\Program Files\file.txt` | Mittel |

---

### Frontend (html/js/css)-spezifische Risiken

| Risiko | Beschreibung | Beispiel | Schwere |
|--------|--------------|----------|---------|
| **XSS (Cross-Site Scripting)** | Unsanitized User-Input | `innerHTML = user_input` | Kritisch |
| **Hardcoded API-Keys** | Keys im JavaScript-Code | `const apiKey = "abc123"` | Kritisch |
| **Unsichere localStorage** | Sensitive Data in Browser | `localStorage.setItem("password", pw)` | Hoch |
| **eval() Missbrauch** | Code-Injection | `eval(user_input)` | Kritisch |
| **CORS Misconfiguration** | Zu permissive CORS | `Access-Control-Allow-Origin: *` | Mittel |

---

## 4. Vorgeschlagene Regeln

### Generische Security-Regeln (alle Stacks)

#### Secrets & Credentials

**MUST NOT:**
- Hardcodierte Secrets (API-Keys, Passwords, Tokens) im Code
- Secrets in Logs ausgeben
- Secrets in Git committen
- Secrets in Fehlermeldungen

**MUST:**
- Credentials aus Environment-Variablen oder Config-Dateien lesen
- Config-Dateien mit Secrets in .gitignore
- Secrets maskieren in Logs (z.B. `api_key=***`)

**Beispiel (Python):**
```python
# ❌ FALSCH
api_key = "sk-abc123def456"

# ✅ RICHTIG
import os
api_key = os.getenv("API_KEY")
if not api_key:
    raise ValueError("API_KEY environment variable not set")
```

---

#### Command Injection Prevention

**MUST NOT:**
- User-Input direkt in Shell-Commands
- String-Interpolation für Commands mit User-Input
- `eval()`, `exec()` oder äquivalent mit User-Input

**MUST:**
- User-Input validieren und sanitizen
- Parameterized APIs nutzen (nicht Shell)
- Whitelisting statt Blacklisting

---

#### Destructive Operations

**MUST:**
- Confirmation vor destructive Operations (delete, drop, truncate)
- Dry-run Option anbieten
- Backups vor destructive Operations

**MUST NOT:**
- Recursive delete ohne Safeguards
- DROP ohne WHERE-Clause (SQL)
- Truncate ohne Confirmation

---

### Python-spezifische Regeln

#### Code Execution

**MUST NOT:**
- `eval(user_input)` oder `exec(user_input)`
- `pickle.loads()` mit untrusted data
- `subprocess.run(..., shell=True)` mit User-Input
- `__import__(user_input)`

**MUST:**
- `ast.literal_eval()` statt `eval()` für sichere Data-Parsing
- `subprocess.run()` mit List-Arguments (nicht shell=True)
- JSON/YAML für Data, nicht pickle bei untrusted sources

**Beispiel:**
```python
# ❌ FALSCH
result = eval(user_input)
subprocess.run(f"ls {user_path}", shell=True)

# ✅ RICHTIG
import ast
result = ast.literal_eval(user_input)  # Nur für literals
subprocess.run(["ls", user_path])       # Kein shell=True
```

---

#### SQL Injection Prevention

**MUST:**
- Parameterized Queries (Prepared Statements)
- ORM mit Parameter-Binding

**MUST NOT:**
- String-Interpolation für SQL-Queries
- F-Strings mit User-Input für SQL

**Beispiel:**
```python
# ❌ FALSCH
cursor.execute(f"SELECT * FROM users WHERE id={user_id}")

# ✅ RICHTIG
cursor.execute("SELECT * FROM users WHERE id=?", (user_id,))
```

---

#### File Operations

**MUST:**
- Path-Validierung (kein ../, keine absoluten Pfade wenn nicht nötig)
- `os.path.join()` für Pfade, keine String-Concatenation
- Permission-Checks vor File-Access

**MUST NOT:**
- Unsanitized Pfade aus User-Input
- `open(user_path)` ohne Validierung

**Beispiel:**
```python
# ❌ FALSCH
with open(f"/data/{user_file}") as f:  # Path Traversal möglich

# ✅ RICHTIG
import os
safe_path = os.path.normpath(f"/data/{user_file}")
if not safe_path.startswith("/data/"):
    raise ValueError("Invalid path")
with open(safe_path) as f:
    ...
```

---

### Shell / Linux-spezifische Regeln

#### Destructive Commands

**MUST NOT:**
- `rm -rf /` oder ähnliche root-level deletes
- `sudo rm -rf` ohne explizite Confirmation
- `> /dev/sda` oder Disk-Overwrite-Commands

**MUST:**
- Quote all variables: `rm "$file"` nicht `rm $file`
- Explizite Pfade statt Wildcards bei delete
- Dry-run zeigen vor destructive Operations

**Beispiel:**
```bash
# ❌ FALSCH
rm -rf $directory    # Wenn $directory leer: rm -rf (löscht alles)

# ✅ RICHTIG
if [ -z "$directory" ]; then
    echo "ERROR: directory variable is empty"
    exit 1
fi
rm -rf "$directory"
```

---

#### Command Construction

**MUST:**
- Variablen in Quotes: `"$var"`
- Arrays für Command-Arguments

**MUST NOT:**
- Unquoted Variables
- `eval` mit User-Input
- `curl <url> | sh`

**Beispiel:**
```bash
# ❌ FALSCH
file=$1
rm $file  # Wenn $file = "-rf /" → Katastrophe

# ✅ RICHTIG
file="$1"
if [ ! -f "$file" ]; then
    echo "File not found: $file"
    exit 1
fi
rm "$file"
```

---

#### Privilege Operations

**MUST:**
- Explizite Confirmation für sudo-Commands
- Principle of least privilege
- Logging von sudo-Operations

**MUST NOT:**
- `sudo su` in Scripts ohne Grund
- Blanket sudo für ganze Scripts

---

### Windows CMD / MS-DOS-spezifische Regeln

#### Destructive Commands

**MUST NOT:**
- `del /s /q C:\` oder ähnliche root-level deletes
- `format C:` ohne Confirmation
- `rmdir /s /q` auf System-Ordner

**MUST:**
- Quotes für Paths mit Leerzeichen: `del "C:\Program Files\file.txt"`
- Explizite Pfade statt Wildcards bei delete
- Confirmation vor destructive Operations

---

#### Command Construction

**MUST:**
- Variables in Quotes: `"%var%"`
- Validate Paths vor Commands

**MUST NOT:**
- Unquoted Variables
- User-Input direkt in Commands

**Beispiel:**
```cmd
REM ❌ FALSCH
del %userfile%

REM ✅ RICHTIG
if not exist "%userfile%" (
    echo File not found: %userfile%
    exit /b 1
)
del "%userfile%"
```

---

### Frontend (html/js/css)-spezifische Regeln

#### XSS Prevention

**MUST:**
- Sanitize User-Input vor Output
- Use `.textContent` statt `.innerHTML` für User-Content
- Framework-eigene Escaping nutzen (React, Vue, etc.)

**MUST NOT:**
- `innerHTML = user_input` ohne Sanitization
- `eval(user_input)`
- `document.write(user_input)`

**Beispiel:**
```javascript
// ❌ FALSCH
element.innerHTML = userInput;  // XSS-Risiko

// ✅ RICHTIG
element.textContent = userInput;  // Automatisch escaped
```

---

#### Secrets in Frontend

**MUST NOT:**
- API-Keys im JavaScript-Code
- Passwords oder Tokens in localStorage ohne Encryption
- Sensitive Data in URL-Parameters

**MUST:**
- Backend-Proxy für API-Calls mit Secrets
- HttpOnly Cookies für Session-Tokens
- HTTPS für alle Requests mit sensitive Data

**Beispiel:**
```javascript
// ❌ FALSCH
const apiKey = "sk-abc123def456";  // Im Client sichtbar
fetch(`https://api.example.com?key=${apiKey}`);

// ✅ RICHTIG
// Backend-Proxy nutzen, API-Key im Backend halten
fetch("https://backend.example.com/api/proxy");
```

---

#### Data Storage

**MUST:**
- Session-Data in sessionStorage (gelöscht bei Tab-Close)
- Sensitive Data NUR in httpOnly Cookies (Server-side)
- Encryption für sensitive Data in localStorage (wenn unvermeidbar)

**MUST NOT:**
- Passwords in localStorage
- Session-Tokens in localStorage
- PII (Personally Identifiable Information) unverschlüsselt

---

## 5. Vorschlag: Struktur der Dokumentation

### Option A: Ein zentrales Dokument

**Datei:** `standard/security-safety.md`

**Struktur:**
1. Generische Regeln (alle Stacks)
2. Python-spezifische Regeln
3. Shell/Linux-spezifische Regeln
4. Windows CMD-spezifische Regeln
5. Frontend-spezifische Regeln

**Vorteil:** Zentral, einfache Referenz  
**Nachteil:** Lang, möglicherweise unübersichtlich

---

### Option B: Aufsplitten

**Generisch:**
- `standard/security-safety.md` (generische Regeln)

**Stack-spezifisch:**
- Ergänzungen in bestehenden Templates
- `project-templates/python-stack.md` → Security-Sektion hinzufügen
- `project-templates/shell-scripting.md` → Neu erstellen mit Security
- etc.

**Vorteil:** Regeln dort, wo sie gebraucht werden  
**Nachteil:** Fragmentiert

---

### Empfehlung: Hybrid

**Zentral:**
- `standard/security-safety.md` mit generischen Regeln + Übersicht

**Stack-spezifisch:**
- Kurze Security-Sektionen in Templates mit Verweis auf zentrale Regeln

**Vorteil:** Best of both worlds

---

## 6. Ergänzung zu bestehendem error-handling.md

**Vorschlag:** Erweitere `standard/error-handling.md` um Security-Sektion

**Alternativ:** Benenne um zu `standard/error-handling-security.md`

**Oder:** Neues Dokument `standard/security-safety.md`

---

## 7. Offene Fragen

### Entscheidungsbedarf

1. **Dokumenten-Struktur:**
   - Ein zentrales `security-safety.md`?
   - Oder aufsplitten in Templates?
   - Oder hybrid?

2. **Bestehende error-handling.md:**
   - Erweitern um Security-Details?
   - Umbenennen?
   - Oder separates Dokument?

3. **Stack-Coverage:**
   - Sind die 4 Stacks (Python, Shell/Linux, Windows CMD, Frontend) ausreichend?
   - Fehlt ein Stack? (z.B. Docker, Kubernetes?)

4. **Tiefe der Regeln:**
   - Sind die Regeln konkret genug?
   - Zu detailliert oder zu allgemein?

5. **Neue Templates:**
   - Soll `project-templates/shell-scripting.md` erstellt werden?
   - Soll `project-templates/frontend.md` erstellt werden?

---

## 8. Vorschlag für plan.md Update

### Status Abschnitt C

```markdown
## Abschnitt C – Security & Safety (Punkt 5)

Status: Analyse abgeschlossen, Vorschlag erstellt

### Durchgeführt
- Analyse bestehender Security-Regeln (standard/error-handling.md)
- Identifikation übergreifender Risikoszenarien (5 Kategorien)
- Identifikation stack-spezifischer Risiken:
  - Python: 5 Risiken
  - Shell/Linux: 5 Risiken
  - Windows CMD: 4 Risiken
  - Frontend: 5 Risiken
- Entwurf operationalisierter Regeln mit Code-Beispielen
- Strukturvorschlag: Hybrid (zentral + stack-spezifisch)

### Output
- abschnitt-c-analyse.md (diese Datei)

### Offene Entscheidungen
1. Dokumenten-Struktur: Zentral, aufgesplittet oder hybrid?
2. error-handling.md erweitern oder separates Dokument?
3. Stack-Coverage ausreichend?
4. Tiefe der Regeln angemessen?
5. Neue Templates für Shell/Frontend erstellen?

### Nächste Schritte
- Entscheidungen durch Stakeholder einholen
- Nach Freigabe: Dokumentation erstellen
```

---

## 9. Zusammenfassung

**Risiko-Schwerpunkte:**
- Hardcodierte Secrets (alle Stacks)
- Command Injection (Python, Shell, CMD)
- Destructive Operations (Shell, CMD)
- XSS (Frontend)
- SQL Injection (Python)

**Vorgeschlagene Regeln:**
- **Generisch:** Secrets, Command Injection, Destructive Ops
- **Python:** eval/exec, SQL Injection, subprocess, File Ops
- **Shell/Linux:** rm -rf, Unquoted Vars, sudo
- **Windows CMD:** del /s /q, Unquoted Paths
- **Frontend:** XSS, Hardcoded Keys, localStorage

**Empfehlung Struktur:**
- Hybrid: Zentrale `security-safety.md` + Stack-spezifische Sektionen in Templates

**Nächster Schritt:**
- Entscheidung über offene Fragen
- Danach: Implementierung

---

**Status:** Vorschlag bereit für Review
