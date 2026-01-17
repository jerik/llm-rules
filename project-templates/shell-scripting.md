# Shell Scripting

Template für Shell-Scripting (Bash, sh) und Windows CMD/PowerShell.

---

## Shell / Linux (Bash, sh)

### Coding Standards

#### Quoting
- MUST: Quote all variables: `"$var"`
- MUST: Quote command substitutions: `"$(command)"`
- Begründung: Verhindert Word-Splitting und Glob-Expansion

#### Error Handling
- SHOULD: `set -e` (exit on error)
- SHOULD: `set -u` (exit on undefined variable)
- SHOULD: `set -o pipefail` (pipe fails if any command fails)

```bash
#!/usr/bin/env bash
set -euo pipefail
```

#### Functions
- SHOULD: Funktionen statt Inline-Code
- SHOULD: `local` für Function-Variables

```bash
# Beispiel
my_function() {
    local var="$1"
    echo "Processing: $var"
}
```

---

### Security

#### Destructive Commands

**MUST NOT:**
- `rm -rf /` oder root-level deletes
- `sudo rm -rf` ohne explizite Confirmation
- `> /dev/sda` oder Disk-Overwrite-Commands

**MUST:**
- Variable validation vor `rm`
- Quotes: `rm "$file"` nicht `rm $file`

**Beispiel:**
```bash
# ❌ FALSCH
directory="$1"
rm -rf $directory  # Wenn $directory leer: rm -rf (löscht alles!)

# ✅ RICHTIG
directory="$1"
if [ -z "$directory" ]; then
    echo "ERROR: directory parameter is empty" >&2
    exit 1
fi

if [ ! -d "$directory" ]; then
    echo "ERROR: directory does not exist: $directory" >&2
    exit 1
fi

# Optional: Confirmation
echo "About to delete: $directory"
read -p "Continue? (y/n) " -n 1 -r
echo
if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    exit 1
fi

rm -rf "$directory"
```

---

#### Command Injection

**MUST NOT:**
- Unquoted Variables
- `eval` mit User-Input
- `curl <url> | sh`
- Command substitution mit User-Input ohne Validation

**MUST:**
- Validate Input
- Use Arrays for Commands

**Beispiel:**
```bash
# ❌ FALSCH
user_file="$1"
cat $user_file  # Wenn user_file="-n /etc/passwd": cat interpretiert -n

# ✅ RICHTIG
user_file="$1"

# Validation
if [[ ! "$user_file" =~ ^[a-zA-Z0-9._-]+$ ]]; then
    echo "ERROR: Invalid filename" >&2
    exit 1
fi

if [ ! -f "$user_file" ]; then
    echo "ERROR: File not found: $user_file" >&2
    exit 1
fi

cat "$user_file"
```

---

#### Privilege Operations

**MUST:**
- Explizite Confirmation für `sudo`-Commands
- Principle of Least Privilege
- Logging von sudo-Operations

**MUST NOT:**
- `sudo su` in Scripts ohne Grund
- Blanket sudo für ganze Scripts

**Beispiel:**
```bash
# ❌ FALSCH
sudo rm -rf /var/log/*

# ✅ RICHTIG
echo "About to delete log files (requires sudo)"
read -p "Continue? (y/n) " -n 1 -r
echo
if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    exit 1
fi

sudo rm -rf /var/log/*.log
echo "Log files deleted (logged to syslog)"
```

---

#### Wildcard Injection

**Problem:** Wildcards können zu Filenames expandieren, die wie Options aussehen

**MUST:**
- Explicit Paths statt Wildcards bei kritischen Ops
- `--` Separator nutzen
- `./*` statt `*`

**Beispiel:**
```bash
# ❌ GEFÄHRLICH
# Wenn File "-rf" existiert: rm * → rm -rf (löscht rekursiv!)
rm *.txt

# ✅ RICHTIG
rm -- *.txt         # -- signalisiert Ende der Options
# oder
rm ./*.txt          # ./ verhindert Option-Interpretation
```

---

### Tooling

#### Linting
- SHOULD: `shellcheck` verwenden
- MUST: Zero errors

```bash
shellcheck script.sh
```

#### Testing

Siehe auch: [standard/testing.md](../standard/testing.md)

**bats (Bash Automated Testing System):**
- SHOULD: bats für Tests verwenden
- SHOULD: Test edge cases (empty vars, missing files, invalid options)

**Installation:**
```bash
# Ubuntu/Debian
sudo apt-get install bats

# macOS
brew install bats-core
```

**Test-Struktur:**
```
project/
├── script.sh
└── test/
    └── script.bats
```

**Beispiel:**
```bash
#!/usr/bin/env bats

# test/script.bats

@test "script returns 0 on success" {
  run ./script.sh --help
  [ "$status" -eq 0 ]
}

@test "script outputs help text" {
  run ./script.sh --help
  [[ "$output" =~ "Usage:" ]]
}

@test "script fails with invalid option" {
  run ./script.sh --invalid
  [ "$status" -ne 0 ]
}

@test "script handles empty input" {
  run ./script.sh ""
  [ "$status" -ne 0 ]
  [[ "$output" =~ "ERROR" ]]
}

@test "script validates file exists" {
  run ./script.sh nonexistent.txt
  [ "$status" -ne 0 ]
  [[ "$output" =~ "File not found" ]]
}
```

