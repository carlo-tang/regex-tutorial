# Regex Tutorial – Projektnotizen

**Datum:** 2026-06-25
**Verzeichnis:** /home/carlo/regex-tutorial

---

## Projektziel

Leichtes Regex-Tutorial für ein gemischtes Team (Linux-Admin + Netzwerkkollegen) via Microsoft Teams.
Ziel: Praxisnaher Einstieg, schnelle Erfolgserlebnisse, keine Kommandozeilen-Vorkenntnisse nötig.

---

## Rahmenbedingungen

- **Publikum:** Netzwerkkollegen (GUI-affin, nicht CLI-erfahren) + Carlo (Linux-Admin)
- **Format:** Teams-Präsentation, Präsentator teilt Bildschirm
- **Platform:** regex101.com · Flavor: **PCRE2 (PHP)** · Flags: `g` `m`
- **Warum PCRE2 (PHP):** Voreinstellung auf regex101. PHP verwendet `/` als Pattern-Delimiter → `/` muss escaped werden (`\/`), gehört daher zu den Metazeichen
- **Umfang:** Leichter Einstieg, kein Tiefgang in Backreferences/Capture Groups

---

## Workflow

- `lecture-notes.md` ist **Source of Truth** — hier wird gearbeitet
- `aufgaben.md` leitet sich davon ab — wird am **Session-Ende** synchronisiert
- `cheatsheet.md` — Referenzkarte zum Mitnehmen (noch nicht aktualisiert)

---

## Entscheidungen

- regex101.com + vorbereitete Links statt eigener Webseite
- `/` gehört zu den Metazeichen (Zeile 5) wegen PHP-Delimiter
- Cisco-Seite (403) → PDF vorhanden in kontext/. Inhalt (PCRE Cambridge Doku) zu fortgeschritten — ausgeklammert
- POSIX/grep-Unterschiede: nur kurz erwähnen, nicht vertiefen
- Backreferences, Suchen/Ersetzen: zu fortgeschritten, rauslassen
- `(?i:...)` und `(?-i)`: nur als Fortgeschrittenen-Tabelle, keine Live-Demo

---

## Aktuelle Links (regex101)

| Phase | Link | Version |
|---|---|---|
| Phase 0 – Zeichentypen | https://regex101.com/r/9jKmsx/1 | v1 |
| Phase 1 – Hallo Welt | https://regex101.com/r/QEd4Fj/4 | v4 |
| Phase 2 – IP-Adressen | https://regex101.com/r/lO9tf0/2 | v2 |

---

## Struktur (50 Minuten)

- **Intro:** MIT Cheatsheet zeigen + deutsche Fachbegriffe + regex101-Oberfläche erklären (Explanation, Match information, Quick reference)
- **Phase 0** – Zeichentypen, ASCII-Fallstricke, Quantifizierer (~15 min)
- **Phase 1** – Hallo Welt: Literale, Case Sensitivity, `(?i)`, Punkt, Anker, Alternation (~15 min)
- **Phase 2** – IP-Adressen: `\d`, Escaping, Quantifizierer, `https?` (~15 min)
- **Puffer / Fragen:** 5 min
- **Bonus + Exkurs:** wenn Zeit bleibt

---

## Wichtige inhaltliche Entscheidungen

- ASCII-Fallstricke in Phase 0: `[a-Z]` Fehler, `[A-z]` unerwartete Sonderzeichen, `[0-z]` noch mehr
- `[A-Za-z]` explizit kombinieren statt Ranges über Grenzen
- Quantifizierer-Demo auf Zeilen 6+7 (`122333444455555`, `aBBcccDDDDeeeee`)
- `https?` als `?`-Beispiel — Phase 0b erwähnt, Phase 2 live mit http/https im Test-String
- `Hallo.Welt` matcht nur 1 Zeile — Punkt = genau 1 Zeichen, nicht "irgendwas dazwischen"
- Bonus-Regex private IPs: jede Alternative vollständig ausschreiben (172.16 und 192.168 verbrauchen bereits 2 Oktette)
- Exkurs: Regex = Format-Prüfung, nicht Wertebereich. Vollständiger IP-Regex gezeigt, nicht getippt.
- Matching vs. Validierung: `^` und `$` als Anker für Validierung

---

## Dateien

- `lecture-notes.md` — Moderationsvorlage (Source of Truth): Moderationstext, Regexe, Lösungen, Erklärungen
- `aufgaben.md` — Kollegenversion (ohne Lösungen), synchron mit lecture-notes
- `cheatsheet.md` — Referenzkarte (noch nicht auf aktuellem Stand)
- `kontext/` — Quellmaterial (Cisco PDF, Kollegen-Input, TLCL)

---

## Nächste Schritte

- [ ] Tutorial durchführen
- [ ] cheatsheet.md auf aktuellen Stand bringen
- [ ] Feedback nach Tutorial einholen
