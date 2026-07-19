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
- 2026-07-18 — [Architektur] TerrainConfig als ScriptableObject-Parameter-
  Object: alle Pipeline-Einstellwerte in einem Asset, Konsumenten lesen
  dieselbe Quelle (Get-only-Properties); mehrere Assets = tauschbare
  Presets ohne Code-Change. Generator-Signatur: Generate(TerrainConfig).
- 2026-07-18 — [Terrain] heightCurve (AnimationCurve): remappt das
  normalisierte 0–1-Profil per Evaluate — nach der Oktaven-Normalisierung,
  vor der Meter-Skalierung im MeshBuilder. x²-Intuition: jeder Wert „mal
  sich selbst" → Täler sacken ab (0,2→0,04), Gipfel bleiben (0,9→0,81),
  Terrain wird dramatischer. Verifiziert per Kurven-Biege-Test.
- 2026-07-18 — [Planung] Ziel-Bild des kombinierten Tools (PCG + Engine-
  Tool, siehe DECISIONS 2026-07-18): Terrain mit Bergen und Tälern,
  später evtl. Straßen/Wege; Start-Village mit Haus-Asset am Spawnpunkt;
  Bäume und Gras platzieren, an Flüssen passender Shader/Material;
  vorhandene Partikeleffekte (Glühwürmchen, Fackeln) an ausgewählten
  Stellen. Abgabe braucht zusätzlich: Tool-Beschreibung im TDD, UML-
  Klassendiagramm, Ablaufdiagramm, mind. ein Design Pattern, Fehlbedienung
  ausgeschlossen oder mit Nutzer-Feedback.
