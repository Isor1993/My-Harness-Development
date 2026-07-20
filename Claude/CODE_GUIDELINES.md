# CODE_GUIDELINES.md — Code-Konventionen

Ownership: Code-Konventionen: Namen, Architektur, Tests.
Status: Rohmaterial aus Brainstorm 2026-07-17 — beim ersten
Development-Einsatz in der Praxis nachschärfen.

## Priorität
Projekt-Typ: **Uni** (wird beim Kopieren des Harness pro Projekt einmalig
gesetzt: Uni oder Privat — keine Abfrage pro Session).
Bei Uni-Projekten gewinnt Block 1 im Konfliktfall, bei privaten Block 2.

## Block 1 — Stil & Naming (Uni-Pflicht, SAE-Conventions Stand 12/2024)
1. Code ausschließlich Englisch. Kommentare und Ausgaben Englisch oder
   Deutsch — aber einheitlich.
2. Kommentare erklären das Warum, nicht das Was. XML-`<summary>` für
   Methoden erwünscht.
3. Bezeichner aussagekräftig: nur ASCII, keine unbekannten Abkürzungen,
   kein `__`-Beginn, mehr als ein Buchstabe (Ausnahme: Schleifenzähler).
   Methoden = Verb + Nomen (`CalculateHeight`).
4. camelCase für Parameter und lokale Variablen. Private Felder:
   `_camelCase`. Booleans als Frage (`isRunning`, `hasTarget`, `canJump`).
5. PascalCase für Klassen, Structs, Interfaces, Methoden, Enums,
   Konstanten, Properties. Interfaces mit `I`-Präfix. Enum-Typen Singular,
   geflaggte Enums Plural. Callbacks beginnen mit `On`/`Handle`.
6. Format: eine Anweisung pro Zeile (einzeiliger Scope hinter der
   Bedingung erlaubt). Geschweifte Klammern je eigene Zeile (Allman);
   Properties dürfen einzeilig sein.
7. Member-Variablen private oder protected — Zugriff von außen über
   Properties. Keine ungarische Notation.
8. Namespaces und Ordner strukturieren das Projekt.
9. Nicht Spezifiziertes: Microsoft-C#-Konventionen bzw. Unity-Style-Guide.

## Kommentare & Datei-Header (Isors Standard)
Gilt überall, Uni wie privat. Code, Kommentare und Ausgaben: **immer
Englisch** — keine Ausnahme; erfüllt damit die SAE-Kommentar-Regeln
(Block 1, Regel 1–2). Deutsch bleibt nur der Unterhaltung mit Claude
vorbehalten.

Arbeitsteilung: Isor tippt Code ohne Kommentare; Header, Summaries und
Kommentare ergänzt Claude automatisch beim Review bzw. wenn Code
geschrieben wurde (vereinbart 2026-07-18).

### Datei-Header
Jede .cs-Datei beginnt mit diesem Block:

    /*****************************************************************************
    * Project : <Unity-Projektname>
    * File    : <Dateiname>.cs
    * Date    : <TT.MM.JJJJ — Erstelldatum>
    * Author  : Eric Rosenberg
    *
    * Description :
    * <Was die Klasse macht und wofür sie zuständig ist, 2–5 Zeilen.>
    *
    * History :
    * <TT.MM.JJJJ> ER Created
    ******************************************************************************/

History: pro nennenswerter Änderung eine neue Zeile `<Datum> ER <Was>`.

### Summaries
- XML-`<summary>` über jeder Klasse, jedem Enum, jeder Methode und
  jedem public Property/Event.
- Ausnahme: Unitys Standard-Eventmethoden (`Awake`, `Start`, `Update`,
  `OnEnable` …) brauchen keine.
- Methoden mit Parametern oder Rückgabewert: `<param>` / `<returns>`
  ergänzen.

### Inspector-Felder
- Jedes serialisierte Feld bekommt ein `[Tooltip]` — was es ist und
  wofür es benutzt wird.
- Zusammengehörige Felder mit `[Header("...")]` gruppieren; Wertebereiche
  mit `[Range]` o. Ä. absichern. Ziel: Der Inspector ist ohne Blick in
  den Code verständlich.

## Block 2 — Architektur & Unity-Praxis (eigene Auswahl)
Quelle: Code-Rules des Dozenten (v2.2), gefiltert im Brainstorm 2026-07-17.

