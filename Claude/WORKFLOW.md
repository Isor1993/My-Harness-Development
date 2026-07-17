# WORKFLOW.md — Session-Disziplin

Ownership: Session-Typen und Session-Disziplin.

## Grundregeln
- Jede Session hat genau einen Typ und einen Fokus.
- Vor jedem /clear: Doku-Pflicht — INDEX.md und ROADMAP.md nachziehen,
  Gebautes in FEATURE_LOG.md, Entscheidungen in DECISIONS.md,
  im Uni-Modus zusätzlich Knowledge nach KNOWLEDGE_RULES.md.
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
Gebautes in FEATURE_LOG.md, Entscheidungen in DECISIONS.md.
Commit-Message als Vorschau zeigen, bevor committet wird.

### Art (minimal, ungetestet)
Prompts für Image-Generation/Concepts erzeugen — bildliche Kommunikation
mit der AI.
