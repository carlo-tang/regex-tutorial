# Regex Tutorial – Projektnotizen

**Datum:** 2026-06-24
**Verzeichnis:** /home/carlo/regex-tutorial

---

## Projektziel

Leichtes Regex-Tutorial für ein gemischtes Team (Linux-Admin + Netzwerkkollegen) via Microsoft Teams.
Ziel: Praxisnaher Einstieg, schnelle Erfolgserlebnisse, keine Kommandozeilen-Vorkenntnisse nötig.

---

## Rahmenbedingungen

- **Publikum:** Netzwerkkollegen (GUI-affin, nicht CLI-erfahren) + Carlo (Linux-Admin)
- **Format:** Teams-Präsentation, Präsentator teilt Bildschirm
- **Platform:** regex101.com (PCRE voreingestellt, GUI-freundlich, Echtzeit-Highlighting)
- **Flavor:** PCRE (auf regex101 vorgewählt, Kollegen sehen das nicht)
- **Umfang:** Leichter Einstieg, kein Tiefgang in Backreferences/Capture Groups

---

## Entscheidungen

- regex101.com + vorbereitete Links (Test-Strings bereits befüllt) statt eigener Webseite
- Cisco-Seite (appreexp.html) war nicht abrufbar (403) — Cisco-Syntax wird ausgeklammert
- POSIX/grep-Unterschiede (z.B. \d vs [0-9]) werden nur kurz erwähnt, nicht vertieft
- Backreferences und Suchen/Ersetzen: zu fortgeschritten, rauslassen

---

## Lehrpfad (geplant)

### Einstieg — Erfolgserlebnis zuerst
1. Literal: `192` eintippen → sofort Treffer sehen
2. Punkt als Wildcard: `192.168` matcht auch `192X168` → Aha-Moment
3. Punkt escapen: `192\.168` → nur echte IPs

### Grundbausteine
- `.` — beliebiges Zeichen
- `^` und `$` — Anfang/Ende der Zeile
- `[0-9]` und `\d` — Zeichenklassen
- `+`, `*`, `?`, `{n,m}` — Quantifizierer
- `[abc]`, `[^abc]` — Mengen und Negation
- `|` — Alternation (oder)
- `\` — Escaping

### Themenblöcke (Netzwerk-Kontext)
- **IP-Adressen:** einfach → präzise (nur gültige Oktette)
- **Hostnamen/FQDNs:** Buchstaben, Ziffern, Bindestriche, Domains

### Bewusst ausgeklammert (für diesen Einstieg)
- Capture Groups / Backreferences
- MAC-Adressen (zu spezifisch für Einstieg)
- Cisco IOS Regex-Syntax
- grep -P / POSIX ERE Unterschiede (nur kurz erwähnen)

---

## Materialien (kontext/)

- `input-kollege.md` — Kollegeninput: gut strukturiert, aber zu umfangreich für Einstieg
- `cisco-regex-link.md` + `Regular Expression Reference - Cisco.pdf` — Seite ist erreichbar (PDF vorhanden). Inhalt: PCRE-Referenzdokumentation der University of Cambridge, von Cisco für ihr Sicherheitstool MARS gehostet. Bestätigt: Cisco verwendet PCRE → unsere Wahl von PCRE auf regex101 ist korrekt. Inhalt selbst (Lookahead, Atomic Grouping, Recursive Patterns) ist viel zu fortgeschritten für den Einstieg — ausgeklammert.
- `TLCL-25.12.pdf` S. 267–288 — "The Linux Command Line" Kap. 19 (grep/POSIX BRE/ERE)

---

## Zeitplan (50 Minuten)

- Intro + regex101 erklären: 5 min
- Phase 1 (Aufgaben 1–3, Hallo Welt): ~15 min
- Phase 2 (Aufgaben 4–6, IP-Adressen): ~20 min
- Puffer / Fragen: 10 min
- Bonus A+B (Hostnamen, private IPs): wenn Zeit bleibt

## Dateien

- `lecture-notes.md` — Moderationsvorlage (Source of Truth): Moderationstext, Regexe, Lösungen, Erklärungen
- `aufgaben.md` — Kollegenversion: leitet sich aus lecture-notes.md ab (erst lecture-notes fertig, dann sync)
- `cheatsheet.md` — Referenzkarte: alle Konzepte + Netzwerk-Beispiele zum Mitnehmen

## Nächste Schritte

- [ ] Links in regex101.com erstellen (Save & Share, Flags: gm, PCRE)
- [ ] Links in aufgaben.md und loesungen.md eintragen
- [ ] Links testen
