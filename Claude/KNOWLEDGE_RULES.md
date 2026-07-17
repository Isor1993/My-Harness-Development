# KNOWLEDGE_RULES.md — Regeln für das externe Wissensarchiv

Ownership: Schreib- und Ablageregeln für den externen Knowledge-Ordner.
Warum extern: Wissen ist projektübergreifend.

## Ablageort
`C:\Repos Isor\Knowledge\` — außerhalb des Harness, für alle Projekte.

## Struktur
- Unterordner = Themengruppen, wachsen nach Bedarf.
  Bestand: `Patterns/`, `Unity/`, `ProcGen/`, `Seiten/`.
- Eine .md-Datei pro Konzept. Dateiname: `klein-mit-bindestrichen.md`.
- `Seiten/` enthält Offline-Kopien der Artifact-Seiten als eigenständige
  HTML-Dateien: `JJJJ-MM-TT-titel.html`.
- Kein eigener Index: Der Ordnerbaum ist der Index.

## Format pro Datei
1. `# Titel`
2. `## Was & Warum` — 3–6 Sätze in eigenen Worten (nicht kopiert).
3. Optional `## Anwendung im Projekt` — das konkrete Beispiel aus der Session.
4. Optional ein Visual bei komplexen Konzepten: Mermaid-Block direkt im
   Markdown; zusätzlich Link auf die Artifact-Seite und/oder Offline-Kopie.
5. Abschlusszeile `Quelle:` mit Session-Datum und Links.
- Härtegrenze: Passt es nicht auf einen Bildschirm, sind es zwei Konzepte
  → zwei Dateien. Verweise zwischen Dateien als relative Markdown-Links.

## Wann wird geschrieben
- Pflicht in Brainstorm-Sessions im Uni-Modus (siehe WORKFLOW.md).
- Zusätzlich fragt Claude bei jedem Session-Ende — egal welcher Typ —
  ob etwas als Knowledge behalten werden soll.
- Gesammelt am Session-Ende vor /clear: Claude schlägt die Einträge vor,
  Isor entscheidet, was gespeichert wird.
- Bestehende Datei zum selben Konzept: erweitern statt duplizieren.
