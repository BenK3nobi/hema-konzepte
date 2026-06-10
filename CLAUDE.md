# CLAUDE.md — HEMA Kurs

## Was ist das
Planungs- und Didaktik-Repository (Deutsch) für einen Liechtenauer-Langschwert-Kurs.
Es enthält die **Trainingspläne** (Wie und wann lehrt man?) und die didaktisch-
sportwissenschaftliche Begründung dahinter. **Kein Code** — reine Markdown-Inhalte.

## Die Idee
Bodenständiges Fechtsystem auf Basis Liechtenauer (Peter von Danzig). Lücken im
Langschwert werden durch **Fiore** ausgeglichen. **Dolch & Stange** dienen als
Deloading, **Ringen** ergänzt; Ausblick in **Dussack & Säbel**. **Meyer** als
Vertiefung. Grundlage: sportmedizinische/wissenschaftliche Trainingsweisen
(Motorlernphasen, Constraints-Led Approach, Verletzungsprävention, Periodisierung).

## Zwei Kontexte (ineinander überführbar)
- **Verein** (`docs/curriculum-verein/`): Jahresprogramm, Anfänger (A1–A3) +
  Fortgeschrittene (F1–F3), parallel, plus offenes Training.
- **Hochschulsport** (`docs/curriculum-hochschulsport/`): 13-Wochen-Kurs, 1×90 min,
  Anfänger + Fortgeschrittene gemischt im selben Raum.

Beide Kurse greifen auf **dieselben Techniken** zu. Da die App den Fortschritt
**pro Technik** speichert, kann ein Teilnehmer zwischen den Kursen wechseln, ohne
Gelerntes zu verlieren (siehe `App-Integration/ueberfuehrbarkeit.md`).

## Architektur / Quelle der Wahrheit
- **App = Glossar & Technik-Referenz** (Single Source of Truth für *was* eine
  Technik ist: Kurz-Summary, `solo_drills`). Liegt in `../HEMA-app (copy)/app`,
  Nuxt 4 + Nuxt Content (Markdown + YAML-Frontmatter).
  Techniken: `content/techniques/gl-*.md` (Glossar) und `step-*.md` (Schrittarbeit).
  Der **Slug** (z. B. `gl-zornhau`) ist die stabile ID.
- **Dieses Repo = Didaktik** (*wie/wann* lehren). Wochenpläne verweisen auf die
  App-Technik-**Slugs**, definieren aber keine Technik-Inhalte neu (kein Duplikat).
- **Kurse & Übergänge** leben in der App-Datenbank (SQLite). Geseedet über
  `../HEMA-app (copy)/app/scripts/seed.mjs` mit den Helfern `upsertCourse` und
  `upsertGraduation` (Graduierung = Liste `[{slug, min_status}]`).

## Konventionen
- Sprache: **Deutsch**. Fachbegriffe exakt wie im App-Glossar schreiben.
- Format: **Markdown** ist die Quelle. CSV/PDF nur als generierter Output oder im
  `archiv/`. Keine CSV/PDF mehr als primäre Quelle pflegen.
- Jeder Wochenplan listet die behandelten Techniken über ihren **App-Slug**
  (z. B. `gl-zornhau`, `step-dreieckschritt`) in einem Abschnitt „Techniken (App)".
- Didaktik-Leitplanken: Sicherheit zuerst; **max. 1–2 neue Techniken pro Einheit**
  (neuronale Interferenz); leichte Ausrüstung früh (Risk Compensation); jede
  4. Woche Deload.

## Wo was liegt
Alle Kurs-Inhalte liegen im Ordner **`docs/`** (MkDocs-Konvention, siehe
„Website" unten). Dateien im Repo-Root sind Werkzeug/Meta, kein Kursinhalt.
```
CLAUDE.md                    Dieses Dokument (Repo-Root, nicht auf der Website)
mkdocs.yml                   Website-Konfiguration (MkDocs Material)
requirements.txt             Python-Abhängigkeit (mkdocs-material) für den Build
.gitlab-ci.yml               CI: baut & veröffentlicht via GitLab Pages
archiv/                      Diskussions-PDFs (Recherche-Rohmaterial, nicht publiziert)
docs/                        ← alle veröffentlichten Inhalte
  index.md                   Startseite der Website
  Impressum.md, Datenschutz.md  Pflichtseiten (öffentliche Seite, ausfüllen!)
  quellen.md                 Quellen-/Pad-Übersicht
  Konzept/                   Warum es so aufgebaut ist
    grundidee.md               Vision, Systemwahl, Erweiterungslogik
    didaktik-rationale.md      Motorlernphasen, Blocked/Random, CLA, Lektionierung
    verletzungspraevention.md  HEMA-Verletzungsprofil, Prehab, Helm, Risk Compensation
    periodisierung.md          Mikro/Meso/Makro, Deload, Saisons
    methoden.md                Spiele, Drills, Eskalationspyramide, Tutoren/CLA
  Grundlagen/                Gemeinsame Basis beider Kurse
    glossar.md                 Index auf App-Slugs (Quelle = App)
    sicherheit-kodex.md        Sicherheitsregeln + Verhaltenskodex
    ausruestung.md             Ausrüstungsliste
  curriculum-verein/         Jahresprogramm
    README.md, anfaenger-A1..A3.md, fortgeschrittene-F1..F3.md, pruefung-uebergang.md
  curriculum-hochschulsport/ 13-Wochen-Kurs
    README.md, 13-wochen-plan.md, 13-wochen-detailplan.md
  Erweiterungen/             Die „Idee"-Bausteine
    fiore-ergaenzungen.md, dolch-stange-ringen.md, dussack-saebel-ausblick.md, meyer-vertiefung.md
  App-Integration/           Brücke zur App
    technik-mapping.md         Crosswalk Curriculum-Technik → App-Slug (+ Lücken)
    ueberfuehrbarkeit.md       Hochschulsport ↔ Verein, via Graduierungen
    kurs-definitionen.md       Specs für seed.mjs (Kurse, Graduierungen)
```

## Website (öffentliche Doku via MkDocs + GitLab Pages)
Die Inhalte in `docs/` werden als **statische Website** veröffentlicht — volle
Transparenz für Schüler und Trainer. Die **App wird nur verlinkt**, nicht
eingebettet (Platzhalter-URL `https://hema-app.example` in `mkdocs.yml`,
`docs/index.md` und `docs/Grundlagen/glossar.md` nach App-Deploy ersetzen).
```bash
pip install -r requirements.txt   # einmalig (am besten in venv)
mkdocs serve                      # lokale Vorschau auf http://127.0.0.1:8000
mkdocs build --strict             # Build prüfen (fängt kaputte Links/Nav)
# Deploy: Push auf den GitLab-Default-Branch → .gitlab-ci.yml baut nach public/
```

## Nützliche Befehle (App)
```bash
cd "../HEMA-app (copy)/app"
npm install
npm run seed     # Demo-/Kursdaten laden
npm run dev      # Web-App starten
npm run editor   # Offline-Editor für Techniken/Kurse (Port 4000)
```
