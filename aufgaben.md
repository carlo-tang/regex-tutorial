# Regex Tutorial – Ablauf

**Plattform:** regex101.com · Flavor: PCRE2 (PHP) · Flags: `g` `m`

**Hinweis:** Wir nutzen den **Global-Modus** (`g`) — der Regex markiert *alle* Treffer im Text, nicht nur den ersten.

---

## Phase 0: Zeichentypen

**Link:** [TODO] ← alle öffnen diesen Link

**Test-String:**
```
abcdefghijklmnopqrstuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
0123456789
! @ # % & - _ = , ; : " ' < >
. * + ? ^ $ { } [ ] ( ) | \ /
122333444455555
aBBcccDDDDeeeee
```

*Zeile 4: Sonderzeichen ohne Escape · Zeile 5: Metazeichen — brauchen immer `\` · Zeilen 6+7: für Quantifizierer*

---

### Gemeinsam 0 – Was matcht was?

Tippt gemeinsam nacheinander:

1. `[a-z]` → Welche Zeile wird komplett markiert?
2. `[A-Z]` → Und jetzt?
3. `[0-9]` → Und jetzt?
4. `[a-Z]` → Was passiert jetzt?
5. `[A-z]` → Was matcht hier unerwartet?
6. `[0-z]` → Was matcht jetzt alles?
7. `[0-9A-Za-z]` → Wie schreibt man es richtig?
8. *(live)* — gemischte Zeichenklasse mit Buchstaben, Ziffern und Sonderzeichen
9. `.` → Was passiert? Warum matcht der Punkt plötzlich alles?
10. `\.` → Was ändert sich jetzt?

**Erkenntnis:** Zeichen-Bereiche folgen der ASCII-Tabelle, nicht dem Alphabet. Zeile 4 = Zeichen ohne Escape. Zeile 5 = Metazeichen, die `\` brauchen.

---

### Aufgabe 0

> Schreibt einen Regex, der nur das `?` in Zeile 5 matcht — nicht alle anderen Zeichen.

*Tipp: Nutzt `\`*

---

### Gemeinsam 0b – Quantifizierer

Tippt gemeinsam nacheinander — Fokus auf Zeilen 6 und 7:

1. `\d` → Wie viele Treffer? Jede Ziffer einzeln.
2. `\d+` → Was ändert sich? Warum ein Treffer statt vielen?
3. `\d{3}` → Was matcht jetzt genau? Zählt die Gruppen.
4. `\d{2,4}` → Was passiert bei 2 bis 4 Ziffern?
5. `[A-Z]+` → Was matcht in Zeile 7? Warum nicht das `a` am Anfang?
6. `[a-z]+` → Und jetzt? Wie viele Treffer, welche?

**Übersicht Quantifizierer:**

| Symbol | Bedeutung |
|---|---|
| `+` | 1 oder mehr |
| `*` | 0 oder mehr |
| `?` | 0 oder 1 (optional) |
| `{n}` | genau n mal |
| `{n,}` | mindestens n mal |
| `{n,m}` | zwischen n und m mal |

---

### Aufgabe 0b

> Schreibt einen Regex, der auf Zeile 7 (`aBBcccDDDDeeeee`) nur die Großbuchstaben-Blöcke matcht — aber mindestens 2 Großbuchstaben am Stück.

*Tipp: Nutzt `[A-Z]` und `{ }`*

---

## Phase 1: Grundlagen – "Hallo, Welt!"

**Link:** https://regex101.com/r/QEd4Fj/2 ← alle öffnen diesen Link

**Test-String:**
```
Hallo, Welt!
Hallo Welt
Welt Hallo
hallo, Welt!
HALLO, WELT!
Hallo, Welt
Hallo, welt!
```

---

### Gemeinsam 1 – Literal & Groß-/Kleinschreibung

Tippt gemeinsam nacheinander:

1. `Hallo` → Wie viele Zeilen werden markiert? Welche fehlen?
2. `hallo` → Was ändert sich?
3. `HALLO` → Was passiert?

**Erkenntnis:** Regex unterscheidet Groß- und Kleinschreibung.

---

### Aufgabe 1

> Schreibt einen Regex, der sowohl `Hallo` als auch `hallo` findet — aber nicht `HALLO`.

*Tipp: Nutzt `[ ]`*

---

### Gemeinsam 2 – Punkt als Wildcard & Anker

Tippt gemeinsam nacheinander:

1. `Hallo.Welt` → Wie viele Zeilen matchen? Warum nicht mehr?
2. `^Hallo` → Was bewirkt das `^`?

**Erkenntnis:** `.` = genau 1 beliebiges Zeichen. `^` = Zeilenanfang.

---

### Aufgabe 2

> Schreibt einen Regex, der nur Zeilen findet, die mit `Hallo` **enden**.

*Tipp: Nutzt `$`*

---

## Phase 2: IP-Adressen

**Link:** https://regex101.com/r/lO9tf0/1 ← alle öffnen diesen Link

**Test-String:**
```
192.168.1.1
10.0.0.1
172.16.5.100
8.8.8.8
256.1.1.1
192.168.1.999
192_168_1_1
Kein IP: nur Text
192.168.1.1 ist der Router
Server: 10.0.0.254
```

---

### Gemeinsam 3 – Ziffern & die Wildcard-Falle

Tippt gemeinsam nacheinander:

1. `\d+` → Was wird markiert? Was bedeutet `\d`, was bedeutet `+`?
2. `\d+.\d+.\d+.\d+` → Welcher Eintrag matcht überraschend? Warum?
3. `\d+\.\d+\.\d+\.\d+` → Was ändert sich? Was macht der Backslash?

**Erkenntnis:** `\d` = Ziffer. `+` = ein oder mehr. `\.` = wörtlicher Punkt (kein Wildcard).

---

### Aufgabe 3

> Das Muster `\d+\.\d+\.\d+\.\d+` matcht noch `256.1.1.1` und `192.168.1.999`.  
> Schreibt einen Regex, der pro Oktett **maximal 3 Ziffern** erlaubt.

*Tipp: Nutzt `{ }`*

---

## Bonus (wenn Zeit bleibt)

**Link:** https://regex101.com/r/lO9tf0/1 ← IP-Test-String von oben

### Bonus – Private IP-Bereiche

> Schreibt einen Regex, der **nur** IPs aus privaten Netzen findet:  
> 10.x.x.x · 172.16.x.x · 192.168.x.x

*Tipp: Nutzt `( )` und `|`*
