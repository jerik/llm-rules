Ich beantworte **konkret und operativ**, ohne Theorieballast.
Wir klären **was Punkt 4 ist**, **wo es liegt** und **wie du globale Regeln sauber einbindest** (inkl. Links).

---

## 1) Was ist „Punkt 4“ konkret?

**Punkt 4 = eine globale, persönliche Baseline für pi**, die **immer gilt**, unabhängig vom Projekt.

Zweck:

* Dinge, die **immer gleich bleiben**
* die **nicht projektspezifisch** sind
* und die du **nicht in jedes Repo kopieren willst**

Beispiele:

* Kommunikationsstil
* Arbeitsdisziplin
* Sicherheitsgrundsätze
* „keine impliziten Entscheidungen“
* „plan.md ist bindend“

👉 Das ist **kein Projekt-Wissen**, sondern **dein persönliches Arbeitsmodell**.

---

## 2) Wo liegt das bei pi? (konkret)

pi lädt **automatisch** folgende Datei, falls sie existiert:

```text
~/.pi/agent/AGENTS.md
```

Das ist der **globale Systemkontext** für dich als Nutzer.

Reihenfolge (wichtig):

1. `~/.pi/agent/AGENTS.md`  ← global
2. `./AGENTS.md` im Projekt ← projekt-spezifisch
3. optional: CLI `--append-system-prompt`

➡️ **Globales gilt immer**, Projekt kann ergänzen oder spezifizieren.

---

## 3) Was gehört dort hinein – und was nicht?

### ✅ Gehört in `~/.pi/agent/AGENTS.md`

* Rolle des Agenten
* Arbeitsdisziplin
* Verbote & Leitplanken
* Gate-Logik (keine Optimierung ohne Freigabe)
* Sicherheitsgrundsätze (hoch-level)

➡️ Das ist im Prinzip deine **minimale operative `instructions.md`**.

### ❌ Gehört **nicht** hinein

* Projektziele
* Tech-Stacks
* Templates
* Activation Manifests
* plan.md-Inhalte

Diese gehören **immer ins Projekt**.

---

## 4) Wie bindest du deine `llm-rules` dort ein?

Du hast **drei realistische Optionen**.
Ich bewerte sie direkt.

---

### Option A – Inhalt direkt kopieren (nicht empfohlen)

```md
# ~/.pi/agent/AGENTS.md
<kopierter Inhalt aus llm-rules/standard/...>
```

❌ Nachteile:

* Duplizierung
* Drift
* Pflegeaufwand

---

### Option B – Symlink (empfohlen, wenn lokal)

**Variante: Softlink**

```bash
ln -s ~/repos/llm-rules/standard/global.md ~/.pi/agent/AGENTS.md
```

oder **Hardlink** (nur wenn gleiches Filesystem):

```bash
ln ~/repos/llm-rules/standard/global.md ~/.pi/agent/AGENTS.md
```

✅ Vorteile:

* Single Source of Truth
* Kein Copy/Paste
* Änderungen sofort wirksam

⚠️ Voraussetzung:

* `llm-rules` liegt **lokal** und stabil
* Pfad ändert sich selten

👉 **Das ist für dich wahrscheinlich die beste Lösung.**

---

### Option C – Generierung per Script (für später)

* `~/.pi/agent/AGENTS.md` wird **generiert**
* aus `llm-rules/standard/*`
* z. B. bei Login oder vor pi-Start

✅ Vorteile:

* maximal kontrolliert
* skalierbar

❌ Overkill **für jetzt**

---

## 5) Konkrete Empfehlung für dein Setup

Basierend auf allem, was du beschrieben hast:

### ✅ Mach es so:

1. Wähle **eine** Datei aus deinen `llm-rules`, z. B.:

   ```text
   llm-rules/standard/agent-baseline.md
   ```

2. Diese enthält:

   * Rolle
   * Arbeitsdisziplin
   * Gate-Logik
   * Abbruchregeln

3. Lege einen **Symlink**:

   ```bash
   ln -s ~/path/to/llm-rules/standard/agent-baseline.md ~/.pi/agent/AGENTS.md
   ```

4. In jedem Projekt:

   * eigenes `AGENTS.md` **oder**
   * generiertes `AGENTS.generated.md` (aus Activation Manifest)

Damit hast du:

* globale Konsistenz
* projekt-spezifische Flexibilität
* kein Copy/Paste
* ein Tool (pi)

---

## 6) Woran erkennst du, dass es korrekt funktioniert?

Starte pi in **irgendeinem** Projekt und prüfe:

* Befolgt pi die Arbeitsdisziplin **ohne dass du sie erwähnst**?
* Verweist pi auf `plan.md`, wenn vorhanden?
* Vermeidet pi Optimierung ohne Gate?

Wenn ja → **globale Baseline greift**.

---

Wenn du willst, können wir als nächsten Schritt:

* die **exakte Struktur der globalen Baseline-Datei** festziehen, oder
* klären, **wie Projekt-AGENTS.md minimal aussehen sollte**, damit es sauber mit der globalen Baseline zusammenspielt.

Siehe: chatgpt llm vorgabe zielbild
