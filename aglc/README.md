# Agentic Development Life Cycle (AGLC)

Beschreibt die Anwendung der genannten Prinzipien auf den Entwicklungsprozess, und zwar nicht nur den Softwareentwicklungsprozess, sondern den gesamten Produktentwicklungsprozess.

Die Grundidee ist, die Agenten dort einzusetzen, wo sie am effizientesten sind, und das ist bei der Generierung von Code und beim Zusammenfassen und Aktualisieren vieler verschiedener disparater Quellen, Dokumentation, Tests sowie beim Erzeugen von Übersichten, Proof of Work und anderen unterstützenden Artefakten.
[Matt Pokock und Uncle Bob.](https://www.youtube.com/watch?v=zcLPGC-tvgk&t=583s)

## Why have a process and not just vibe engineer

[Agents are able to code themselves into a corner](https://www.youtube.com/watch?v=zcLPGC-tvgk&t=675s)

## Scope

- The phases of agentic development, and which SDLC phases survive, mutate, or disappear.
- Where the human sits in the loop, and how that position changes as trust accumulates.
- Verification as the new bottleneck: if generation is cheap, review is the constraint.
- Artifacts that matter: specs, agent instruction files, evals, plans, diffs.
- Team-level questions: parallel agents, merge pressure, code ownership, review capacity.

## Notes

- [`feature-lifecycle.md`](feature-lifecycle.md) — the path from an unstructured request to living
  documentation: lanes, gates, the merge queue, the release branch, and severity.
- [`artifacts.md`](artifacts.md) — the catalogue of every document the lifecycle produces, what
  belongs in each, and how long it is allowed to live.
- [`releasing.md`](releasing.md) — main, release branches/candidates, going live as a tag.

## Open questions

- Which SDLC phase is actually eliminated — or is every phase just compressed?
- Is the unit of work still the ticket, or does it become the spec?
- What does "done" mean when the cost of another iteration approaches zero?
- How does the classic testing pyramid change when tests are as cheap to generate as the code?
- Where does architecture live? An agent can write any module, but who holds the shape of the whole?
- What is the agentic equivalent of technical debt, and how does it accrue differently?
