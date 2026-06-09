# Kurs- & Graduierungs-Definitionen (Spezifikation für `seed.mjs`)

Konkrete Vorlage für die App-Brücke. Wird in **Phase 5** in
`../../../HEMA-app (copy)/app/scripts/seed.mjs` umgesetzt (`upsertCourse`,
`upsertGraduation`) und mit `npm run seed` angelegt. Idempotent: bestehende
Einträge werden nicht überschrieben.

## Helfer-Signaturen (Ist-Zustand der App)
```js
upsertCourse({ name, semester, description, code, disciplines, trainerIds, memberIds, createdBy })
upsertGraduation({ name, discipline, rank_order, description, requirements })
// requirements = { techniques: [{slug, min_status}], min_total_xp?, min_trainer_checks? }
// min_status ∈ { technique_ok, tactical_application, freeplay_application }
```
Disziplin-Enums vorhanden: `langschwert`, `messer`, `dolch`, `ringen`, `stab`,
`schwert-und-buckler`, `rapier`, `andere`.

## Kurse
| Name | semester | code | disciplines |
|---|---|---|---|
| **Hochschulsport** | je Semester (z. B. `SS26`) | `HSP-SS26` | `langschwert` |
| **Verein Anfänger** | `2026` | `VEREIN-A` | `langschwert` |
| **Verein Fortgeschrittene** | `2026` | `VEREIN-F` | `langschwert`, `dolch`, `stab`, `ringen` |

> Die Erweiterungs-Disziplinen am Fortgeschrittenen-Kurs bilden den
> **Deloading-/Winterblock** ab (Dolch/Stange/Ringen, siehe
> `../Erweiterungen/dolch-stange-ringen.md`).

## Graduierungen (Disziplin `langschwert`)
Aufsteigende `rank_order`. Die mittlere Stufe entspricht der bereits in `seed.mjs`
vorhandenen **„Freischüler Langschwert"** — beibehalten, nur ergänzen.

### rank_order 1 — „Grundlagen-Check"
Bewegung, Grundhäue, vier Huten, Distanz, einfache Versatzung — Ende **A1** /
Übergang **A2** (siehe `ueberfuehrbarkeit.md`).
```js
upsertGraduation({
  name: 'Grundlagen-Check', discipline: 'langschwert', rank_order: 1,
  description: 'Bewegung, Grundhäue, Huten, Distanz, einfache Versatzung.',
  requirements: { techniques: [
    { slug: 'step-grundstand',  min_status: 'technique_ok' },
    { slug: 'step-dreieckschritt', min_status: 'technique_ok' },
    { slug: 'gl-oberhau',       min_status: 'technique_ok' },
    { slug: 'gl-unterhau',      min_status: 'technique_ok' },
    { slug: 'gl-mittelhau',     min_status: 'technique_ok' },
    { slug: 'gl-stich',         min_status: 'technique_ok' },
    { slug: 'gl-mensur',        min_status: 'technique_ok' },
    { slug: 'gl-vom-tag',       min_status: 'technique_ok' },
    { slug: 'gl-ochs',          min_status: 'technique_ok' },
    { slug: 'gl-pflug',         min_status: 'technique_ok' },
    { slug: 'gl-alber',         min_status: 'technique_ok' },
    { slug: 'gl-versetzen',     min_status: 'technique_ok' }
  ], min_trainer_checks: 8 }
})
```

### rank_order 2 — „Freischüler Langschwert" *(bereits vorhanden)*
Die 5 Meisterhäue `technique_ok`. = Einstieg **A3 / F1**. Aktuell in `seed.mjs`
mit `min_total_xp: 100, min_trainer_checks: 5`. **Unverändert lassen**, ggf.
`rank_order` auf 2 setzen, falls noch 1.

### rank_order 3 — „Übergang Fortgeschrittene"
Meisterhäue **taktisch** anwendbar + Winden/Bindungsarbeit — Einstieg **F1+**.
```js
upsertGraduation({
  name: 'Übergang Fortgeschrittene', discipline: 'langschwert', rank_order: 3,
  description: 'Meisterhäue taktisch + Winden/Bindungsarbeit im Gefecht.',
  requirements: { techniques: [
    { slug: 'gl-zornhau',     min_status: 'tactical_application' },
    { slug: 'gl-krumphau',    min_status: 'tactical_application' },
    { slug: 'gl-zwerchhau',   min_status: 'tactical_application' },
    { slug: 'gl-schielhau',   min_status: 'tactical_application' },
    { slug: 'gl-scheitelhau', min_status: 'tactical_application' },
    { slug: 'gl-winden',      min_status: 'technique_ok' },
    { slug: 'gl-indes',       min_status: 'tactical_application' },
    { slug: 'gl-versetzen',   min_status: 'tactical_application' }
  ], min_trainer_checks: 15 }
})
```

## Umsetzungshinweise (Phase 5)
- Neue Kurse/Graduierungen in den `if (!SKIP_DEMO)`-losen Teil bzw. einen eigenen
  produktiven Block setzen (Demo-Daten bleiben optional).
- `createdBy: adminId` mitgeben (Helper benötigt es).
- Codes (`VEREIN-A`/`VEREIN-F`) sind Einschreibe-Codes — vor Veröffentlichung
  ändern (werden gehasht gespeichert).
- Nach Anlegen: in der App-Graduierungsübersicht verifizieren (siehe
  Verifikation im Plan).
