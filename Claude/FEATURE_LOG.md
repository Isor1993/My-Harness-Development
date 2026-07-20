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
- 2026-07-18 — TerrainConfig: ScriptableObject-Parameter-Object
  (`Assets/Shared/MeshBuilder/`) mit allen Pipeline-Einstellwerten
  (Heightmap/Noise/Mesh) plus neuer heightCurve (AnimationCurve, formt
  das 0–1-Höhenprofil nach der Normalisierung um). Generator nimmt jetzt
  die Config statt sechs Einzelparameter, TerrainPreview hält nur noch
  TestMode + Config-Referenz (mit Null-Guards in Awake und Start).
  Geprüft: alle drei TestModes unverändert, Kurven-Biege-Test (Täler
  sichtbar flacher), Guard-Test.
- 2026-07-18 — Terrain-Editor-Tool (MVP): TerrainToolWindow (EditorWindow,
  Tools → Terrain Generator) + TerrainToolPresenter
  (`Assets/Shared/MeshBuilder/Editor/`) — generiert, ersetzt und löscht
  das Terrain im Edit Mode aus der TerrainConfig; Fehlbedienung per
  DisabledScope + HelpBox abgefangen. Geprüft: 6-Punkte-Testplan inkl.
  Ersetzen bei Seed-Wechsel und Editor-Neustart (Config-Referenz bleibt).
- 2026-07-18 — TerrainConfig um Terrain-Material erweitert (leer =
  Default-Material der aktiven Render-Pipeline); der Presenter weist es
  bei jedem Generate zu, damit Material-Wechsel sofort greifen. Geprüft
  mit eigenem URP-Lit-Material (Smoothness runter gegen Plastik-Look).
- 2026-07-19 — Asset-Ordnerstruktur aufgeräumt: Terrain-Pipeline von
  `Shared/MeshBuilder` nach `Systems/TerrainGenerator` (Scripts/Editor/
  Materials/SO_Settings), Environment/ParticleEffects-Umbenennungen,
  TutorialInfo entfernt. Umzug im Unity-Editor, Referenzen geprüft.
- 2026-07-19 — PlateauModifier: statische Modifier-Stufe zwischen
  HeightmapGenerator und MeshBuilder (`Systems/TerrainGenerator/Scripts/`),
  plättet eine kreisförmige Fläche auf Zielhöhe mit Blend-Ring;
  Radius 0 = aus. Plateau-Parameter als neue Config-Gruppe, im
  Tool-Presenter eingehängt. Geprüft per 7-Punkte-Plan (Regression
  Radius 0, Center-Verschiebung, Height-Extreme, Blend-Kante, Seed-Test).
- 2026-07-19 — Chunk-Umbau der Terrain-Pipeline: Welt 2048 m als 8×8
  Chunks à 129 Vertices (2 m/Quad; 16×16 = 1 m/Quad rein per Inspector).
  TerrainConfig mit chunksPerEdge/chunkResolution statt
  heightmapResolution plus abgeleiteten Properties (MetersPerQuad,
  ChunkSizeInMeters); Generator und PlateauModifier rechnen pro Chunk
  und sampeln nach Weltposition (nahtlos, thread-tauglich); Presenter
  baut pro Generate einen Terrain-Root mit einem Kind-Objekt je Chunk
  komplett neu. Geprüft visuell: keine Geometrie-Nähte im Wireframe und
  Game View, Plateau global korrekt platziert.
- 2026-07-19 — TerrainPreview gelöscht (samt Szenen-Objekt): der
  Test-Treiber ist durch das Editor-Tool abgelöst.
- 2026-07-19 — Terrain-Feinschliff (Scripts/): HeightCurve-Ergebnis auf
  0–1 geclamped (Overshoot konnte unter den Weltboden ziehen); Octave-
  Offsets auf ±10000 begrenzt (benannte Konstante MaxOctaveOffset) gegen
  Float-Präzisions-Terrassen bei feiner Auflösung. Visuell verifiziert:
  glatte Auflösung 129 ohne Riefen.
- 2026-07-19 — Nahtlose Chunk-Normalen: HeightmapGenerator liefert die
  Heightmap mit 1-Vertex-Randring (Weltpositions-Sampling der Nachbarzelle),
  MeshBuilder rechnet Normalen analytisch aus den Nachbarhöhen (zentrale
  Differenz) statt RecalculateNormals — geteilte Chunk-Kanten bekommen
  dieselbe Normale, Beleuchtungsnähte verschwinden. PlateauModifier auf das
  Padding-Mapping nachgezogen, Blend-Ring auf Mathf.SmoothStep umgestellt
  (kein Knick). Visuell im Streiflicht verifiziert: keine Nähte.
- 2026-07-20 — Wasserspiegel: TerrainConfig um Water-Gruppe erweitert
  (isWaterEnabled, waterLevel 0–1, shoreMargin, waterMaterial) plus
  OnValidate-Warnung, wenn ein aktives Plateau auf/unter dem Wasserstand
  läge (Dorf säuft ab). TerrainToolPresenter baut bei aktivem Wasser eine
  Plane als Kind des Terrain-Roots (BuildWaterPlane): auf die Karte
  skaliert, mittig, auf waterLevel × heightMultiplier gehoben, Material
  mit Pipeline-Default-Fallback, MeshCollider entfernt. Visuell verifiziert:
  Seen füllen die Täler, Shader-Schaum zeichnet die Uferlinie sauber nach.
