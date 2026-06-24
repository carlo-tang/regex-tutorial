# Regex Tutorial – Moderationsvorlage

**Plattform:** regex101.com · Flavor: PCRE · Flags: `g` `m`

---

## Phase 0: Zeichentypen

**Link an Kollegen schicken:** [TODO]

**Test-String:**
```
abcdefghijklmnopqrstuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
0123456789
! @ # % & - _ = , ; : " ' < > /
. * + ? ^ $ { } [ ] ( ) | \
```

*Zeile 4: Sonderzeichen ohne Escape · Zeile 5: Metazeichen — brauchen immer `\`*

---

### Gemeinsam 0 – Was matcht was?

**Sag:** *"Tippt nacheinander mit mir:"*

1. `[a-z]` → *"Welche Zeile wird komplett markiert?"*
2. `[A-Z]` → *"Und jetzt?"*
3. `[0-9]` → *"Und jetzt?"*
4. `.` → *"Was passiert? Warum matcht der Punkt plötzlich alles?"*
5. `\.` → *"Was ändert sich jetzt?"*

| Regex | Treffer | Hinweis |
|---|---|---|
| `[a-z]` | Zeile 1 komplett | |
| `[A-Z]` | Zeile 2 komplett | |
| `[0-9]` | Zeile 3 komplett | |
| `.` | Alles — jedes Zeichen auf jeder Zeile | Punkt = Wildcard, Überraschungsmoment! |
| `\.` | Nur der `.` in Zeile 5 | Backslash macht aus dem Metazeichen ein Literal |

**Erkläre:** Zeile 4 = Zeichen die direkt getippt werden können. Zeile 5 = Metazeichen — in Regex haben sie eine Sonderbedeutung, deshalb brauchen sie `\` davor wenn man das Zeichen selbst meint.

---

### Aufgabe 0

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der nur das ? in Zeile 5 matcht — nicht alle anderen Zeichen.
Tipp: Nutzt \
```

**Lösung:** `\?`  
**Treffer:** Nur das `?` in Zeile 5.  
**Erkläre:** `?` ist ein Metazeichen (bedeutet "0 oder 1 mal"). Mit `\?` wird es zum wörtlichen Fragezeichen. Gleiches Prinzip für alle Zeichen aus Zeile 5.

---

## Phase 1: Grundlagen – "Hallo, Welt!"

**Link an Kollegen schicken:** https://regex101.com/r/QEd4Fj/1

---

### Gemeinsam 1 – Literal & Groß-/Kleinschreibung

**Sag:** *"Tippt nacheinander mit mir:"*

1. `Hallo` → *"Wie viele Zeilen werden markiert? Welche fehlen?"*
2. `hallo` → *"Was ändert sich?"*
3. `HALLO` → *"Was passiert?"*

| Regex | Treffer | Hinweis |
|---|---|---|
| `Hallo` | 4 Zeilen | "hallo" und "HALLO" fehlen |
| `hallo` | 1 Zeile | nur Zeile 3 |
| `HALLO` | 1 Zeile | nur Zeile 4 |

**Erkläre:** Regex hat keine Sprachkenntnis — es vergleicht Zeichen für Zeichen, exakt wie geschrieben.

---

### Aufgabe 1

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der sowohl Hallo als auch hallo findet — aber nicht HALLO.
Tipp: Nutzt [ ]
```

**Lösung:** `[Hh]allo`  
**Treffer:** 5 Zeilen (alle außer "HALLO, WELT!")  
**Erkläre:** `[Hh]` = Zeichenklasse — matcht genau ein Zeichen, entweder "H" oder "h". Alles in eckigen Klammern ist eine Auswahl von einem Zeichen.

---

### Gemeinsam 2 – Punkt als Wildcard & Anker

**Sag:** *"Tippt nacheinander mit mir:"*

1. `Hallo.Welt` → *"Warum matcht auch 'Hallo Welt' mit Leerzeichen?"*
2. `^Hallo` → *"Was bewirkt das `^`?"*

| Regex | Treffer | Hinweis |
|---|---|---|
| `Hallo.Welt` | 3 Zeilen | Punkt matcht "," und " " |
| `^Hallo` | 4 Zeilen | Zeilen, die mit "Hallo" beginnen |

**Erkläre:** `.` = beliebiges Zeichen — auch Leerzeichen und Kommas. Das ist der häufigste Anfängerfehler in der Praxis. `^` = Zeilenanfang.

---

### Aufgabe 2

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der nur Zeilen findet, die mit ! enden.
Tipp: Nutzt $
```

**Lösung:** `!$`  
**Treffer:** 2 Zeilen ("Hallo, Welt!" und "Hallo, welt!")  
**Erkläre:** `$` = Zeilenende. Falls niemand draufkommt: *"Es gibt ein Gegenstück zu `^`..."*  
**Alternative:** `Welt!$` — präziser, nur Zeilen die auf "Welt!" enden.

---

## Phase 2: IP-Adressen

**Link an Kollegen schicken:** https://regex101.com/r/lO9tf0/1

---

### Gemeinsam 3 – Ziffern & die Wildcard-Falle

**Sag:** *"Tippt nacheinander mit mir:"*

1. `\d+` → *"Was wird markiert? Was bedeutet `\d`, was bedeutet `+`?"*
2. `\d+.\d+.\d+.\d+` → *"Welcher Eintrag matcht überraschend? Warum?"*
3. `\d+\.\d+\.\d+\.\d+` → *"Was ändert sich? Was macht der Backslash?"*

| Regex | Besonderheit |
|---|---|
| `\d+` | Alle Ziffernblöcke einzeln markiert |
| `\d+.\d+.\d+.\d+` | `192_168_1_1` matcht! Punkt = Wildcard |
| `\d+\.\d+\.\d+\.\d+` | `192_168_1_1` matcht nicht mehr |

**Erkläre:** `\d` = Ziffer. `+` = ein oder mehr. `\.` = wörtlicher Punkt. Warten bis jemand `192_168_1_1` bemerkt — dann: *"Genau das passiert in der Praxis: man glaubt nach IPs zu suchen, matcht aber auch Nicht-IPs."*

---

### Aufgabe 3

**Frage (kopieren & in Teams einfügen):**
```
Das Muster \d+\.\d+\.\d+\.\d+ matcht noch 256.1.1.1 und 192.168.1.999.
Schreibt einen Regex, der pro Oktett maximal 3 Ziffern erlaubt.
Tipp: Nutzt { }
```

**Lösung:** `\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}`  
**Erkläre:** `{1,3}` = mindestens 1, maximal 3 Ziffern.  
**Wichtig:** `256.1.1.1` und `192.168.1.999` matchen trotzdem — Regex prüft nur Struktur, nicht ob der Wert ≤ 255 ist. Das kann Regex grundsätzlich nicht leisten.

---

## Bonus (wenn Zeit bleibt)

**Link:** https://regex101.com/r/lO9tf0/1 (gleicher wie Phase 2)

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der nur IPs aus privaten Netzen findet:
10.x.x.x, 172.16.x.x oder 192.168.x.x
Tipp: Nutzt ( ) und |
```

**Lösung:** `(10|172\.16|192\.168)\.\d{1,3}\.\d{1,3}\.\d{1,3}`  
**Treffer:** `10.0.0.1`, `172.16.5.100`, `192.168.1.1`, `192.168.1.999`, `192.168.1.1 ist der Router`  
**Erkläre:** `|` = ODER. Die Klammern fassen die Alternativen zusammen — ohne sie würde `|` den ganzen Ausdruck trennen.
