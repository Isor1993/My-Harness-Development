# DECISIONS.md — Entscheidungen

Ownership: Entscheidungen: was, warum, verworfene Alternativen.
Format: `## JJJJ-MM-TT — Titel` mit Was / Warum / Verworfen, je 1–2 Zeilen.

## 2026-07-16 — Brainstorm-Modus normal/uni
Was: Jede Brainstorm-Session startet mit der Modus-Abfrage normal oder uni.
Warum: Uni-Modus braucht eigene Regeln — erklären statt bauen, visuell,
Verständnis-Checks, Knowledge-Pflicht.
Verworfen: ein Einheitsmodus für alle Brainstorm-Sessions.

## 2026-07-17 — Knowledge als externer Ordner
Was: Wissensarchiv als Ordner `C:\Repos Isor\Knowledge\` mit
Themen-Unterordnern, eine .md pro Konzept, Offline-Kopien der
Artifact-Seiten unter Seiten/.
Warum: Wissen ist projektübergreifend und überlebt das Projekt; eine
einzelne Datei wäre unübersichtlich gewachsen.
Verworfen: einzelne KNOWLEDGE.md im Projekt als Puffer mit Auslagerung.

## 2026-07-17 — CODE_GUIDELINES: Zwei Blöcke + Projekt-Typ
Was: Block 1 = Uni-Pflicht (Stil/Naming), Block 2 = eigene Architektur-
Regeln; `Projekt-Typ: Uni/Privat` im Dateikopf entscheidet den
Konfliktfall, einmalig beim Projekt-Setup gesetzt.
Warum: Uni-Regeln sind Pflicht, sollen aber nach dem Studium sauber
entfernbar sein; Projekt-Typ ändert sich nie mitten im Projekt.
Verworfen: Session-Abfrage des Typs durch den Harness; ein gemischter
Regelblock ohne Herkunfts-Markierung.

## 2026-07-17 — Felder: [SerializeField] private statt public
Was: Default `[SerializeField] private`, `_camelCase` überall; public nur
bewusst und dann als Property.
Warum: Inspector-Wiring funktioniert identisch, Compiler schützt vor
Fremdzugriff; Dozenten-public diente nur der Bequemlichkeit seines
Editor-Setup-Scripts. Deckt sich mit Uni-Regel (Member private/protected).
Verworfen: Dozenten-Default „public fürs Wiring"; sein Underscore-Verbot.

## 2026-07-17 — Dozenten-Patterns als Werkzeugkasten, nicht Pflicht
Was: MVP als Default-Denkmodell (mit Pragmatik-Ausnahmen); SO-Identität,
Event-Channels, RuntimeReference nur bei passender Problemform.
Review-Gate übernommen (angepasst: Isor tippt selbst, Claude prüft mit);
ClaudeSetup zurückgestellt.
Warum: Entspricht den Einsatzkriterien des Dozenten selbst; Isor ist in
der Lernphase und will selbst schreiben, um besser zu werden.
Verworfen: Patterns als Pflicht für jedes System; ClaudeSetup jetzt schon.

## 2026-07-17 — Session-Ende-Rituale erweitert
Was: Knowledge-Abfrage in jeder Session (Uni-Modus: Pflicht);
Commit-Vorschlag durch Claude (Titel `Update V <nächste Nummer>` +
Description), Isor committet selbst; TDD_NOTES.md als Stoffsammlung
fürs Uni-TDD — nur echte Uni-Projekt-Arbeit, keine Harness-Arbeit.
Warum: Erkenntnisse und Commit-Historie gingen sonst beim /clear
verloren; Isor will Commit-Texte nicht selbst formulieren; das TDD ist
Uni-Abgabe (ca. 2026-07-28) und braucht laufendes Rohmaterial.
Verworfen: Knowledge-Abfrage nur im Uni-Modus; Commits durch Claude.

## 2026-07-17 — Sprache: Code englisch, Unterhaltung deutsch
Was: Code, Kommentare, Debug-Ausgaben und Commit-Messages ausnahmslos
Englisch; Harness-Doku und Unterhaltung mit Claude vorerst Deutsch.
Warum: GitHub-Repo ist englischsprachig, Fachbegriffe bleiben konsistent
mit der Unity-/C#-Welt; Deutsch hilft beim Lernen.
Verworfen: Sprachwahl je Situation; sofortige Englisch-Umstellung des
Harness (geparkt in ROADMAP „Später").

## 2026-07-18 — Terrain-Mesh komplett im Code
Was: Kein Unity-Terrain und kein vormodelliertes Mesh-Asset — MeshBuilder
erzeugt das Mesh vollständig aus der Heightmap.
Warum: Höhen ändern sich pro Seed, Größe/Auflösung kommen aus der Config —
ein Asset müsste trotzdem per Code verformt werden; MeshCollider braucht
passende Geometrie; Mesh-Generierung ist Kern der Uni-Aufgabe.
Verworfen: Unity-Terrain; unterteilte Plane als Basis-Asset.

## 2026-07-18 — Heightmap-Konvention: [x, z], quadratisch, ein int
Was: Erster Index = x, zweiter = z — projektweit für alle Heightmap-Zugriffe;
Auflösung als einzelner int statt Vector2Int.
Warum: Rampen-Test zeigte 90°-gedrehtes Terrain, weil Schreiber [z, x] und
Leser [x, z] benutzten; MeshBuilder setzt Quadrate voraus — ungültige
Rechteck-Auflösungen sollen gar nicht erst einstellbar sein.
Verworfen: freie Index-Reihenfolge pro Klasse; Vector2Int-Resolution.

## 2026-07-18 — Reine statische Utilities erlaubt
Was: MeshBuilder und HeightmapGenerator sind statische Klassen — die
Regel „Keine Singletons/Statics" meint zustandsbehaftete Statics.
Warum: Beide sind reine Funktionen ohne Zustand (Daten rein, Daten raus,
testbar); eine Instanz hätte keinen Mehrwert.
Verworfen: Instanz-Klassen hinter Interface (kein zweiter Use-Case — YAGNI).

## 2026-07-18 — Wertebereiche an der Eingabe statt Laufzeit-Checks
Was: Ungültige Parameter (resolution < 2, noiseScale 0, octaves 0,
lacunarity < 1) verhindern [Min]/[Range] im Inspector; der Generator
prüft nicht selbst. Jedes Feld bekommt zudem einen sinnvollen Default.
Warum: Der falsche Wert soll gar nicht erst entstehen (gleiche Denke wie
int statt Vector2Int); fast alle Grenzen schützen vor Division durch
null. Defaults nötig, weil [Min] still gespeicherte Werte nicht korrigiert.
Verworfen: Guard-Klauseln/Exceptions im Generator.

## 2026-07-18 — TerrainConfig flach, nur lesbar, Guard beim Aufrufer
Was: TerrainConfig als ein flaches SO ([Header]-Gruppen, kein Nesting),
Zugriff nur über Get-only-Properties (`=> _feld`); die Null-Prüfung der
Referenz macht der Aufrufer (TerrainPreview in Awake und Start),
nicht der Generator. MeshBuilder bleibt config-agnostisch (Primitive).
Warum: Nesting/Setter ohne aktuellen Nutzen (YAGNI); Schreibzugriffe
würden im Editor das Asset dauerhaft ändern; ein Guard im Generator käme
nach dem ersten Zugriff zu spät und ein Ersatz-Array würde den Fehler
verstecken — beim Aufrufer sitzt die vergessbare Inspector-Verdrahtung.
Verworfen: verschachtelte [Serializable]-Unterklassen (Skizze vom
16.07.), public Felder/Setter, Guard mit 1×1-Fallback im Generator.

## 2026-07-18 — PCG- und Engine-Tool-Aufgabe kombiniert
Was: Beide Uni-Aufgaben (ASSIGNMENT_PCG.md + ASSIGNMENT_TOOL.md) werden
mit einem Projekt erfüllt: die Terrain-Pipeline als prozedurale
Level-Generierung (PCG) plus ein Editor-Tool mit UI und mindestens einem
Design Pattern obendrauf (Engine-Tool). Bestückung nutzt vorhandene
Inhalte aus früheren Abgaben (Shader, Partikeleffekte wie
Glühwürmchen/Fackeln, vorhandene KI) und leicht beschaffbare Assets
(Haus fürs Start-Village, Bäume, Gras).
Warum: Spart Zeit, ergibt ein zusammenhängendes Spielprojekt; die
Aufgabenstellung erlaubt eigene Anwendungsfälle, Spawn-Inhalte sind frei
wählbar.
Verworfen: zwei getrennte Tools/Abgaben ohne gemeinsame Basis.

## 2026-07-18 — Editor-Tool vor Platzierungs-Stufe
Was: Reihenfolge bis Ende Juli: 1. minimales Editor-Tool, 2. Platzierung
(Bäume/Gras → Village → Partikel), 3. Texturierung (erster
Streichkandidat), 4. TDD + UML. Große Bausteine liegen auf Isors
Wochenend-Fenstern.
Warum: Das Tool beschleunigt jede spätere Iteration (Generieren im Edit
Mode statt Play Mode), die Tool-Aufgabe ist formativ schon überfällig
(Feedback einholbar), und es wächst mit jeder Stufe mit.
Verworfen: erst Platzierung fertig bauen, dann das Tool drumherum.

## 2026-07-18 — Terrain-Tool als MVP mit EditorWindow (IMGUI)
Was: View `TerrainToolWindow` (EditorWindow, OnGUI/IMGUI), Presenter
`TerrainToolPresenter` (einzige Tool-Logik: prüfen, Pipeline rufen,
Terrain-Objekt besitzen), Model = bestehende Pipeline unverändert.
Beides in `Assets/Shared/MeshBuilder/Editor/`. Fehlbedienung:
DisabledScope + HelpBox ohne Config; Neu-Generieren ersetzt das Objekt.
Warum: Aufgabe verlangt ein MV-Pattern an echter Stelle; Einbahnstraßen-
Abhängigkeit hält das Model testbar; IMGUI ist für ein kleines Fenster
der einfachste Weg.
Verworfen: UI Toolkit (mehr Boilerplate ohne Nutzen bei dieser Größe);
Logik direkt im Fenster (Pattern wäre nur Etikett).

## 2026-07-18 — Terrain-Material wohnt in der Config
Was: TerrainConfig hält das Terrain-Material (leer = Default der aktiven
Render-Pipeline); der Presenter weist es bei jedem Generate zu, nicht
nur beim Anlegen des Objekts.
Warum: Material ist eine Preset-Eigenschaft wie Seed/Größe — Config-
Tausch wechselt Terrain und Look gemeinsam; Zuweisung pro Generate,
damit Änderungen ohne Clear sichtbar werden. Built-in-Default-Diffuse
rendert unter URP magenta (Signalfarbe für inkompatiblen Shader).
Verworfen: Material-Feld im Tool-Fenster (zweite Quelle neben der
Config); hartes Default-Diffuse; Auto-Regenerieren bei Config-Änderung
(geparkt als spätere Checkbox — Performance beim Slider-Ziehen,
Absichtsprinzip).

## 2026-07-18 — Platzierungs-Reihenfolge: Formen vor Reagieren
Was: Schichten-Prinzip — erst was das Gelände formt, dann was darauf
reagiert: 1. Plateau-Modifier (Village-Fläche), 2. Wasserspiegel
(waterLevel + Ebene statt gegrabener Flüsse), 3. Haus/Village aufs
Plateau, 4. Bepflanzung mit Regeln (Höhe/Steigung/Wassernähe/nicht auf
Plateau), 5. Kür: echte Flüsse, Wege. Modifier schreiben in die
Heightmap, Platzierung liest nur.
Warum: Regeln wie „Gras am Wasser" brauchen Wasser als Input — von Isor
selbst hergeleitet. Wasserspiegel liefert „wo ist Wasser" für einen
Bruchteil der Kosten von Fluss-Carving; Straßen sind das Schwerste und
stehen nicht in den Pflicht-Features.
Verworfen: Bepflanzung zuerst (alte Empfehlung); Flüsse graben vor der
Abgabe; Straßen als fester Bestandteil.

## 2026-07-19 — Asset-Ordner: Kategorie + FolderTemplate, Shared nur bei mehreren Abnehmern
Was: Bausteine liegen unter `Entities/`, `Environment/` oder `Systems/`
je in eigenem Ordner mit Template-Unterordnern (nur die benötigten);
`Shared/` nur für Querschnitts-Utilities mit mehreren Abnehmern. Regeln
in CODE_GUIDELINES (Ordnerstruktur); Terrain-Pipeline entsprechend nach
`Systems/TerrainGenerator` umgezogen.
Warum: `Shared/MeshBuilder` war zum Feature-Ordner gewachsen — falsches
Etikett; Isors Template-System existierte schon und funktioniert.
Verworfen: alte Guideline-Zeile `Assets/Scripts/{System}/`; MeshBuilder
in Shared lassen (nur ein Abnehmer — Zusammenhalten schlägt
spekulatives Teilen).

## 2026-07-19 — Pipeline-Klassen loggen nicht
Was: Reine Pipeline-Funktionen (Generator, Modifier, MeshBuilder)
bleiben still — kein Debug.Log, keine Warnings; Nutzer-Feedback ist
Sache der Presenter-Schicht (StatusMessage im Tool).
Warum: Radius 0 ist gewollter Aus-Schalter, kein Fehler; Logs in
Zellen-Schleifen wären Spam; was nie geloggt wird, muss für die
Uni-Abgabe nie entfernt werden.
Verworfen: Warning bei Radius 0; Debug.Logs mit späterem Ausbau.

## 2026-07-19 — Session-Typen: Brainstorm+Design ein Typ, 1:1-Regel
Was: „Brainstorm/Design" ersetzt die zwei getrennten Typen; pro Baustein
gilt: erst eine Brainstorm/Design-Session (was & wie), dann eine
Development-Session (nur Umsetzung). Eine Design-Session darf mehrere
Bausteine vorentscheiden.
Warum: Design ohne Brainstorm-Anteil kam in der Praxis nie vor; die feste
Reihenfolge gibt Isor einen klaren Schnitt zwischen Entscheiden und Bauen.
Verworfen: vier getrennte Typen; freies Mischen von Design und Umsetzung
in einer Session.

## 2026-07-19 — Chunk-Terrain: 2048 m, Start 2 m/Quad, 8×8 à 129
Was: Terrain wird in Chunks gebaut; Config bekommt chunksPerEdge +
chunkResolution statt heightmapResolution. Welt 2048 m Kante, Start-
Detail 2 m/Quad (8×8 Chunks à 129 Vertices); 1 m/Quad (16×16) bleibt
reiner Inspector-Wechsel und wird visuell entschieden. Noise wird nach
Weltposition gesampelt (nahtlose Chunks); Chunks rechnen unabhängig
(thread-tauglich), optimiert wird erst am Schluss — die langsame Version
ist die Baseline-Messung für die Threading-Abgabe (formativ 2026-08-07).
Warum: 16-Bit-Meshes enden bei 65.535 Vertices — ein Einzelmesh kann
2 km nicht in Gameplay-Detail (1–2 m/Quad) darstellen; Chunks liefern
zudem Culling-Granularität. Platzierung erzwingt kein feines Raster:
Objekte stehen in Weltkoordinaten, die Heightmap wird interpoliert
abgefragt.
Verworfen: Einzelmesh mit 32-Bit-Indizes (kein Culling, alles-oder-
nichts-Regenerierung); 1 m/Quad sofort (4× Kosten bei jedem Tuning-
Klick ohne sichtbaren Mehrwert); Threading jetzt (Messdaten fehlten).

## 2026-07-19 — Wasserspiegel: Einheit, Schalter, Darstellung
Was: waterLevel normalisiert 0–1 (verglichen nach der HeightCurve),
_waterEnabled als explizites Bool, Darstellung als eine Plane auf
waterLevel × heightMultiplier mit Isors vorhandenem Shader-Graph-
Wassershader (_waterMaterial), OnValidate-Warnung wenn
PlateauHeight <= WaterLevel, Uferstreifen-Margin kommt mit in die
Config (visuell bewertet erst mit der Platzierungs-Stufe). Ergänzt
DECISIONS 2026-07-18 „Formen vor Reagieren".
Warum: 0–1 hält das Wasser im Verhältnis zur Karte — Multiplier-
Änderungen verschieben die Uferlinie nicht; das Bool erhält den
eingestellten Level beim Ausschalten; der eigene Shader existiert und
passt (Schaum zeichnet die Uferlinie).
Verworfen: Meter-Wert (Seen schrumpfen beim Hochskalieren); 0-als-Aus-
Konvention wie beim Plateau (Wert ginge beim Ausschalten verloren);
gekaufte Fluss-Assets für die Abgabe (bewertet wird eigener Code;
Flüsse sind eine eigene Pipeline-Stufe → Kür nach dem Portfolio).

## 2026-07-17 — Minimalistisch zur Einsatzreife
Was: Alle vier Session-Typen nur minimal definiert; ausgearbeitet wird
erst, wenn der Praxisbetrieb es verlangt. Regel-Dateien beschreiben nur
den Ist-Zustand, Begründungen gehören hierher.
Warum: Uni-Projekt startet 2026-07-18 — funktionstüchtig schlägt
vollständig.
Verworfen: volle Ausarbeitung aller Dokumente vor Praxisstart
(alte Roadmap-Reihenfolge).

## 2026-07-19 — HeightCurve clampen, Octave-Offsets begrenzen
Was: Generator wickelt Evaluate in Mathf.Clamp01; Octave-Offsets laufen nur
noch ±10000 (benannte Konstante MaxOctaveOffset) statt ±100000.
Warum: Weiche Kurven-Tangenten schwingen zwischen Keys unter 0/über 1 durch —
per Attribut nicht begrenzbar (bewusste Ausnahme zu „Wertebereiche an der
Eingabe", 2026-07-18), daher Laufzeit-Clamp. Große Offsets sampeln Perlin bei
~100000, wo float gröber auflöst als der 2-m-Vertexschritt → identische
Nachbarwerte → Terrassen bei Auflösung 129 (nicht bei 40).
Verworfen: Kurve ungeclampt lassen; Offsets bei ±100000 (Terracing).

## 2026-07-19 — Nahtlose Normalen über Padding-Ring statt RecalculateNormals
Was: HeightmapGenerator gibt die Heightmap um 1 Vertex gepaddet zurück (Ring =
Nachbarhöhen, nicht im Mesh); MeshBuilder baut nur das Innere und rechnet
Normalen analytisch aus Nachbarhöhen (zentrale Differenz). Mapping: Array-/
Schleifengröße = ChunkResolution+2, Weltmapping bleibt ChunkResolution−1 mit
(index−1)-Versatz.
Warum: RecalculateNormals kennt nur das eigene Chunk-Mesh → doppelte Rand-
Vertices bekommen verschiedene Normalen → Beleuchtungsnaht. Der Ring liefert
beiden Chunks identische Nachbarhöhen an der geteilten Kante → identische
Normale, deterministisch, lokal (kein Cross-Chunk-Nachbearbeiten). Ergänzt
DECISIONS 2026-07-19 „Chunk-Nahtlosigkeit" (Geometrie war schon nahtlos, jetzt
auch die Beleuchtung).
Verworfen: Rand-Normalen nachträglich über alle Meshes mitteln (Cross-Chunk-
Pass, Float-Positionsabgleich, fehleranfällig); Einzelmesh mit 32-Bit-Indizes
(kein Culling).

## 2026-07-21 — Platzierungs-Stufe: reine Stufe, zwei Listen, globales Poisson
Was: ObjectPlacer als reine statische Stufe (Geschwister zu MeshBuilder, liest
nur, fasst die Szene nicht an). Zwei Listen: Placeable (`[Serializable] class`,
Array in TerrainConfig — prefab, min/maxHeight, maxSlope, minSpacing,
scaleMin/Max, alignToGround, DensityStrategy) → Placement (`struct`, tausendfach
— prefab, position, rotation, scale). Der Presenter instanziiert. Verteilung:
globales Poisson-Disc über die ganze Karte, Ergebnis pro Chunk einsortiert.
Warum: hält den Pipeline-Stil (reine Daten-Stufe, nur der Presenter mutiert die
Szene); Poisson garantiert den Mindestabstand (Isors Kernanforderung); global,
weil Poissons Garantie global ist und per-Chunk am Nahtrand wieder Berührungen
erzeugt; struct spart Garbage bei tausenden Einträgen, class nötig für den
Inspector. Konkretisiert DECISIONS 2026-07-18 „Formen vor Reagieren".
Verworfen: per-Chunk-Poisson (Naht-Abstand nur mit Rand-Check, mehr Code);
Raster+Versatz (kein garantierter Abstand); reiner Zufall (klumpt); prefab per
Index statt direkt (YAGNI); gespeicherte Placement-Liste als Asset.

## 2026-07-21 — Höhe/Steigung für die Platzierung: gemeinsame SampleHeight-Funktion
Was: „Höhe an Weltposition" wird als `SampleHeight(config, x, z)` aus dem
HeightmapGenerator herausgezogen (Noise → Curve → Plateau an einem Punkt);
Chunk-Schleife und Placer nutzen dieselbe Funktion. Steigung und Ausrichtungs-
Normale kommen aus vier Nachbar-Samples (zentrale Differenz wie im MeshBuilder)
— eine Rechnung, zwei Zwecke. Der Placer braucht das Mesh nicht.
Warum: eine Quelle der Wahrheit statt Doppel-Logik; Funktion statt gespeichertem
Gitter, weil der Placer nur verstreut abfragt (~9.600 Punkte gegen ~1,05 Mio
Gitterzellen) und ein Cache bei Config-Änderung veraltet; Funktion passt zum
stateless-pure Stil (Generator/MeshBuilder halten keinen Zustand).
Verworfen: eigenes globales Höhen-Gitter für die Platzierung (Doppelrechnung,
Veraltungs-Risiko, einziger dauerhafter Zustand — Fremdkörper); Mesh-Faces
abfragen für die Ausrichtung (Normale gibt es analytisch aus SampleHeight).

## 2026-07-21 — Platzierung: ein Poisson-Durchgang pro Typ, Reihenfolge=Priorität, Regel-Filter
Was: ein Poisson-Durchgang je Placeable-Typ (eigener minSpacing-Radius);
Reihenfolge in Liste 1 = Priorität. Jeder Durchgang nimmt eine Blocker-Liste
entgegen (jetzt leer → Bäume/Gras unabhängig; später legt der Dorf-Schritt die
Haus-Grundrisse hinein → Gras/Bäume meiden Häuser, ohne Umbau). Regel-Filter je
Kandidat, billig→teuer: Wasser-Untergrenze (global, nur wenn isWaterEnabled,
height ≥ waterLevel + shoreMargin) → Höhenband (max(Wasser, minHeight)…maxHeight)
→ Steigung (≤ maxSlope). Alle bestanden → Placement.
Warum: derselbe Poisson-Abstandsmechanismus deckt auch den Inter-Typ-Ausschluss
(Blocker als Fremd-Punkte) — kein Sonderfall; Reihenfolge billig→teuer spart die
teure Steigungsrechnung bei früh abgelehnten Punkten; Wasser-Regel global, weil
die Uferlinie eine Welt-Eigenschaft ist. Nutzt shoreMargin aus DECISIONS
2026-07-19 „Wasserspiegel".
Verworfen: fest verdrahtete Inter-Typ-Ausschlüsse / Ausschluss-Matrix jetzt
(YAGNI); Wasser-Regel pro Placeable-Zeile.

## 2026-07-21 — Dichte-Steuerung als austauschbare DensityStrategy (Strategy-Pattern)
Was: „wo viel/wenig" als Wahrscheinlichkeits-Ausdünnung statt variablem Poisson-
Radius. Abstrakte `DensityStrategy` (ScriptableObject) mit
`AcceptanceProbability(x, z) → 0–1`; drei Start-Assets: Uniform (immer 1),
NoiseMask (Rausch-Wert, eigener Seed/Scale), Probability (feste Zahl). Feld pro
Placeable, leer = Uniform. Der Würfel-Wurf (`random < p`) lebt einmal im Placer,
die Strategie liefert nur p.
Warum: fester-Radius-Poisson + Ausdünnung ist eine simple 4. Filterstufe;
variabler-Radius-Poisson bricht die Gitter-Beschleunigung (fummelig); Strategy
ist offen für neue Arten ohne Placer-Änderung; NoiseMask als eigenes Asset →
mehrere Masken frei kombinierbar. Erfüllt zugleich das Design-Pattern-Kriterium
der Tool-Aufgabe (zweites Muster neben MVP).
Verworfen: variabler-Radius-Poisson; enum + switch (nicht ohne Code
erweiterbar); Strategie global statt pro Typ.

## 2026-07-21 — Tool-Panel, Prefabs, placementSeed
Was: reiches Editor-Panel — „Generate Complete" (alles in Prioritätsordnung),
Einzel-Stufen (Terrain / Wasser / Place Objects), plus pro-Typ „Place"/„Clear"
**aus Liste 1 erzeugt** (nicht fest verdrahtet). Eigene „Generated Placement"-
Wurzel unter dem Terrain-Root; „Place Objects" läuft ohne Terrain-Rebuild
(schnelles Tunen), Terrain-Regenerieren räumt die Platzierung als veraltet weg.
Prefabs kunst-blind (Referenz in Placeable; Platzhalter-Primitive jetzt, echte
Assets später ohne Code-Change). Gras-Masse-Rendering im Presenter kapselbar
(GameObjects jetzt → GPU-Instancing/Details später), aufgeschoben inkl.
Praxis-Test Prefab-Gras vs. Detail. Eigener `placementSeed` (getrennt vom
Terrain-Seed → Verteilung neu würfeln ohne Rebuild), pro Typ abgeleitet, alles
Zufällige deterministisch.
Warum: Aufgabe belohnt umfangreiche Tools; Einzel-Stufen helfen beim Bauen/
Debuggen; pro-Typ aus der Liste bleibt datengetrieben (neuer Typ = automatisch
Buttons); getrennter Seed löst die vorgemerkte „inkrementell generieren"-Frage;
Rendering-Tausch hinter dem Presenter hält die Platzierungs-Logik unberührt.
Verworfen: Tree/Grass fest verdrahtete Buttons; Auto-Platzierung bei jedem
Terrain-Generate; Platzierung an den Terrain-Seed gekoppelt.

## 2026-07-23 — SampleHeight komponiert, PlateauModifier bleibt eigener Job
Was: Die geteilte Höhen-Funktion `SampleHeight` ist ein dünner Komponist
(Noise+Curve, dann `PlateauModifier.SampleAt`); die Plateau-Logik bleibt in
`PlateauModifier` (jetzt per-Punkt statt Array), nicht inline in `SampleHeight`
gefaltet. Setzt DECISIONS 2026-07-21 „gemeinsame SampleHeight-Funktion" um.
Warum: DRY (eine Wahrheit, ein Einstiegspunkt) und SRP (ein Job je Klasse)
beißen sich nur bei Inline-Faltung; Trennung nach Job + Teilen durch Komposition
erfüllt beide, kein Gott-Objekt. Von Isor selbst als Spannung erkannt.
Verworfen: Plateau-Rechnung inline in `SampleHeight` (SRP-Verstoß);
`PlateauModifier` löschen; ihn behalten UND Plateau in `SampleHeight`
duplizieren (DRY-Verstoß).

## 2026-07-23 — Kommentar-Konventionen geschärft
Was: XML-Docs mehrzeilig (IDE-Standard, auch Properties); Inline-Kommentare
Default = keiner, sonst einzeilig und Warum-only; Debug-Ausgaben in
`#if UNITY_EDITOR`; Felder ohne `<summary>` (serialisiert → `[Tooltip]`).
Festgehalten in CODE_GUIDELINES.
Warum: benotete Abgabe — geschwätzige/AI-riechende Kommentare sind ein Risiko;
Dozenten-Regel „keine Debug-Logs im Build"; das Warum lebt in DECISIONS/Header,
nicht inline. Aus Isors Praxis-Feedback in Session 2026-07-23.
Verworfen: einzeilige XML-Summaries; der „2–3 Zeilen erlaubt"-Inline-Zusatz
(führte zu Über-Kommentierung).