### Felder & Kapselung
- Default: `[SerializeField] private` fürs Inspector-Wiring — keine
  public Felder (deckt sich mit Block 1 Regel 7).
- Keine Singletons/Statics. Abhängigkeiten über Inspector-Wiring;
  gespawnte Objekte bekommen sie per `Init(...)` injiziert.

### Denkmodell: Model-View-Presenter
- Model = plain C# ohne Unity-API (testbar), View = nur Anzeige,
  Presenter = einzige Brücke. View kennt nie das Model, Model nie die View.
- Pragmatik-Ausnahme: kein Parallel-Model, wo Unity den State schon
  besitzt (Rigidbody, Transform) — zwei Wahrheitsquellen vermeiden.
- Kamera nie als Child des Physik-Bodys: Position käme im Physik-Takt,
  Rotation im Input-Takt — die Mischung lässt die Welt stottern.
  Eigener CameraPresenter liest die Spielerposition in `LateUpdate`
  (nach der Rigidbody-Interpolation).

### SOLID — grob angewendet
- **S**: Eine Klasse, ein Job. Splitten erst, wenn sie zwei echte
  Zuständigkeiten hat — nicht vorsorglich.
- **O**: Erweitern durch neuen Code statt Ändern von funktionierendem —
  Ändern riskiert Regressionen in bereits korrektem Code. Praktisch:
  Definitionen/Configs als ScriptableObjects hinter Interfaces.
  Leitfrage: „Müsste ein dritter Typ diese Klasse wieder anfassen?"
- **L**: Entity-Typen über ein gemeinsames Interface, keine tiefen
  Vererbungsketten.
- **I**: Kleine, fokussierte Interfaces (`IShieldSystem`) statt einem
  fetten `IEntitySystem`.
- **D**: Interfaces nur, wo Austauschbarkeit echten Wert hat — nicht
  per Default auf alles.

### Werkzeuge mit Einsatzkriterium (kein Zwang)
- „X passiert, unabhängige Systeme reagieren" → ScriptableObject-
  Event-Channel. Erst einführen, wenn der zweite konkrete Consumer da ist.
- „Member einer wachsenden Content-Menge" (Items, Gegnertypen) →
  SO-Referenz als Identität statt Enum: neue Member sind neue Assets,
  kein Code-Change, nichts verschiebt sich. Enum nur für bekannte,
  geschlossene Mengen. Den Identitätstyp nach seinem Keyspace benennen,
  nicht nach dem häufigsten Member. Feldtyp per Subtyp einengen statt
  `OnValidate`-Guard — Compiler und Inspector-Picker verhindern den
  falschen Wert dann von vornherein.
- „Service über Szenengrenze" → `RuntimeReference<T>`-SO-Asset
  (single-writer, Consumer nur lesend). Gleiche Szene: direkt verdrahten.
- Bespoke-Lösung statt passendem Werkzeug: kurz begründen.

### UI-Wiring
- Presenter halten serialisierte Referenzen (`Image`, `Text`,
  `RectTransform`) und bauen nie UI zur Laufzeit — kein `new GameObject`,
  kein Canvas-Aufbau im Code; sie steuern, was schon existiert.
- Variable Listen (Inventar, Kontextmenü): eine im Prefab gebackene
  Template-Row klonen und poolen, statt Hierarchie ad hoc zu bauen.
- Read-only-Listen (Log, Statusanzeige): ein `TMP_Text` aus einem
  `StringBuilder` — poolen nur, wenn jede Zeile eigene Interaktion braucht.

### Unity-Handwerk
- Enums append-only: serialisiert wird der Integer — Einfügen oder
  Umsortieren verschiebt gespeicherte Werte still. Neue Member ans Ende.
- Lifecycle — die `Awake`/`OnEnable`-Reihenfolge *zwischen* Objekten ist
  undefiniert, darum zählt das Wo: Model im `Awake` des eigenen
  Presenters erzeugen. Fremde Models in `Start` subscriben (alle Awakes
  laufen vor allen Starts) / `OnDestroy` unsubscriben; View beim
  Subscribe mit dem aktuellen Wert seeden, denn Change-Events feuern
  erst bei der nächsten Änderung. SO-Channels in `OnEnable` /
  `OnDisable`. Jede verdrahtete Referenz null-guarden.
