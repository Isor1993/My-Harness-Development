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

## Session-Typen

### Brainstorm (im Einsatz, getestet)
Ideen verarbeiten/evaluieren, freies Brainstorming, Sonderaufgaben ohne Code.
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

### Design (minimal, ungetestet)
Eine Sache ausdesignen: was gebaut wird und wie, bevor entwickelt wird.
Ergebnis: Entscheidungen in DECISIONS.md, Design-Absicht später ins GDD.

### Development (minimal, ungetestet)
Die eigentliche Entwicklung; läuft, bis die nächste Runde startet.
Vor dem Coden: Review-Gate aus CODE_GUIDELINES.md durchgehen.
Gebautes in FEATURE_LOG.md, Entscheidungen in DECISIONS.md.
Commits: siehe Grundregel „Session-Ende".

### Art (minimal, ungetestet)
Prompts für Image-Generation/Concepts erzeugen — bildliche Kommunikation
mit der AI.
