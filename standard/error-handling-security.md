# Error Handling & Security

Umfassendes Error Handling und operationalisierte Security-Regeln für LLM-Agenten.

---

## Error Handling

### Allgemein
- MUST: Umfassendes Error Handling für alle externen Aufrufe (API, File I/O, Network)
- MUST: Benutzerfreundliche Fehlermeldungen bereitstellen
- MUST NOT: Exceptions unbehandelt durchreichen

### Logging
- SHOULD: Logging für Debugging implementieren
- SHOULD: Log-Level sinnvoll nutzen (info, warn, error)
- MUST NOT: Secrets oder Credentials in Logs ausgeben
- Begründung: Debugging in Produktion ermöglichen, Secrets schützen

### Spezifische Fehlertypen
- MUST: Network Errors abfangen (Timeout, Offline, Connection Refused)
- MUST: Auth-Fehler behandeln und klar kommunizieren (401, 403)
- MUST NOT: Auth-Fehler ignorieren (Security-Risiko)

### Timeouts
- MUST: Timeouts für API-Requests und externe Aufrufe setzen
- SHOULD: Sinnvolle Default-Timeouts (z.B. 10 Sekunden)
- Begründung: Verhindert hängende Anwendungen

---

## Security & Safety

### Übergreifende Prinzipien

**Alle Stacks:**
- MUST: Security by Default
- MUST: Principle of Least Privilege
- MUST: Defense in Depth
- MUST: Bei Unsicherheit: User fragen, nicht raten

---

## Generische Security-Regeln

### Secrets & Credentials

#### MUST NOT

- **Hardcodierte Secrets** im Code
  - Keine API-Keys, Passwords, Tokens
  - Keine Connection Strings mit Credentials
  - Keine Private Keys oder Certificates

- **Secrets in Logs** ausgeben
  - Keine vollständigen API-Keys
  - Keine Passwords (auch nicht maskiert)
  - Keine Tokens

- **Secrets in Git** committen
  - Keine .env-Dateien mit echten Werten
  - Keine Config-Dateien mit Credentials
  - Keine Private Keys

- **Secrets in Fehlermeldungen**
  - Keine API-Keys in Stack-Traces
  - Keine Credentials in Error-Messages

#### MUST

- **Credentials aus Environment-Variablen** lesen
  ```python
  # Beispiel
  import os
  api_key = os.getenv("API_KEY")
  if not api_key:
      raise ValueError("API_KEY environment variable not set")
  ```

- **Config-Dateien mit Secrets in .gitignore** eintragen
  ```
  # .gitignore
  .env
  .env.local
  config.local.yml
  secrets.json
  ```

- **Secrets maskieren** in Logs
  ```python
  # Beispiel
  logger.info(f"Using API key: {api_key[:8]}***")  # Nur ersten 8 Zeichen
  ```

- **Template-Dateien** mit Platzhaltern committen
  ```yaml
  # config.template.yml
  api_key: "YOUR_API_KEY_HERE"
  database_url: "postgresql://user:password@localhost/db"
  ```

---

### Command Injection Prevention

#### MUST NOT

- **User-Input direkt in Shell-Commands**
  ```python
  # ❌ FALSCH
  os.system(f"rm {user_file}")
  ```

- **String-Interpolation für Commands** mit User-Input
  ```bash
  # ❌ FALSCH
  rm $user_input
  ```

- **eval(), exec()** oder äquivalent mit User-Input
  ```python
  # ❌ FALSCH
  eval(user_input)
  ```

#### MUST

- **User-Input validieren und sanitizen**
  ```python
  # Beispiel: Whitelist-Ansatz
  allowed_chars = set("abcdefghijklmnopqrstuvwxyz0123456789-_.")
  if not all(c in allowed_chars for c in user_input):
      raise ValueError("Invalid characters in input")
  ```

- **Parameterized APIs** nutzen (nicht Shell)
  ```python
  # ✅ RICHTIG
  import subprocess
  subprocess.run(["rm", user_file])  # List, nicht shell=True
  ```

- **Whitelisting** statt Blacklisting
  ```python
  # Beispiel
  ALLOWED_COMMANDS = ["ls", "cat", "grep"]
  if command not in ALLOWED_COMMANDS:
      raise ValueError(f"Command not allowed: {command}")
  ```