- `GetComponent` nur bei `Awake`/`Init`/Spawn, Ergebnis cachen — nie in
  `Update`-artigen Methoden: die Suche kostet sonst jeden Frame.
- Zerstörte Unity-Objekte hinter Interfaces fängt `== null` nicht —
  Unitys überladenes `==` liegt auf `UnityEngine.Object`, nicht auf dem
  Interface. Darum: `(target as UnityEngine.Object) == null`.
- Neues Input System, nie `Input.GetAxis` — dessen internes Smoothing
  erzeugt Joystick-Gefühl auf der Maus. Ein einziger `PlayerInputReader`
  als einzige Naht zum Input-System: kontinuierlicher Input → gepollte
  Property, diskreter Input → `System.Action`-Event.
- Action-Maps in `OnEnable` aktivieren, in `OnDisable` deaktivieren —
  aktiv gelassene Maps auf deaktivierten Objekten empfangen weiter Input.
- Maus-Delta roh lassen; Gamepad-Bindings mit Dead Zone (min. 0.12) +
  Scale. Optionales Smoothing per Lerp im Code, nie im Binding-Processor
  — so bleibt es per Einstellung konfigurierbar.
- Keine Design-Doc-Zitate in Code/Kommentaren (rotten still) —
  Absicht in eigenen Worten hinschreiben.
- YAGNI: Abstraktion erst beim zweiten konkreten Use-Case.
- Magic Numbers benennen: ein Literal, das ein externes Faktum kodiert,
  bekommt eine benannte `const` mit Warum-Kommentar. Selbsterklärende
  Arithmetik bleibt roh.
- Unity-6-APIs (`rb.linearVelocity`, nicht `rb.velocity`). 4 Spaces,
  keine Tabs.

### Ordnerstruktur (Praxis-Stand 2026-07-19)
- Jeder Baustein bekommt einen eigenen Ordner unter seiner Kategorie:
  `Entities/<Name>/` (Spielfiguren), `Environment/<Name>/`
  (Umgebungs-Assets), `Systems/<Name>/` (Code-Systeme, z.B.
  `Systems/TerrainGenerator/`).
- Unterordner nach `FolderTemplate/`: Animation, FBX, Materials,
  Prefabs, Scripts, Shader, SO_Settings, Textures, VFX — aber nur
  anlegen, was der Baustein wirklich braucht, keine leeren Ordner.
- `Shared/` nur für echte Querschnitts-Utilities mit mehreren
  Abnehmern (BaseScripts, Interfaces, Timer). Ein System mit einem
  Abnehmer bleibt zusammen in seinem Systems-Ordner.
- Editor-Code in einem Ordner, der wörtlich `Editor` heißt (Unity
  kompiliert ihn editor-only und strippt ihn aus Builds); er darf
  im System-Ordner liegen (z.B. `Systems/TerrainGenerator/Editor/`).
- Ordnernamen Englisch, PascalCase. Umbenennen/Verschieben immer im
  Unity-Editor, nie im Explorer — sonst brechen die .meta-GUIDs und
  damit alle Referenzen.

### Review-Gate (vor dem Coden)
Vor jeder Implementierung den Plan gegen diese Datei prüfen — Claude
prüft mit, wenn er Code zeigt oder reviewt:
1. Fattening-Check: neuer `switch`-Zweig / neue Feldgruppe in einer
   geteilten Klasse? → ab der zweiten Variante Abstraktion erwägen.
2. Enum-/Serialisierungs-Sicherheit verletzt?
3. Gibt es ein passenderes Werkzeug (siehe oben)?
4. Naming-Fix: alle Geschwister des Musters mitfixen, nicht nur die
   eine angezeigte Stelle.

## Bewusst nicht übernommen
- ClaudeSetup (Editor-Script baut und verdrahtet Szenen): zurückgestellt —
  Isor tippt in der Lernphase selbst, Claude erklärt. Wieder prüfen, wenn
  Isor sicher programmiert.
- Dozenten-Verbot von Underscore-Präfixen: Uni-Regel `_camelCase` gilt
  überall, auch in privaten Projekten.
- Dozenten-Default „public Felder fürs Inspector-Wiring": verworfen
  zugunsten Kapselung (siehe DECISIONS.md).
