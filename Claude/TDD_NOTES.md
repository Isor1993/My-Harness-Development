# TDD_NOTES.md — Stoffsammlung fürs Technical Design Document

Ownership: Grobe Einträge für das Uni-TDD (Terrain-/Mesh-Generation,
Abgabe ca. 2026-07-28). Kein fertiger Text — nur Rohmaterial, aus dem
das TDD am Ende generiert wird. Nur echte Arbeit am Uni-Projekt —
Harness-Entwicklung gehört nicht hierher.
Format: `- JJJJ-MM-TT — [Themenblock] Stichpunkt (1–3 Zeilen)`.
Themenblöcke frei wählen (z. B. Architektur, Terrain, Mesh, Input,
Tools); neue Einträge einfach anhängen — sortiert wird beim Generieren.

## Einträge
- 2026-07-18 — [Mesh] MeshBuilder als statischer Übersetzer Heightmap→Mesh.
  Kernformeln: res² Vertices, (res−1)²·6 Triangle-Indizes, i = z·res + x;
  zwei Dreiecke pro Gitterquadrat, Winding im Uhrzeigersinn (von oben).
- 2026-07-18 — [Mesh] Verifikation über drei Tests: flach (Kette steht),
  random (Höhen kommen an; Stachel-Chaos motiviert Perlin Noise), Rampe
  (Richtung stimmt). Rampen-Test deckte [z,x]/[x,z]-Mismatch zwischen
  Schreiber und Leser auf → Konvention festgelegt.
- 2026-07-18 — [Mesh] Praxisgrenzen: 16-Bit-Indexpuffer max. 65.535
  Vertices (res ≤ 255 oder IndexFormat.UInt32); SerializeField-Typwechsel
  resettet den Inspector-Wert.
- 2026-07-18 — [Architektur] Mesh komplett im Code statt Unity-Terrain
  oder Mesh-Asset (Begründung in DECISIONS 2026-07-18).