---

### Destructive Operations

#### MUST

- **Confirmation** vor destructive Operations
  - DELETE, DROP, TRUNCATE, rm -rf
  - Format, Partition-Operations
  - Massenänderungen

- **Dry-run Option** anbieten
  ```python
  # Beispiel
  if not dry_run:
      os.remove(file_path)
  else:
      print(f"Would delete: {file_path}")
  ```

- **Backups** vor destructive Operations (wenn möglich)

#### MUST NOT

- **Recursive delete ohne Safeguards**
  ```bash
  # ❌ FALSCH
  rm -rf $directory  # Wenn $directory leer: Katastrophe
  ```

- **DROP ohne WHERE-Clause** (SQL)
  ```sql
  -- ❌ FALSCH
  DROP TABLE users;  -- Ohne Confirmation
  ```

- **Truncate ohne Confirmation**

---

### Path Traversal Prevention

#### MUST

- **Path-Validierung** vor File-Operations
  ```python
  import os
  safe_path = os.path.normpath(f"/data/{user_file}")
  if not safe_path.startswith("/data/"):
      raise ValueError("Path traversal detected")
  ```

- **os.path.join()** für Pfade nutzen
  ```python
  # ✅ RICHTIG
  path = os.path.join(base_dir, user_subdir, filename)
  ```

#### MUST NOT

- **Unsanitized Pfade** aus User-Input
  ```python
  # ❌ FALSCH
  with open(f"/data/{user_path}") as f:  # ../ möglich
  ```

- **String-Concatenation** für Pfade
  ```python
  # ❌ FALSCH
  path = "/data/" + user_input
  ```

---

## Python-spezifische Security-Regeln

Siehe: [project-templates/python-stack.md](../project-templates/python-stack.md) Security-Sektion

**Risiken:**
- eval/exec Missbrauch
- SQL Injection
- Pickle Deserialization
- subprocess mit shell=True
- Path Traversal

**Key Rules:**
- MUST: `ast.literal_eval()` statt `eval()`
- MUST: Parameterized Queries (SQL)
- MUST: `subprocess.run()` mit List-Arguments (nicht shell=True)
- MUST NOT: `pickle.loads()` mit untrusted data

---

## Shell / Linux-spezifische Security-Regeln

Siehe: [project-templates/shell-scripting.md](../project-templates/shell-scripting.md) Security-Sektion

**Risiken:**
- rm -rf Missbrauch
- Unquoted Variables
- curl piped to sh
- sudo ohne Validierung
- Wildcard Injection

**Key Rules:**
- MUST: Quote all variables: `"$var"`
- MUST NOT: `rm -rf /` oder root-level deletes
- MUST: Validate before `sudo`
- MUST NOT: `curl <url> | sh`

---

## Windows CMD / MS-DOS-spezifische Security-Regeln

Siehe: [project-templates/shell-scripting.md](../project-templates/shell-scripting.md) Windows-Sektion

**Risiken:**
- del /s /q Missbrauch
- Command Injection
- Unquoted Paths mit Leerzeichen

