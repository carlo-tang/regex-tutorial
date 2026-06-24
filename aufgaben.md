# Regex Tutorial – Ablauf

**Plattform:** regex101.com · Flavor: PCRE · Flags: `g` `m`

---

## Phase 0: Zeichentypen

**Link:** [TODO] ← alle öffnen diesen Link

**Test-String:**
```
abcdefghijklmnopqrstuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
0123456789
! @ # $ % & * ( ) - + = . , / ?
```

---

### Gemeinsam 0 – Was matcht was?

Tippt gemeinsam nacheinander:

1. `[a-z]` → Welche Zeile wird komplett markiert?
2. `[A-Z]` → Und jetzt?
3. `[0-9]` → Und jetzt?
4. `[^a-zA-Z0-9 ]` → Was sind die Treffer in der letzten Zeile?

**Erkenntnis:** Zeichenklassen `[ ]` definieren erlaubte Zeichen. `^` inside `[ ]` bedeutet "alles außer".

---

### Aufgabe 0

> Schreibe einen Regex, der alle Buchstaben **und** Ziffern matcht — aber keine Sonderzeichen.

*Tipp: Kombiniert `[a-z]`, `[A-Z]` und `[0-9]` in einer Klammer*

---

## Phase 1: Grundlagen – "Hallo, Welt!"

**Link:** https://regex101.com/r/QEd4Fj/1 ← alle öffnen diesen Link

**Test-String:**
```
Hallo, Welt!
Hallo Welt
hallo, Welt!
HALLO, WELT!
Hallo, Welt
Hallo, welt!
```

---

### Gemeinsam 1 – Literal & Groß-/Kleinschreibung

Tippt gemeinsam nacheinander:

1. `Hallo` → Wie viele Treffer? Welche Zeilen fehlen?
2. `hallo` → Was ändert sich?
3. `HALLO` → Was passiert?

**Erkenntnis:** Regex unterscheidet Groß- und Kleinschreibung.

---

### Aufgabe 1

> Schreibe einen Regex, der sowohl `Hallo` als auch `hallo` findet — aber nicht `HALLO`.

*Tipp: Nutzt `[ ]`*

---

### Gemeinsam 2 – Punkt als Wildcard & Anker

Tippt gemeinsam nacheinander:

1. `Hallo.Welt` → Warum matcht auch "Hallo Welt" (mit Leerzeichen)?
2. `^Hallo` → Was bewirkt das `^`?

**Erkenntnis:** `.` = beliebiges Zeichen. `^` = Zeilenanfang.

---

### Aufgabe 2

> Schreibe einen Regex, der nur Zeilen findet, die mit `!` **enden**.

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

*Test-String: gleicher wie oben (Phase 2)*

> Das Muster `\d+\.\d+\.\d+\.\d+` matcht noch `256.1.1.1` und `192.168.1.999`.  
> Schreibe einen Regex, der pro Oktett **maximal 3 Ziffern** erlaubt.

*Tipp: Nutzt `{ }`*

---

## Bonus (wenn Zeit bleibt)

**Link:** https://regex101.com/r/lO9tf0/1 ← IP-Test-String von oben

### Bonus – Private IP-Bereiche

> Schreibe einen Regex, der **nur** IPs aus privaten Netzen findet:  
> 10.x.x.x · 172.16.x.x · 192.168.x.x
