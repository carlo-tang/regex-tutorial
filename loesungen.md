# Regex Tutorial – Lösungen & Moderationshinweise

---

## Phase 1: Grundlagen – "Hallo, Welt!"

**Test-String:**
```
Hallo, Welt!
Hallo Welt
hallo, Welt!
HALLO, WELT!
Hallo, Welt
Hallo, welt!
```

**Link:** https://regex101.com/r/QEd4Fj/1

---

### Gemeinsam 1 – Literal & Groß-/Kleinschreibung

| Regex | Treffer | Hinweis |
|---|---|---|
| `Hallo` | 4 Zeilen | "hallo" und "HALLO" fehlen |
| `hallo` | 1 Zeile | nur Zeile 3 |
| `HALLO` | 1 Zeile | nur Zeile 4 |

**Moderationshinweis:** Kurz erklären, dass Regex keine Sprachkenntnis hat — es vergleicht Zeichen für Zeichen, exakt wie geschrieben.

---

### Aufgabe 1 – Lösung

**Regex:** `[Hh]allo`  
**Treffer:** 5 Zeilen (alle außer "HALLO, WELT!")  
**Erklärung:** `[Hh]` = Zeichenklasse — matcht genau ein Zeichen, entweder "H" oder "h". `[abc]` listet alle erlaubten Zeichen auf.

---

### Gemeinsam 2 – Punkt als Wildcard & Anker

| Regex | Treffer | Hinweis |
|---|---|---|
| `Hallo.Welt` | 3 Zeilen | Punkt matcht "," und " " |
| `^Hallo` | 4 Zeilen | Zeilen, die mit "Hallo" beginnen |

**Moderationshinweis:** Den Überraschungsmoment nutzen — "Hallo Welt" (mit Leerzeichen) matcht, weil `.` jedes Zeichen ist, auch ein Leerzeichen. Das ist der häufigste Anfängerfehler in der Praxis.

---

### Aufgabe 2 – Lösung

**Regex:** `!$`  
**Treffer:** 2 Zeilen ("Hallo, Welt!" und "Hallo, welt!")  
**Erklärung:** `$` = Zeilenende. Kombiniert: "ein Ausrufezeichen direkt am Zeilenende".  
**Alternative:** `Welt!$` — präziser, nur Zeilen die auf "Welt!" enden.

---

## Phase 2: IP-Adressen

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

**Link:** https://regex101.com/r/lO9tf0/1

---

### Gemeinsam 3 – Ziffern & die Wildcard-Falle

| Regex | Besonderheit |
|---|---|
| `\d+` | Alle Ziffernblöcke einzeln markiert |
| `\d+.\d+.\d+.\d+` | `192_168_1_1` matcht! Punkt = Wildcard |
| `\d+\.\d+\.\d+\.\d+` | `192_168_1_1` matcht nicht mehr |

**Moderationshinweis:** Die Wildcard-Falle ist der didaktische Kern. Warten bis jemand "192_168_1_1" im Test-String bemerkt, dann erklären: "Genau das passiert in der Praxis — man glaubt, nach IPs zu suchen, matcht aber auch Nicht-IPs."

---

### Aufgabe 3 – Lösung

*Test-String: gleicher wie oben (Phase 2)*

**Regex:** `\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}`  
**Erklärung:** `{1,3}` = mindestens 1, maximal 3 Ziffern pro Oktett.  
**Wichtiger Hinweis:** `256.1.1.1` und `192.168.1.999` matchen trotzdem — Regex prüft nur Struktur (Format), nicht ob der Wert zwischen 0 und 255 liegt. Das kann Regex grundsätzlich nicht leisten.

---

## Bonus – Private IP-Bereiche

**Regex:** `(10|172\.16|192\.168)\.\d{1,3}\.\d{1,3}\.\d{1,3}`  
**Treffer:** `10.0.0.1`, `172.16.5.100`, `192.168.1.1`, `192.168.1.999`, `192.168.1.1 ist der Router`  
**Erklärung:** `(10|172\.16|192\.168)` = Gruppe mit ODER-Operator. Die Klammern sind wichtig — ohne sie würde `|` den ganzen Ausdruck trennen, nicht nur den Anfang.
