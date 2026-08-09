# Agentic Development Life Cycle (AGLC)

What replaces the SDLC when most of the code is written by agents and the human's job shifts from
authoring to specifying, steering, and verifying.

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
