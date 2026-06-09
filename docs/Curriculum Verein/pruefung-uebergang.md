# Prüfung & Übergang (Level-Checks)

Definiert die **Minimalanforderungen** und das **Format** der Übergänge zwischen
den Blöcken. Maßgeblich ist der **Technik-Status in der App** (Trainer-Bewertung
pro Slug), nicht die besuchte Wochenzahl — das macht Verein und Hochschulsport
**überführbar** (siehe `../App-Integration/ueberfuehrbarkeit.md`). Die hier
genannten Schwellen entsprechen den Graduierungen aus
`../App-Integration/kurs-definitionen.md`.

## Format der Level-Checks
- **Wann:** in den Deload-Wochen (jede 4.), ohne Zusatzbelastung.
- **Wie:** kombinierter Check — (1) Technik isoliert zeigen, (2) in einer Lektion
  situativ anwenden, (3) im Constraint-Sparring nutzen. Entspricht der
  Status-Hierarchie `technique_ok → tactical_application → freeplay_application`.
- **Bewertung:** Trainer trägt den Status pro Technik in der App ein
  (`trainer_checks`). Selbsteinschätzung zählt nicht.
- **Haltung:** kein Bestehen/Durchfallen-Drama — der Check macht den Lernstand
  sichtbar und liefert den nächsten Lernplan (offene Slugs).

## Übergangsschwellen

### A1 → A2 — Graduierung *Grundlagen-Check* (`rank_order` 1)
**Minimal (`technique_ok`):** Grundstand & Schritt (`step-grundstand`,
`step-dreieckschritt`), Grundhäue (`gl-oberhau`, `gl-unterhau`, `gl-mittelhau`),
Stich (`gl-stich`), Distanz (`gl-mensur`), vier Huten (`gl-vom-tag`, `gl-ochs`,
`gl-pflug`, `gl-alber`), einfache Versatzung (`gl-versetzen`).
**Format:** Stationen-Check (wie A1 Woche 12) + kurzes eingeschränktes Gefecht.

### A2 → A3 — Graduierung *Freischüler Langschwert* (`rank_order` 2)
**Minimal (`technique_ok`):** alle 5 Meisterhäue (`gl-zornhau`, `gl-krumphau`,
`gl-zwerchhau`, `gl-schielhau`, `gl-scheitelhau`); zusätzlich Zornhau-Ort-Anwendung.
**Format:** Meisterhau-Rotation (Hut sehen → passenden Hau) + Constraint-Sparring
„Meisterhau muss vorkommen". *(Diese Graduierung existiert bereits in `seed.mjs`.)*

### A3 → F1 — Schlüsseltechniken `tactical_application`
**Minimal:** Winden (`gl-winden`), Indes/Fühlen (`gl-indes`, `gl-f-hlen`) sowie
Meisterhäue auf `tactical_application`; ein vollständiges Stück demonstrieren.
**Format:** Lektion (asymmetrisch) + halbfreies Gefecht mit Bindungsfokus.

### F1 → F2 → F3 — Graduierung *Übergang Fortgeschrittene* (`rank_order` 3)
**Minimal:** Meisterhäue + Winden Richtung `freeplay_application`; taktische
Werkzeuge (Duplieren/Mutieren, Fehler/Feller) situativ; Doppeltreffer-Disziplin.
**Format:** Szenario-/Random-Sparring mit taktischen Auflagen; Quellenstück
selbst auslegen; (F3) eine Anfänger-Lektion geben.

## Eintragen in der App (praktisch)
1. Trainer öffnet die Graduierungsübersicht des Mitglieds.
2. Pro geprüfter Technik den erreichten `trainer_status` setzen.
3. App zeigt, ob die Graduierung erfüllt ist (alle Slug-Anforderungen + ggf.
   `min_trainer_checks`). Erfüllt → Block-Aufstieg bzw. Einstufung beim Wechsel.
