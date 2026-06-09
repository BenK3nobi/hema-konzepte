# Überführbarkeit (Hochschulsport ↔ Verein)

Beide Kurse sind **ineinander überführbar**, weil die App den Fortschritt **pro
Technik** speichert (Trainer-Bewertung `trainer_checks.trainer_status` je
`technique_slug`), **kursunabhängig**. Ein Wechsler steigt nicht „in Woche X",
sondern **anhand seines Technik-Status** ein. Die Schwellen sind als
**Graduierungen** definiert (Spezifikation: `kurs-definitionen.md`).

## Status-Hierarchie (App)
Maßgeblich ist immer die **Trainer-Bewertung**, nicht die Selbsteinschätzung
(`../../../HEMA-app (copy)/app/server/utils/graduations.ts`):

```
not_assessed (0) < technique_ok (2) < tactical_application (3) < freeplay_application (4)
```
- **technique_ok** — Technik sauber isoliert ausführbar.
- **tactical_application** — Technik situativ/taktisch korrekt angewandt.
- **freeplay_application** — im freien Gefecht zuverlässig einsetzbar.

## Inhaltlicher Crosswalk
Der 13-Wochen-Hochschulsportkurs deckt inhaltlich **Verein A1 komplett** ab und
führt zusätzlich alle 5 Meisterhäue **auf Einführungsniveau** ein (= Anfang A2).

| Hochschulsport-Woche | Thema | Verein-Entsprechung |
|---|---|---|
| 1–4 | Haltung/Schritte, Huten, Drei Wunder, Distanz | **A1** Wo. 1–6 |
| 5–8 | Vor/Nach, Stark/Schwach, Indes, Versetzen | **A1** Wo. 7–11 (Versatzung, Ketten) |
| — | Konsolidierung/Check | **A1** Wo. 12 (Leistungscheck) |
| 9–13 | Zornhau → Krumphau → Zwerchhau → Schiel-/Scheitelhau | **A2** Einstieg (Meisterhäue, Einführungsniveau) |

## Einstufungsregeln (Graduierungs-basiert)
Maßgeblich ist die in der App erfüllte Graduierung — **nicht** die besuchte
Wochenzahl.

| Erfüllte Graduierung | Einstieg im Verein |
|---|---|
| *(keine)* / nur einzelne Grundtechniken | **A1** (Anfängerblock von vorn) |
| **Grundlagen-Check** (Bewegung + Grundhäue + Huten + Versetzen `technique_ok`) | **A2** (Meisterhäue) |
| **Freischüler Langschwert** (5 Meisterhäue `technique_ok`) | **A3** (Winden, Vertiefung) bzw. **F1** |
| **Übergang Fortgeschrittene** (Meisterhäue `tactical_application` + Winden) | **F1+** |

**Andersherum** (Verein → Hochschulsport) ist trivial: Wer A1/A2 erfüllt hat,
ist im 13-Wochen-Kurs über-qualifiziert und übernimmt sinnvoll eine
**Tutoren-/Lektionsgeber-Rolle** (siehe `../Konzept/methoden.md`).

## Praktischer Ablauf beim Wechsel
1. Wechsler ist in der App bereits angelegt; sein Technik-Status zieht mit.
2. Trainer öffnet die Graduierungsübersicht → sieht erfüllte/offene Anforderungen.
3. Einstieg laut Tabelle oben. Offene Slugs der nächsten Graduierung = der
   konkrete Lernplan.
4. Im Verein werden dieselben Slugs **vertieft** (Ziel: `technique_ok` →
   `tactical_application` → `freeplay_application`), nicht neu gelernt.

> Die Graduierungen selbst (Name, Disziplin, `rank_order`, Requirement-Slugs)
> werden in `kurs-definitionen.md` spezifiziert und über `seed.mjs` angelegt.
