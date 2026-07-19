# ROADMAP.md — Baureihenfolge

Ownership: Nur Baureihenfolge. Was fertig ist, steht im FEATURE_LOG.md,
Begründungen in DECISIONS.md — nie hier.

## Grobziel
Generischer Harness für Game-Dev-Projekte und Brainstorm-/Learn-Sessions:
.md-Dateien als Gedächtnis, Sessions als Wegwerf-Arbeitsräume. Wird hier
entwickelt und getestet, später pro Projekt (z.B. Isor's Tower) kopiert
und spezialisiert.

## Nahziel (Stand 2026-07-18)
Der Harness ist minimal einsatzbereit. Uni-Betrieb läuft: Terrain- und
Mesh-Generation samt Bestückung (ASSIGNMENT_PCG.md, kombiniert mit
ASSIGNMENT_TOOL.md — siehe DECISIONS 2026-07-18).
Termine: Die Einzeltermine der Aufgaben sind formative Abgaben
(Richtlinie mit Puffer) — PCG formativ 2026-07-24, Engine-Tool war
formativ 2026-07-10. Die echte Abgabe aller Module ist das Portfolio am
2026-08-21. Isors eigenes Ziel: alles bis ca. Ende Juli fertig, damit
der August Puffer für Bugs, Tests und Review bleibt.
Ausgearbeitet wird am Harness nur noch, was der Praxisbetrieb verlangt.

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
3. [ ] Uni: PCG + Engine-Tool fertigstellen — Chunk-Umbau fertig
   (2026-07-19) → Randnotizen fixen (HeightCurve unter Null,
   Plateau-Krater, Beleuchtungs-Nähte an Chunk-Grenzen prüfen) →
   Parameter-Tuning (Ziel-Look: flache Basis, ~5 große Hügel, Senken
   für Wasser) → Wasserspiegel → Platzierungs-Stufe (Editor-Tool ist
   fertig; Ziel: Ende Juli)
4. [ ] TDD für die Uni-Abgabe aus TDD_NOTES.md generieren (Ziel: Ende
   Juli; Portfolio-Abgabe 2026-08-21)
5. [ ] Uni: Threadoptimierung (K2, K3, S3; formativ 2026-08-07)
6. [ ] Uni: Spiel-Kleinteile — Menü, Interaktion, evtl. Player Controller
7. [ ] Uni: akademische Aufgabe — eine zusätzliche Quelle ergänzen
8. [ ] Uni: Gras-Models aus Blender optimieren — zu viele Triangles
   (Dozenten-Feedback, Performance-Risiko bei Massen-Platzierung durchs
   PCG-Tool)
9. [ ] Uni: Gesamt-Review des Projekts vor der Portfolio-Abgabe
   (Bugs, Verbesserungen, Testen; bekannte halbfertige Stellen werden
   beim PCG-Zusammenbau sichtbar und nach und nach gefixt)

## Später (nur bei Bedarf)
- DOC_RULES.md, CLAUDE.md voll ausarbeiten, GLOSSARY.md,
  GDD_RULES.md + GDD.md, Knowledge-Archivierung automatisieren
- Save-System-Design (Persistenz über Szenen: JSON; SO höchstens als
  Laufzeit-Container) — eigene Design-Session, wenn es ansteht
- ClaudeSetup-artiges Editor-Setup-Script — erst wenn Isor sicher
  programmiert (Lernphase: Isor tippt selbst)
- Harness-Dokumente auf Englisch umstellen — prüfen, ob das in der
  Praxis besser funktioniert (Unterhaltung darf deutsch bleiben)
- Development-Session „automatisierter Modus" (Claude baut, Isor
  reviewt) — erst nach der Lernphase
- Uni-Projekt-Kür nach dem Portfolio: echte Flüsse (Spline-Mesh,
  evtl. gekaufte Assets), Insel via Falloff-Map + Wasserspiegel
