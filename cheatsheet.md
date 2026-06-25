# Regex Cheatsheet – Netzwerk Edition

**Flavor:** PCRE2 (PHP) auf regex101.com · Flags: `g` `m`

---

## Metazeichen

| Zeichen | Bedeutung | Beispiel |
|---|---|---|
| `.` | Beliebiges Zeichen (außer Zeilenumbruch) | `1.1` matcht "1.1", "1X1", "1 1" |
| `\.` | Wörtlicher Punkt (escaped) | `1\.1` matcht nur "1.1" |
| `^` | Zeilenanfang | `^192` matcht Zeilen, die mit 192 beginnen |
| `$` | Zeilenende | `\.log$` matcht Zeilen, die auf ".log" enden |
| `\` | Escaping — nimmt Metazeichen ihre Sonderbedeutung | `\*` matcht ein wörtliches Sternchen |

---

## Quantifizierer

| Zeichen | Bedeutung | Beispiel |
|---|---|---|
| `+` | 1 oder mehr | `\d+` matcht "1", "42", "999" |
| `*` | 0 oder mehr | `\d*` matcht auch leere Stellen |
| `?` | 0 oder 1 (optional) | `https?` matcht "http" und "https" |
| `{n}` | Genau n-mal | `\d{3}` matcht genau 3 Ziffern |
| `{n,m}` | Mindestens n, höchstens m-mal | `\d{1,3}` matcht 1–3 Ziffern |
| `{n,}` | Mindestens n-mal | `\d{2,}` matcht 2 oder mehr Ziffern |

---

## Zeichenklassen

| Ausdruck | Bedeutung | Entspricht |
|---|---|---|
| `[abc]` | Genau eines von: a, b oder c | — |
| `[a-z]` | Alle Kleinbuchstaben | — |
| `[A-Z]` | Alle Großbuchstaben | — |
| `[0-9]` | Alle Ziffern | `\d` |
| `[a-zA-Z0-9]` | Buchstaben und Ziffern | `\w` (+ Unterstrich) |
| `[^abc]` | Alles außer a, b, c (Negation) | — |
| `\d` | Ziffer | `[0-9]` |
| `\w` | Wortzeichen (Buchstabe, Ziffer, Unterstrich) | `[a-zA-Z0-9_]` |
| `\s` | Whitespace (Leerzeichen, Tab, ...) | — |
| `\D` | Keine Ziffer | `[^0-9]` |

> **Achtung:** `\w` enthält den Unterstrich `_` — für Hostnamen daher `[a-zA-Z0-9-]` verwenden.

---

## Gruppen und Alternation

| Ausdruck | Bedeutung | Beispiel |
|---|---|---|
| `(abc)` | Gruppe | `(tcp\|udp)` matcht "tcp" oder "udp" |
| `\|` | ODER (Alternation) | `ssh\|telnet\|ftp` |
| `(a\|b)+` | Gruppe mit Quantifizierer | — |

---

## Netzwerk-Beispiele

### IPv4-Adresse (Struktur, ohne Wertebereich-Prüfung)
```
\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}
```

### Nur private IP-Adressen (RFC 1918)
```
(10\.\d{1,3}\.\d{1,3}\.\d{1,3}|172\.16\.\d{1,3}\.\d{1,3}|192\.168\.\d{1,3}\.\d{1,3})
```

### Hostname / FQDN
```
[a-zA-Z0-9-]+(\.[a-zA-Z0-9-]+)+
```

### MAC-Adresse (mit Doppelpunkt oder Bindestrich)
```
([0-9a-fA-F]{2}[:\-]){5}[0-9a-fA-F]{2}
```

### Port-Nummer (1–65535, nur Struktur)
```
[1-9]\d{0,4}
```

### Nur bestimmte Ports
```
(22|80|443|8080)
```

### Log-Zeile mit Quell-IP und Ziel-Port
```
SRC=(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}).*DPT=(22|80|443)
```

---

## Wichtige Hinweise

**Regex prüft Struktur, nicht Semantik.**  
`256.1.1.1` matcht ein IP-Regex — ob der Wert gültig ist, muss Code prüfen.

**PCRE vs. grep (Linux-Kommandozeile)**  
Auf regex101 wird PCRE verwendet. In der Kommandozeile:
- `grep 'Muster'` → POSIX BRE (kein `\d`, kein `+` ohne `\`)
- `grep -E 'Muster'` → POSIX ERE (`+`, `?`, `|` funktionieren, aber kein `\d`)
- `grep -P 'Muster'` → PCRE (wie regex101, `\d` funktioniert)

**Case-Sensitivity**  
Regex unterscheidet standardmäßig Groß- und Kleinschreibung. Drei Wege:
- Flag `i` im Flags-Feld (global für das gesamte Muster)
- `(?i)` direkt im Muster (ab dieser Stelle case insensitive)
- `(?i:...)` nur für einen Teil des Musters (Fortgeschrittene)

**Metazeichen escapen**  
Diese Zeichen müssen mit `\` escaped werden, wenn sie wörtlich gemeint sind:  
`. * + ? ^ $ { } [ ] ( ) | \ /`

> **Hinweis:** `/` gilt speziell für regex101 mit PCRE2 (PHP), da PHP `/` als Pattern-Delimiter verwendet.
