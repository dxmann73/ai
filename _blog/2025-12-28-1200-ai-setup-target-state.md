# Zielbild für mein AI-Setup

> Drawn from [_journey/2025-12-28.md](../_journey/2025-12-28.md).
>
> Post: none yet.

## TLDR

Mein AI-Setup: mehrere Agenten mit unterschiedlichen Stärken, schlanke
Projektregeln, wachsender Kontext, Feedback-Loops und Arbeit, die auch an
langsamen Tagen weitergeht.

## Content

Zielbild für mein AI-Setup. Inspiriert von steipetes "Shipping at Inference-Speed" und Theos "How I
code with AI right now".

### Agenten

- **Codex** / GPT-5, konkret `gpt-5.2-codex high`. Denkt länger, besser für große Refactorings.
  Läuft unter WSL.
- **Claude Code** / Opus 4.5. Eager, besser für kleinere Edits.
- **Cursor CLI** / Composer 1. Kann beides.

### Regeln und Kontext

`AGENTS.md` nach der Spec, verschachtelt über die Verzeichnisebenen:

- `~/dev` — allgemeine Guidelines (knapp sein etc.), geklont aus dave-home
- `~/dev/<project>` — Projekt-Guidelines, wird beim Aufsetzen aus anderem Projekt kopiert
- `~/dev/<project>/frontend`, `.../frontend/components` — je eine Ebene tiefer

`CLAUDE.md` ist ein Symlink auf `AGENTS.md`. Dasselbe File liegt in `~/.config/opencode/AGENTS.md`.
Cursor kann kein `AGENTS.md`, dort stattdessen Rules. Skills nach der agentskills.io-Spec; Codex und
Cursor haben beide eigene Skill-Verzeichnisse.

Prinzip: klein anfangen. Eine Regel kommt erst dazu, wenn der Agent denselben Fehler wiederholt.

### Workflow

**Planen.** Plan Mode mit GPT-5, detaillierter Plan. Dem Agenten sagen: "ask clarifying questions",
"provide alternative approaches". Neue Docs per `read thing @https://...` reinziehen. Ergebnis
landet in `feature-spec.md` und geht ins SCM. Scope wird laufend nachgezogen.

**Implementieren.** Mehrere Modelle probieren, laut @rahul mindestens vier Wege. Output _lesen_.
Grenzen suchen und schieben, in dieser Reihenfolge:

1. Prompt tweaken statt Code tweaken — versuchen das als One-Shot hinbekommen, ggf. `/summarize`
1. `AGENTS.md` tweaken: "Every manual edit you make to code is an opportunity to tweak an AGENTS.md"
1. Mehr Kontext geben
1. Tooling-Zugriff geben (`@browser` etc.)

Geht es gar nicht, Kontext prüfen; im schlimmsten Fall den Prompt wegsperren und später mit besseren
Modellen nochmal ansetzen. Den Agent Code-Kommentare setzen lassen. Tests später. Was für die
aktuelle Arbeit zählt, kommt in `AGENTS.md`. Kleine Beispiel-Implementierungen generieren und sagen
"mach es so".

### Tooling

- Jeder manuelle Schritt ist ein offener Loop, der geschlossen gehört.
- Agent Aliases anlegen lassen, die er später selbst in der Shell nutzt.
- Separate Boxen/VMs mit laufendem Dev-Stack, die `--dangerously-skip-permissions` dürfen.
- Code-Review-Tools im Repo: Greptile oder CodeRabbit, dann `@greptile review` im PR.
- Git Worktrees, Setup über `Worktrees.json`, in Cursor nativ.
- Firecrawl als Tool zum Doku-Lesen, ohne den Kontext vollzumüllen.
- Cloud/Remote: Cursor Cloud Agents, Codex Cloud Delegation. Unterwegs per Handy: doom-coding.

### Slow Days

Wenig Fokus, keine Ideen — dann: Use Cases und Feature-Beschreibungen updaten, Verzeichnisstruktur
aufräumen, Code Reviews, refactoren, Tests schreiben, neue Evals ausdenken, Dependencies und
Komponenten updaten, Tailwind-Props sortieren.

### Offen

- Greptile im PR wirklich einführen
- Modelle mit wenig Aufwand an Feedback kommen lassen, statt jedes Mal alles zu laufen
- Remote-Agenten fürs Reisen aufsetzen
- Claude Code: Features, Permissions, Rules, Skills noch nicht durchdrungen
- Playwright-Scripts für Quarkus- und TanStack-Apps als Skills

### Referenzen

- [Shipping at Inference-Speed — Peter Steinberger](https://steipete.me/posts/2025/shipping-at-inference-speed)
  (insbesondere der
  [Config-Abschnitt](https://steipete.me/posts/2025/shipping-at-inference-speed#my-config))
- [steipete/agent-scripts — AGENTS.MD](https://github.com/steipete/agent-scripts/blob/main/AGENTS.MD)
- [How I code with AI right now — Theo, t3․gg](https://youtu.be/-g1yKRo5XtY) — Timestamps:
  [Worktrees](https://www.youtube.com/watch?v=-g1yKRo5XtY&t=788s),
  [Worktrees.json](https://youtu.be/-g1yKRo5XtY?t=943),
  [Greptile](https://youtu.be/-g1yKRo5XtY?t=1951),
  [Tools als Feedback-Kanal](https://youtu.be/-g1yKRo5XtY?t=2113)
- [AGENTS.md — Spec](https://agents.md)
- [Specification — Agent Skills](https://agentskills.io/specification)
- [rberg27/doom-coding — coden per Smartphone](https://github.com/rberg27/doom-coding)
- [Codex — OpenAI](https://openai.com/de-DE/codex/)
- [Windows sandbox / WSL — Codex](https://developers.openai.com/codex/windows#windows-subsystem-for-linux)
- [Sample Configuration — Codex](https://developers.openai.com/codex/config-sample)
- [Codex Security](https://developers.openai.com/codex/security)
- [Prompting / Example workflows — Codex](https://developers.openai.com/codex/workflows)
- [Rules / Execution policy — Codex](https://developers.openai.com/codex/exec-policy)
- [Custom instructions with AGENTS.md — Codex](https://developers.openai.com/codex/guides/agents-md)
- [Build skills — Codex](https://developers.openai.com/codex/skills#where-to-save-skills)
- [Developer commands (Slash Commands) — Codex](https://developers.openai.com/codex/ide/slash-commands)
- [Codex IDE extension — Cloud Delegation](https://developers.openai.com/codex/ide/features#cloud-delegation)
- [Claude](https://claude.ai/new)
- [Cursor CLI — Overview](https://cursor.com/docs/cli/overview)
- [Permissions — Cursor CLI](https://cursor.com/docs/cli/reference/permissions)
- [Rules — Cursor Docs](https://cursor.com/docs/context/rules) (inkl.
  [Agent Skills](https://cursor.com/docs/context/rules#agent-skills))
- [Worktrees — Cursor Docs](https://cursor.com/docs/configuration/worktrees)
- [Cursor Cloud Agents](https://cursor.com/agents)