- 2026-07-18 — [Tools] Terrain-Editor-Tool als MVP: View TerrainToolWindow
  (EditorWindow, IMGUI), Presenter TerrainToolPresenter (prüft Config,
  ruft Pipeline, besitzt „Generated Terrain"-Objekt), Model = bestehende
  Pipeline unverändert. Fehlbedienung: Button-DisabledScope + HelpBox;
  Edit-Mode-Fallen: sharedMesh statt mesh, DestroyImmediate,
  [SerializeField] am Config-Feld (Serialisierungs-Bewertungspunkt).
  Liefert fürs TDD: Tool-Beschreibung + Stoff für UML-/Ablaufdiagramm.
- 2026-07-18 — [Tools] Tool fertig und getestet (6-Punkte-Plan inkl.
  Editor-Neustart). IMGUI-Kernmuster fürs TDD: Felder geben ihren neuen
  Wert zurück (`_config = ObjectField(...)`), Buttons feuern nur im
  Klick-Frame, DisabledScope sperrt per using-Klammer garantiert nur
  seinen Bereich. typeof als Typ-Filter des ObjectFields.
- 2026-07-18 — [Tools] URP-Lektion: Magenta = Shader inkompatibel mit
  aktiver Pipeline; Built-in-Materials (Default-Diffuse) funktionieren
  unter URP nicht → GraphicsSettings.currentRenderPipeline.defaultMaterial
  als Fallback, eigentliches Material kommt aus der Config.
- 2026-07-18 — [Tools] Bewusste Grenzen v1 (TDD-Kapitel „Erweiterungen"):
  kein Undo für Generate/Clear; GameObject.Find sieht nur aktive Objekte;
  Auto-Regenerate als Checkbox geparkt (Performance/Absichtsprinzip —
  siehe DECISIONS 2026-07-18).
- 2026-07-19 — [Terrain/Shader] Bekannte Grenze Wasser-Shader (TDD-Kapitel
  „Erweiterungen"): aktueller Shader läuft auf einer flachen Ebene in
  eine Richtung — passend für den geplanten globalen Wasserspiegel
  (Seen/Talwasser, siehe DECISIONS 2026-07-18), nicht für echte Flüsse
  mit Flussbett-Kurven. Für Flüsse bräuchte es ein eigenes, entlang eines
  Splines gebautes Mesh (Mesh-Baustein vor der Shader-Anpassung) — bleibt
  Kür nach der formativen Abgabe, kein Shader-Nachbessern.
- 2026-07-19 — [Terrain] PlateauModifier: Modifier-Stufe zwischen
  Generator und MeshBuilder, schreibt in-place in die Heightmap.
  Kernformeln: Position normiert auf 0–1 (x/(res−1), Cast Pflicht gegen
  Integer-Division), Distanz zum Center via Vector2.Distance; drei Fälle:
  innen → Zielhöhe, Ring → Lerp(PlateauHeight, original, t) mit
  t = (dist − radius)/blend, außen → unangetastet. Lerp-Richtung: t=0 an
  der Plateau-Kante = volle Plateauhöhe.
- 2026-07-19 — [Architektur] Nicht-destruktive Pipeline: Modifier
  schreiben vor dem Mesh-Build, danach ist die Heightmap read-only —
  das Mesh ist ein Schnappschuss, keine Live-Verbindung (zwei
  Wahrheitsquellen vermeiden). Änderungen gehen in die Quelldaten
  (Config/Modifier), dann läuft die Pipeline komplett neu; Determinismus
  (Seed) macht das Original jederzeit reproduzierbar — Löschen eines
  Modifiers = Eintrag streichen + neu generieren.
- 2026-07-19 — [Terrain] Plateau-Guards: radius Min(0) mit 0 als
  gewolltem Aus-Schalter (kein Extra-Bool); blend Min(0.001) sichert die
  Division in t ab — statt sich auf die Zweig-Reihenfolge zu verlassen
  (gleiche Linie wie noiseScale: falscher Wert entsteht gar nicht erst).
  Pipeline-Klassen loggen nicht (siehe DECISIONS 2026-07-19).
- 2026-07-19 — [Terrain] Chunk-Entscheidung: Welt 2048 m Kante, Start
  2 m/Quad → 8×8 Chunks à 129 Vertices (16.641/Chunk, ~1,06 Mio gesamt);
  1 m/Quad = 16×16 Chunks per Inspector. Begründung: 16-Bit-Limit
  65.535 Verts/Mesh (max. 255×255) hieße ~8 m/Quad bei 2 km — zu grob;
  Chunks liefern Culling + Thread-Granularität. Threading bewusst ans
  Ende: langsame Version = Baseline-Messdaten für die Threading-Abgabe.
- 2026-07-19 — [Terrain] Chunk-Nahtlosigkeit: Noise nach Weltposition
  sampeln, nie nach lokalem Chunk-Index — Nachbarränder fragen identische
  Weltkoordinaten ab → identische Höhen, Naht gratis. Bekannte Baustelle:
  RecalculateNormals kennt nur das eigene Chunk-Mesh → Beleuchtungsnaht;
  Lösung: Normalen aus der Heightmap rechnen, eine Reihe Überlappung.
- 2026-07-19 — [Terrain] Wasserspiegel-Design: waterLevel 0–1 (Vergleich
  nach der HeightCurve), Plane auf waterLevel × heightMultiplier mit
  eigenem Shader-Graph-Material; _waterEnabled-Bool; OnValidate-Warnung
  bei PlateauHeight <= WaterLevel; Margin für kahlen Uferstreifen als
  Platzierungs-Input (platzieren erst ab height > waterLevel + margin).
  0–1 statt Meter: Wasser skaliert mit der Karte, Uferlinie bleibt bei
  Multiplier-Änderungen exakt gleich.
- 2026-07-19 — [Terrain] Platzierung entkoppelt von Auflösung: Objekte
  stehen in Weltkoordinaten (beliebige floats), die Heightmap ist nur
  das interpolierte Höhen-Nachschlagewerk (Mischung der Nachbarpunkte,
  gewichtet nach Nähe). Auflösung bestimmt Bodenform-Detail und
  Steigungs-Glättung, nicht die Platzierungs-Präzision.
