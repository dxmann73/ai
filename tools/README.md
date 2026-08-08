# Tools

The concrete landscape: models, harnesses, protocols, and the machinery around them.

Fast-moving. Notes here should be dated, since half of them will be wrong within a year.

## Scope

- Models: capabilities, tradeoffs, and how to choose between them for a given job.
- Harnesses and agent runtimes: CLIs, IDE integrations, background and scheduled agents.
- Protocols and integration: MCP, tool definitions, sandboxing, permissions.
- Evals: measuring whether an agent setup actually works, and why most measurement is bad.
- Skills, subagents, and workflow composition.
- Local infrastructure: how a personal setup is wired together and why.

## Notes

- [Beads: where issue data lives when work spans repositories](./beads-where-issue-data-lives.md)
  (2026-08-08) — storage model, routing, hydration, and the four ways to reference work in another
  repository. The adoption side is in
  [`adoption/beads-adoption.md`](../adoption/beads-adoption.md).

## Open questions

- What separates a harness that compounds over time from one that just wraps an API?
- How much of an agent's effectiveness is the model versus the scaffolding around it?
- What is worth automating permanently versus asking for ad hoc?
- When does adding a tool make an agent worse?
