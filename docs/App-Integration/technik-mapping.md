# Technik-Mapping (Curriculum ↔ App-Slug)

Crosswalk zwischen den im Curriculum genannten Techniken und den **App-Slugs**
(`../../../HEMA-app (copy)/app/content/techniques/`). Die App ist die Quelle der
Wahrheit für Technik-Definitionen; Wochenpläne referenzieren **nur Slugs**.
Status: ✅ Slug existiert · ⛔ Lücke (Slug fehlt, in Phase 5 anzulegen).

## Langschwert — vorhanden ✅
Das Langschwert-Glossar der App ist nahezu vollständig.

| Bereich | Slugs |
|---|---|
| Fünf Wörter / Prinzipien | `gl-vor` `gl-nach` `gl-indes` `gl-stark-st-rke` `gl-schwach-schw-che` `gl-f-hlen` `gl-fechttempo` `gl-mensur` `gl-bl-e` `gl-leng-und-ma` `gl-zettel` |
| Drei Wunder | `gl-drei-wunder` `gl-hau` `gl-stich` `gl-schnitt` |
| Grundhäue | `gl-oberhau` `gl-unterhau` `gl-mittelhau` `gl-wechselhau` |
| Meisterhäue | `gl-meisterh-ue` `gl-zornhau` `gl-krumphau` `gl-zwerchhau` `gl-schielhau` `gl-scheitelhau` |
| Huten | `gl-hut` `gl-leger` `gl-haupthuten` `gl-vom-tag` `gl-ochs` `gl-pflug` `gl-alber` `gl-langer-ort-langort` `gl-nebenhut` `gl-schrankhut` `gl-wechselhut` `gl-zornhut` `gl-einhorn` `gl-eisenpforte` `gl-beileger-beihuten` |
| Bindung/Handarbeit | `gl-band-bindung` `gl-anbinden` `gl-handarbeit` `gl-h-ngen` `gl-winden` `gl-versetzen` `gl-absetzen` `gl-abschneiden` `gl-abnehmen` `gl-streichen` `gl-duplieren` `gl-mutieren` `gl-zucken` `gl-durchwechseln` `gl-umschlagen` `gl-schnappen` `gl-fehler-feller` `gl-brechen-bruch` `gl-ansetzen` `gl-abziehen-abzug` `gl-nachreisen` `gl-berlaufen` `gl-kron` `gl-sprechfenster` `gl-st-ck` |
| Nahdistanz | `gl-ringen-am-schwert` `gl-durchlaufen` `gl-einlaufen` `gl-h-ndedr-cken` `gl-halbschwert` `gl-mordschlag` `gl-harnischfechten` |
| Schwert-Anatomie | `gl-gehilz-geh-ltz` `gl-fl-che` `gl-lange-schneide` `gl-kurze-schneide` `gl-ort` `gl-leichmeister` `gl-b-ffel` `gl-blo-fechten` |
| Schrittarbeit | `step-grundstand` `step-beinarbeit` `step-zufechten` `step-dreieckschritt` `step-doppelter-dreieckschritt` `step-passierschritt` `step-nachholschritt` `step-kreuztritt` `step-seitschritt` `step-gestohlener-schritt` `step-kurze-schritte` `step-vor` `step-nach` `step-indes` `step-tempo` `step-krieg` `step-ipsilaterale-fu-regel` `step-zam-of-eyner-wogen` |
| Schritt (italienisch) | `step-mosse` `step-motus` `step-caminieren` `step-passata` `step-botta-lunga-passo-straordinario` |

> Hinweis: Die beiden Curricula nennen Techniken teils in Prosa. Alle in den
> Wochenplänen (`curriculum-hochschulsport/13-wochen-plan.md`,
> `curriculum-verein/anfaenger-A1.md`) referenzierten Begriffe sind durch obige
> Slugs gedeckt. Konsistenz-Check siehe unten.

## Erweiterungs-Waffen — Lücken ⛔ (Phase 5)
Die App ist auf Langschwert ausgelegt. Für die „Idee"-Bausteine fehlen Slugs.
Die Disziplin-Enums existieren bereits (`messer`/dolch, `ringen`, `stab`, sowie
Schule `Fiore`), die Quelle `src-meyer-1570` ist vorhanden — aber **keine
`src-fiore`-Quelle** und kaum disziplinfremde Technikdateien.

| Baustein | Disziplin (App-Enum) | Status | Anzulegen (Beispiele) |
|---|---|---|---|
| **Fiore-Ergänzungen** | `langschwert` / school `Fiore` | ⛔ | Quelle `src-fiore`; Techniken z. B. `gl-fiore-rota`, Nahdistanz/Greifen |
| **Dolch** | `messer`/`dolch` | ⛔ | `gl-dolch-*` (Grundlagen, Entwaffnen) |
| **Stange/Stab** | `stab` | ⛔ | `gl-stab-*` (Grundhaltung, Stöße) |
| **Ringen** | `ringen` | teilw. | `gl-ringen-am-schwert` ✅; eigenständiges Ringen ⛔ |
| **Dussack** | `andere` | ⛔ | `gl-dussack-*` (Ausblick) |
| **Säbel** | `andere` | ⛔ | `gl-saebel-*` (Ausblick) |
| **Meyer-Vertiefung** | `langschwert` | teilw. | Quelle `src-meyer-1570` ✅; Meyer-spezifische Stücke ⛔ |

Details je Baustein in `../Erweiterungen/`. Anlegen über das App-Schema
(`content/techniques/_VORLAGE.md`) in Phase 5.

## Konsistenz-Check (manuell/Skript)
Prüfen, dass jeder in einem Wochenplan referenzierte Slug existiert:
```bash
APP="../HEMA-app (copy)/app/content/techniques"
grep -rhoE 'gl-[a-z0-9-]+|step-[a-z0-9-]+' curriculum-hochschulsport curriculum-verein \
  | sort -u | while read s; do [ -f "$APP/$s.md" ] || echo "FEHLT: $s"; done
```
Erwartung: keine Ausgabe für Langschwert-Slugs; Erweiterungs-Slugs erscheinen bis
Phase 5 als „FEHLT" und sind hier oben als ⛔ dokumentiert.
