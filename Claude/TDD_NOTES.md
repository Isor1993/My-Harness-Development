# TDD_NOTES.md — Stoffsammlung fürs Technical Design Document

Ownership: Grobe Einträge für das Uni-TDD (Terrain-/Mesh-Generation,
Abgabe ca. 2026-07-28). Kein fertiger Text — nur Rohmaterial, aus dem
das TDD am Ende generiert wird.
Format: `- JJJJ-MM-TT — [Themenblock] Stichpunkt (1–3 Zeilen)`.
Themenblöcke frei wählen (z. B. Architektur, Terrain, Mesh, Input,
Tools); neue Einträge einfach anhängen — sortiert wird beim Generieren.

## Einträge
- 2026-07-17 — [Architektur] Grundmuster MVP: Model plain C# (Logik,
  testbar), View nur Anzeige, Presenter als einzige Brücke.
  Details: CODE_GUIDELINES.md Block 2.
- 2026-07-17 — [Architektur] Felder: `[SerializeField] private` als
  Default, keine Singletons; Abhängigkeiten per Inspector-Wiring bzw.
  `Init(...)`-Injection bei gespawnten Objekten.
- 2026-07-17 — [Input] Neues Input System; ein zentraler
  `PlayerInputReader` als einzige Naht zum Input-System.
- 2026-07-17 — [Struktur] Ordnerschema `Assets/Scripts/{System}/`;
  Unity 6; Stil/Naming nach SAE-Conventions (CODE_GUIDELINES.md Block 1).