**Assertions:**
```bash
# Exit status
[ "$status" -eq 0 ]      # Success
[ "$status" -ne 0 ]      # Failure

# Output matching
[[ "$output" =~ "pattern" ]]    # Regex match
[ "$output" = "exact" ]         # Exact match

# File checks (in test setup)
[ -f "file.txt" ]        # File exists
[ -d "directory" ]       # Directory exists
```

**Setup/Teardown:**
```bash
#!/usr/bin/env bats

setup() {
  # Run before each test
  export TEST_DIR="$(mktemp -d)"
  export TEST_FILE="$TEST_DIR/test.txt"
}

teardown() {
  # Run after each test
  rm -rf "$TEST_DIR"
}

@test "creates file" {
  ./script.sh create "$TEST_FILE"
  [ -f "$TEST_FILE" ]
}
```

**Run Tests:**
```bash
# All tests
bats test/*.bats

# Specific test file
bats test/script.bats

# Verbose output
bats -t test/*.bats
```

---

### Definition of Done (Beispiel)

```bash
# 1. Shellcheck
shellcheck *.sh

# 2. Tests
bats test/*.bats

# Bei Fehler: Iterieren bis alle grün!
```

---

## Windows CMD / MS-DOS

### Coding Standards

#### Quoting
- MUST: Quotes für Paths mit Leerzeichen: `"%var%"`
- MUST: Quotes für alle Variables in Commands

#### Error Handling
- SHOULD: `if errorlevel 1` nach kritischen Commands
- SHOULD: `@echo off` am Anfang

```cmd
@echo off
setlocal enabledelayedexpansion

some_command
if errorlevel 1 (
    echo ERROR: Command failed
    exit /b 1
)
```

---

### Security

#### Destructive Commands

**MUST NOT:**
- `del /s /q C:\` oder root-level deletes
- `format C:` ohne Confirmation
- `rmdir /s /q` auf System-Ordner

**MUST:**
- Path-Validierung vor `del`
- Quotes: `del "%file%"` nicht `del %file%`

**Beispiel:**
```cmd
@echo off
set "directory=%~1"

REM Validation
if "%directory%"=="" (
    echo ERROR: directory parameter is empty
    exit /b 1
)

if not exist "%directory%" (
    echo ERROR: directory does not exist: %directory%
    exit /b 1
)

REM Confirmation
echo About to delete: %directory%
set /p confirm="Continue? (Y/N): "
if /i not "%confirm%"=="Y" exit /b 1

rmdir /s /q "%directory%"
```

---

#### Command Injection

**MUST NOT:**
- Unquoted Variables
- User-Input direkt in Commands

**MUST:**
- Validate Paths
- Quotes für alle Variables

**Beispiel:**
```cmd
REM ❌ FALSCH
set userfile=%1
del %userfile%  REM Injection-Risiko

REM ✅ RICHTIG
set "userfile=%~1"

if not exist "%userfile%" (
    echo ERROR: File not found: %userfile%
    exit /b 1
)

del "%userfile%"
```

---

#### Path mit Leerzeichen

**MUST:**
- Alle Paths in Quotes
- `%~1` nutzt (entfernt umgebende Quotes sicher)

**Beispiel:**
```cmd
REM ❌ FALSCH
set folder=C:\Program Files\App
cd %folder%  REM Fehler: cd versucht "C:\Program" zu öffnen

REM ✅ RICHTIG
set "folder=C:\Program Files\App"
cd "%folder%"
```

---

### PowerShell

Falls PowerShell verwendet wird:

```powershell
# Execution Policy beachten
# Best Practice: Signed Scripts

# Error Handling
$ErrorActionPreference = "Stop"

# Parameter Validation
param(
    [Parameter(Mandatory=$true)]
    [ValidateScript({Test-Path $_})]
    [string]$FilePath
)

# Deletion mit Confirmation
Remove-Item -Path $FilePath -Confirm
```

---

## Zusammenfassung

### Shell/Linux Key Rules

1. **MUST:** Quote all variables: `"$var"`
2. **MUST:** Validate vor `rm -rf`
3. **MUST NOT:** `rm -rf /` oder root-level
4. **MUST NOT:** `eval` mit User-Input
5. **MUST:** Confirmation für `sudo`
6. **SHOULD:** `shellcheck` verwenden

### Windows CMD Key Rules

1. **MUST:** Quotes für Paths: `"%var%"`
2. **MUST:** Validate vor `del /s /q`
3. **MUST NOT:** `del /s /q C:\` oder root-level
4. **MUST:** Path-Validierung
5. **SHOULD:** Error-Level checking

### Generell

- **Immer validieren vor destructive Operations**
- **Immer quoten**
- **Immer Confirmation bei kritischen Ops**
