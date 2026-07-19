# WORKFLOW.md — Session-Disziplin

Ownership: Session-Typen und Session-Disziplin.

## Grundregeln
- Jede Session hat genau einen Typ und einen Fokus.
- Vor jedem /clear: Doku-Pflicht — INDEX.md und ROADMAP.md nachziehen,
  Gebautes in FEATURE_LOG.md, Entscheidungen in DECISIONS.md;
  Knowledge-Abfrage in jeder Session (KNOWLEDGE_RULES.md), im
  Uni-Modus Pflicht; TDD-relevante Ergebnisse grob in TDD_NOTES.md
  einreihen (nur echte Uni-Projekt-Arbeit).
- Session-Ende: Claude schlägt den Commit vor — Titel im Schema
  `Update V <nächste Nummer>` (vierstellig hochgezählt, z. B. V 0.0003)
  plus fertige Description (was die Session geändert hat). Titel und
  Description immer auf Englisch — das Repo ist englischsprachig.
  Isor committet selbst über sein Git-Tool.
- Max. 2–4 parallel offene Sessions.
- Session-Schnitt an der Baustein-Grenze, nicht am Kontext-Balken:
  Baustein fertig → Sichern → Commit → /clear. Einen neuen Baustein
  nicht unter ~30 % Restkontext starten — lieber vorher schneiden.

## Session-Typen

### Brainstorm/Design (im Einsatz, getestet)
Ideen verarbeiten/evaluieren, freies Brainstorming, ausdesignen was
gebaut wird und wie — Sonderaufgaben ohne Code. Ergebnis: Entscheidungen
in DECISIONS.md, Design-Absicht später ins GDD.
Regel pro Baustein: erst eine Brainstorm/Design-Session (was & wie
entscheiden), dann die Development-Session (nur Umsetzung). Eine
Brainstorm/Design-Session darf mehrere Bausteine vorentscheiden.
Zu Beginn fragt Claude, welcher Modus gilt: **normal** oder **uni**.
- **Normal** — wie oben, kurz und bündig.
- **Uni** — Fokus auf Erklären, nicht auf Bauen/Entscheiden:
  - Einfach erklären, wo möglich visuell (Skizzen/Diagramme, zusätzlich
    als Artifact fürs Handy).
  - Ausführlich statt knapp — bewusste Ausnahme von "kurz und bündig".
  - Claude prüft aktiv per Rückfrage, ob Isor es verstanden hat; falls
    nicht: einfacher erklären, nicht nur wiederholen.
  - Ziel: Isor lernt es und kann es später selbst anwenden.
  - Erkenntnisse am Session-Ende in den Knowledge-Ordner
    (KNOWLEDGE_RULES.md).

### Development (minimal; erster Praxistest: Chunk-Umbau)
Nur Umsetzung dessen, was in DECISIONS.md vorentschieden ist.
Lern-Modus ist der Normalfall: Isor tippt selbst; Claude liefert
Gerüst, Erklärungen und Rechenbeispiele zum Prüfen — keine fertigen
Dateien. Kleine Design-Fragen (Namen, Ablageort) werden inline geklärt;
Fragen, die Architektur oder mehrere Bausteine betreffen, werden
notiert und in die nächste Brainstorm/Design-Session gegeben.
Vor dem Coden: Review-Gate aus CODE_GUIDELINES.md durchgehen.
Gebautes in FEATURE_LOG.md, Entscheidungen in DECISIONS.md.
Commits: siehe Grundregel „Session-Ende". Typ wird im Praxistest
nachgeschärft. (Später-Schublade: automatisierter Modus — Claude baut,
Isor reviewt — erst nach der Lernphase.)

### Art (minimal, ungetestet)
Prompts für Image-Generation/Concepts erzeugen — bildliche Kommunikation
mit der AI.
