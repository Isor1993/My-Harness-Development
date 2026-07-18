# FEATURE_LOG.md — Gebautes

Ownership: Nur fertig Gebautes und Geprüftes — nichts Geplantes.
Nur Projekt-/Spiel-Features; Harness-Bauten stehen in ROADMAP.md
unter Erledigt.
Format: `- JJJJ-MM-TT — Feature (1–2 Sätze: was und wo)`

- 2026-07-18 — MeshBuilder: statische Klasse im Uni-Repo
  (`Assets/Shared/MeshBuilder/`), baut aus quadratischer float[,]-Heightmap
  ein Unity-Mesh (Vertices, Triangles, Normals). Geprüft per Flach-,
  Random- und Rampen-Test.
- 2026-07-18 — TerrainPreview: Test-MonoBehaviour daneben — erzeugt
  Test-Heightmap, ruft `MeshBuilder.Build`, weist das Mesh dem
  MeshFilter zu.
- 2026-07-18 — HeightmapGenerator: statische Klasse
  (`Assets/Shared/MeshBuilder/`), erzeugt quadratische 0–1-Heightmap aus
  Perlin Noise mit Oktaven (persistence/lacunarity, Seed→Offsets,
  Normalisierung über Amplitudensumme). Geprüft per Determinismus-,
  Seed- und Oktaven-Test.
- 2026-07-18 — TerrainPreview erweitert: TestMode-Enum (Flat/Ramp als
  Regressions-Checks, Noise ruft den Generator), Noise- und
  Mesh-Parameter als Inspector-Felder mit [Min]/[Range]-Guards,
  Tooltips und Headern.
