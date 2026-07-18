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
- 2026-07-18 — [Terrain] HeightmapGenerator: statisch, oktavierter Perlin
  Noise. Kernformeln: Amplitude = persistence^o, Frequenz = lacunarity^o
  (per `*=` in der Schleife), Summe / Amplitudensumme → Ergebnis bleibt
  0–1. Seed → System.Random → ein Offset pro Oktave
  (Mathf.PerlinNoise ist seedlos).
- 2026-07-18 — [Terrain] Verifikation über drei Tests: Determinismus
  (gleicher Seed = gleiches Terrain), Seed-Variation (anderes Terrain),
  Oktaven 1 vs. 6 (mehr Detail, gleiche Höhe — Beweis der Normalisierung).
- 2026-07-18 — [Terrain] Parameter-Guards per [Min]/[Range] im Inspector:
  resolution ≥ 2, noiseScale > 0, octaves ≥ 1 (alle drei verhindern
  Division durch null), lacunarity ≥ 1 (sonst kehren sich Oktaven um).
  Oktaven über ~6 bringen keinen sichtbaren Mehrwert (0.5⁹ ≈ 0,2 %),
  kosten aber linear Rechenzeit.
