# Regex Tutorial – Moderationsvorlage

**Plattform:** regex101.com · Flavor: PCRE2 (PHP) · Flags: `g` `m`

**Hinweis zu den Flags:** Wir nutzen den **Global-Modus** (`g`). Das bedeutet: der Regex sucht nicht nur den ersten Treffer und hört auf, sondern läuft den gesamten Text durch und markiert *alle* Treffer. Ohne `g` würde z.B. `[a-z]` nur den ersten Buchstaben `a` markieren. Wenn die Kollegen fragen warum alles auf einmal markiert wird — das ist der Grund.

---

## Phase 0: Zeichentypen

**Link an Kollegen schicken:** [TODO]

**Erkläre zuerst die regex101-Oberfläche:**  
Mitte: "Regular Expression" (oben) und "Test String" (unten) — ergibt sich von selbst.  
Rechte Spalte zeigen und erklären:
- **Explanation** — erklärt jeden Teil des Regex in Klartext
- **Match information** — listet alle Treffer mit Position und Inhalt
- **Quick reference** — Spickzettel mit allen Regex-Symbolen (praktisch zum Nachschlagen)

*Tipp: Explanation ist besonders nützlich — wenn jemand nicht versteht warum ein Regex matcht, einfach dort nachschauen.*

---

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

**Sag:** *"Tippt nacheinander mit mir:"*

1. `[a-z]` → *"Welche Zeile wird komplett markiert?"*
2. `[A-Z]` → *"Und jetzt?"*
3. `[0-9]` → *"Und jetzt?"*
4. `[a-Z]` → *"Was passiert jetzt?"*
5. `[A-z]` → *"Und jetzt? Was matcht hier unerwartet?"*
6. `[0-z]` → *"Was matcht jetzt alles?"*
7. `[0-9A-Za-z]` → *"Wie schreibt man es richtig?"*
8. *(live eintippen, kein festes Beispiel)* — gemischte Zeichenklasse mit Buchstaben, Ziffern, Sonderzeichen und Escaping, z.B. `[db4sf94\*\+!_-]` → *"Was braucht `\`, was nicht?"*
9. `.` → *"Was passiert? Warum matcht der Punkt plötzlich alles?"*
10. `\.` → *"Was ändert sich jetzt?"*

| Regex | Treffer | Hinweis |
|---|---|---|
| `[a-z]` | Zeile 1 komplett | |
| `[A-Z]` | Zeile 2 komplett | |
| `[0-9]` | Zeile 3 komplett | |
| `[a-Z]` | Fehlermeldung | `a` (97) liegt in ASCII nach `Z` (90) — ungültiger Bereich |
| `[A-z]` | Zeilen 1+2 + `^` `[` `]` `\` `_` | ASCII 65–122: zwischen Z und a liegen noch 6 Sonderzeichen |
| `[0-z]` | Zeilen 1+2+3 + viele Sonderzeichen | ASCII 48–122: `:` `;` `<` `>` `?` `@` stecken auch drin |
| `[0-9A-Za-z]` | Zeilen 1+2+3 komplett, nichts aus 4+5 | Explizit kombiniert — keine ASCII-Überraschungen |
| `[db4sf94\*\+!_-]` | *(live)* einzelne Buchstaben/Ziffern + `*` `+` `!` `_` `-` | Metazeichen in `[ ]` brauchen `\`, harmlose Zeichen nicht |
| `.` | Alles — jedes Zeichen auf jeder Zeile | Punkt = Wildcard, Überraschungsmoment! |
| `\.` | Nur der `.` in Zeile 5 | Backslash macht aus dem Metazeichen ein Literal |

**Erkläre (ASCII):** Regex-Bereiche wie `[A-z]` folgen der ASCII-Tabelle, nicht dem Alphabet. Zwischen `Z` (90) und `a` (97) liegen `[`, `\`, `]`, `^`, `_`, `` ` `` — die werden ungewollt mitgematcht. Deshalb immer explizit kombinieren: `[A-Za-z]`.

