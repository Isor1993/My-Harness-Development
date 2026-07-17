# ROADMAP.md — Baureihenfolge

Ownership: Nur Baureihenfolge. Was fertig ist, steht im FEATURE_LOG.md,
Begründungen in DECISIONS.md — nie hier.

## Grobziel
Generischer Harness für Game-Dev-Projekte und Brainstorm-/Learn-Sessions:
.md-Dateien als Gedächtnis, Sessions als Wegwerf-Arbeitsräume. Wird hier
entwickelt und getestet, später pro Projekt (z.B. Isor's Tower) kopiert
und spezialisiert.

## Nahziel (Stand 2026-07-17)
Der Harness ist minimal einsatzbereit. Ab 2026-07-18 Uni-Betrieb:
Terrain- und Mesh-Generation samt Bestückung, Abgabe ca. 2026-07-28.
Ausgearbeitet wird nur noch, was der Praxisbetrieb verlangt.

## Erledigt
- [x] Kern: INDEX.md, ROADMAP.md, CLAUDE.md (minimal), Übergabetest
  (2026-07-14)
- [x] WORKFLOW.md: Grundregeln + alle vier Session-Typen (Brainstorm im
  Einsatz, Rest minimal) (2026-07-17)
- [x] Knowledge-System: externer Ordner `C:\Repos Isor\Knowledge\` +
  KNOWLEDGE_RULES.md (2026-07-17)
- [x] FEATURE_LOG.md + DECISIONS.md angelegt (2026-07-17)
- [x] CODE_GUIDELINES.md als Rohmaterial: Uni-Conventions + gefilterte
  Dozenten-Rules, Zwei-Block-Struktur (2026-07-17)

## Als Nächstes
1. [ ] Praxistest: Uni-Betrieb ab 2026-07-18, Harness dabei nachschärfen
2. [ ] CODE_GUIDELINES.md beim ersten Development-Einsatz in der Praxis
   nachschärfen (Rohmaterial liegt vor)
3. [ ] TDD für die Uni-Abgabe aus TDD_NOTES.md generieren
   (vor ca. 2026-07-28)

## Später (nur bei Bedarf)
- DOC_RULES.md, CLAUDE.md voll ausarbeiten, GLOSSARY.md,
  GDD_RULES.md + GDD.md, Knowledge-Archivierung automatisieren
- Save-System-Design (Persistenz über Szenen: JSON; SO höchstens als
  Laufzeit-Container) — eigene Design-Session, wenn es ansteht
- ClaudeSetup-artiges Editor-Setup-Script — erst wenn Isor sicher
  programmiert (Lernphase: Isor tippt selbst)