**Key Rules:**
- MUST: Quotes für Paths mit Leerzeichen: `"%var%"`
- MUST NOT: `del /s /q C:\` oder root-level deletes
- MUST: Validate Paths vor Commands

---

## Frontend (html/js/css)-spezifische Security-Regeln

Siehe: [project-templates/frontend.md](../project-templates/frontend.md) Security-Sektion

**Risiken:**
- XSS (Cross-Site Scripting)
- Hardcoded API-Keys
- Unsichere localStorage
- eval() Missbrauch
- CORS Misconfiguration

**Key Rules:**
- MUST: `.textContent` statt `.innerHTML` für User-Content
- MUST NOT: API-Keys im JavaScript-Code
- MUST NOT: Passwords in localStorage
- MUST: Backend-Proxy für API-Calls mit Secrets

---

## Security Checklist (für LLM-Agenten)

### Vor jeder Code-Generierung

**Fragen stellen:**

1. **Secrets:** Enthält der Code Secrets oder Credentials?
   - Falls ja: Aus Environment-Variablen lesen
   
2. **User-Input:** Wird User-Input verarbeitet?
   - Falls ja: Validieren und sanitizen
   - Keine String-Interpolation in Commands

3. **Destructive Operation:** Löscht oder ändert der Code Daten?
   - Falls ja: Confirmation erforderlich
   - Dry-run Option anbieten

4. **External Calls:** API, Subprocess, File I/O?
   - Falls ja: Error Handling + Timeouts
   - Keine unbehandelten Exceptions

5. **Privileged Operations:** sudo, admin-rights?
   - Falls ja: User fragen
   - Principle of Least Privilege

### Bei Unsicherheit

**MUST:** User fragen, nicht raten

```
f1: Soll dieser Code API-Keys aus Environment-Variablen lesen?
f2: Ist eine Confirmation für diese Delete-Operation gewünscht?
```

---

## Logging Security

### MUST

- **Mask Secrets** in Logs
  ```python
  # Beispiel
  logger.info(f"API call with key: {api_key[:8]}***")
  ```

- **Separate Logs** für Security-Events
  ```python
  security_logger.warning(f"Failed login attempt from IP: {ip}")
  ```

### MUST NOT

- **Full Secrets** in Logs
  ```python
  # ❌ FALSCH
  logger.debug(f"Using API key: {api_key}")
  ```

- **Passwords** in Logs (auch nicht gehashed)
  ```python
  # ❌ FALSCH
  logger.info(f"User logged in with password: ***")  # Trotzdem falsch
  ```

- **Sensitive PII** in Logs
  ```python
  # ❌ FALSCH
  logger.info(f"Processing SSN: {ssn}")
  ```

---

## Error Messages Security

### MUST

- **Generic Error Messages** nach außen
  ```python
  # ✅ RICHTIG
  return {"error": "Authentication failed"}
  ```

- **Detailed Error Messages** nur intern (Logs)
  ```python
  logger.error(f"Auth failed: Invalid API key format: {api_key[:8]}***")
  ```

### MUST NOT

- **Stack Traces** mit Secrets nach außen
  ```python
  # ❌ FALSCH
  return {"error": str(exception)}  # Kann Secrets enthalten
  ```

- **Database Errors** ungefiltert nach außen
  ```python
  # ❌ FALSCH
  return {"error": f"SQL error: {sql_exception}"}  # Zeigt DB-Struktur
  ```

---

## Configuration Files

### MUST

- **.gitignore** für Secrets
  ```
  # .gitignore
  .env
  .env.local
  .env.*.local
  config.local.*
  secrets.*
  *.key
  *.pem
  ```

- **Template-Dateien** committen
  ```yaml
  # config.template.yml (committed)
  api_key: "YOUR_API_KEY_HERE"
  database:
    host: "localhost"
    password: "YOUR_PASSWORD_HERE"
  ```

- **README** mit Setup-Anweisungen
  ```markdown
  ## Setup
  1. Copy `config.template.yml` to `config.yml`
  2. Edit `config.yml` and add your credentials
  3. Ensure `config.yml` is in .gitignore
  ```

### MUST NOT

- **Real Secrets** in committed files
- **Credentials** in commit history
- **API-Keys** in example code

---

## Zusammenfassung

### Top Security-Regeln (alle Stacks)

1. **MUST NOT:** Secrets hardcoden
2. **MUST NOT:** User-Input in Commands ohne Validation
3. **MUST:** Confirmation vor Destructive Ops
4. **MUST:** Error Handling + Timeouts
5. **MUST:** Path-Validierung
6. **MUST:** Mask Secrets in Logs
7. **MUST:** Bei Unsicherheit: User fragen

### Stack-spezifische Details

Siehe entsprechende Template-Dokumentation:
- **Python:** [project-templates/python-stack.md](../project-templates/python-stack.md)
- **Shell/Linux:** [project-templates/shell-scripting.md](../project-templates/shell-scripting.md)
- **Windows CMD:** [project-templates/shell-scripting.md](../project-templates/shell-scripting.md)
- **Frontend:** [project-templates/frontend.md](../project-templates/frontend.md)

### Bei Unsicherheit

**Immer User fragen, niemals raten bei Security-Fragen.**