**ASCII-Tabelle zeigen:** [theasciicode.com.ar](https://theasciicode.com.ar/) — Positionen von Buchstaben, Ziffern und Sonderzeichen aufzeigen.

**Erkläre (Metazeichen):** Zeile 4 = Zeichen die direkt getippt werden können. Zeile 5 = Metazeichen — in Regex haben sie eine Sonderbedeutung, deshalb brauchen sie `\` davor wenn man das Zeichen selbst meint.

---

### Aufgabe 0

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der nur das ? in Zeile 5 matcht — nicht alle anderen Zeichen.
Tipp: Nutzt \
```

**Lösung:**
```
\?
```
**Treffer:** Nur das `?` in Zeile 5.  
**Erkläre:** `?` ist ein Metazeichen (bedeutet "0 oder 1 mal"). Mit `\?` wird es zum wörtlichen Fragezeichen. Gleiches Prinzip für alle Zeichen aus Zeile 5.

---

### Gemeinsam 0b – Quantifizierer

**Sag:** *"Tippt nacheinander mit mir — wir schauen auf Zeilen 6 und 7:"*

1. `\d` → *"Wie viele Treffer? Jede Ziffer einzeln."*
2. `\d+` → *"Was ändert sich? Warum ein Treffer statt vielen?"*
3. `\d{3}` → *"Was matcht jetzt genau? Zählt die Gruppen."*
4. `\d{2,4}` → *"Was passiert bei 2 bis 4 Ziffern?"*
5. `[A-Z]+` → *"Was matcht in Zeile 7? Warum nicht das 'a' am Anfang?"*
6. `[a-z]+` → *"Und jetzt? Wie viele Treffer, welche?"*

| Regex | Treffer auf Zeile 6+7 | Hinweis |
|---|---|---|
| `\d` | 15 Einzelziffern | ein Match pro Zeichen |
| `\d+` | `122333444455555` (1 Treffer) | gierig — nimmt so viele wie möglich |
| `\d{3}` | `122` `333` `444` `455` `555` | genau 3, Rest wird neu angefangen |
| `\d{2,4}` | `1223` `3344` `4455` `555` | gierig: nimmt 4 wenn möglich, sonst weniger |
| `[A-Z]+` | `BB` `DDDD` | nur Großbuchstaben-Blöcke |
| `[a-z]+` | `a` `ccc` `eeeee` | nur Kleinbuchstaben-Blöcke |

**Übersicht Quantifizierer:**

| Symbol | Bedeutung |
|---|---|
| `+` | 1 oder mehr |
| `*` | 0 oder mehr |
| `?` | 0 oder 1 (optional) |
| `{n}` | genau n mal |
| `{n,}` | mindestens n mal |
| `{n,m}` | zwischen n und m mal |

**Erkläre:** Quantifizierer sind immer **gierig** — sie nehmen so viele Zeichen wie möglich. `\d+` auf `122333` gibt einen einzigen Treffer, nicht sechs.

---

### Aufgabe 0b

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der auf Zeile 7 (aBBcccDDDDeeeee) nur die
Großbuchstaben-Blöcke matcht — aber genau 2 oder mehr Großbuchstaben.
Tipp: Nutzt [A-Z] und { }
```

**Lösung:**
```
[A-Z]{2,}
```
**Treffer:** `BB` und `DDDD`  
**Erkläre:** `{2,}` = mindestens 2 mal. `[A-Z]{1,}` wäre dasselbe wie `[A-Z]+`.

---

## Phase 1: Grundlagen – "Hallo, Welt!"

**Link an Kollegen schicken:** https://regex101.com/r/QEd4Fj/2

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

**Sag:** *"Tippt nacheinander mit mir:"*

1. `Hallo` → *"Wie viele Zeilen werden markiert? Welche fehlen?"*
2. `hallo` → *"Was ändert sich?"*
3. `HALLO` → *"Was passiert?"*

| Regex | Treffer | Hinweis |
|---|---|---|
| `Hallo` | 5 Zeilen | "hallo", "HALLO" fehlen — auch "Welt Hallo" matcht! |
| `hallo` | 1 Zeile | nur "hallo, Welt!" |
| `HALLO` | 1 Zeile | nur "HALLO, WELT!" |

**Erkläre:** Regex hat keine Sprachkenntnis — es vergleicht Zeichen für Zeichen, exakt wie geschrieben.

---

### Aufgabe 1

**Frage (kopieren & in Teams einfügen):**
```
Schreibt einen Regex, der sowohl Hallo als auch hallo findet — aber nicht HALLO.
Tipp: Nutzt [ ]
```

**Lösung:**
```
[Hh]allo
```
**Treffer:** 6 Zeilen (alle außer "HALLO, WELT!")  
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
Schreibt einen Regex, der nur Zeilen findet, die mit Hallo enden.
Tipp: Nutzt $
```

**Lösung:**
```
Hallo$
```
**Treffer:** 1 Zeile ("Welt Hallo")  
**Erkläre:** `$` = Zeilenende. Das Gegenstück zu `^`. `Hallo$` matcht nur wenn `Hallo` am Ende der Zeile steht — die anderen Zeilen enthalten `Hallo` auch, aber nicht am Ende.

---

## Phase 2: IP-Adressen

**Link an Kollegen schicken:** https://regex101.com/r/lO9tf0/1

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

**Lösung:**
```
\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}
```
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

**Lösung:**
```
(10\.\d{1,3}\.\d{1,3}\.\d{1,3}|172\.16\.\d{1,3}\.\d{1,3}|192\.168\.\d{1,3}\.\d{1,3})
```
**Treffer:** `10.0.0.1`, `172.16.5.100`, `192.168.1.1`, `192.168.1.999`, `192.168.1.1 ist der Router`, `10.0.0.254`  
**Erkläre:** `|` = ODER. Jede Alternative ist ein vollständiges Muster — `10` braucht noch 3 Oktette, `172.16` und `192.168` nur noch 2. Deshalb muss jede Alternative für sich ausgeschrieben werden.  
**Wichtig — auf `192.168.1.999` hinweisen:** *"Diese IP matcht — obwohl 999 kein gültiges Oktett ist. Warum?"* → `\d{1,3}` prüft nur: 1 bis 3 Ziffern. Ob der Wert zwischen 0 und 255 liegt, kann Regex grundsätzlich nicht prüfen. Regex erkennt **Format**, nicht **Bedeutung**.

---

## Exkurs: Regex für Validierung (nicht nur Matching)

**Sag:** *"Regex kann auch zur Validierung eingesetzt werden — mit einem Unterschied:"*

- **Matching** (ohne Anker): Muster wird *irgendwo* im Text gesucht
- **Validierung** (mit `^` und `$`): Der *gesamte* String muss dem Muster entsprechen

**Aber:** Regex validiert nur **Struktur**, nie **Wertebereich**. `\d{1,3}` akzeptiert 999 genauso wie 1.

*"Wie würde ein Regex aussehen, der wirklich nur Werte von 0 bis 255 erlaubt?"*

**Zeige** (nicht tippen lassen — nur anschauen):

```
^(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}$
```

| Teil | Matcht |
|---|---|
| `25[0-5]` | 250–255 |
| `2[0-4]\d` | 200–249 |
| `1\d\d` | 100–199 |
| `[1-9]?\d` | 0–99 |
| `(...){3}` | dasselbe Muster noch 3× mit Punkt davor |
| `^` und `$` | gesamte Zeile muss passen — kein Prefix/Suffix erlaubt |

**Fazit:** Für echte IP-Validierung wird Regex schnell komplex. In der Praxis: Regex prüft das Format, die Programmiersprache prüft den Wertebereich. Regex ist ein Werkzeug — kein Allheilmittel.
