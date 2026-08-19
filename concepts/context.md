---
status: draft
tags: [concepts, context]
---

# Context Management

Deciding what goes into the window, and what stays out — the practical craft behind
"context decides outcomes" (see [first principles](first-principles.md)).

## Dumb zone / Smart zone

TODO
matt pocock has resources on this https://www.youtube.com/watch?v=nQwJVHCtDDY&t=93s
david ondrej with that dude here who claims he coined the term https://www.youtube.com/watch?v=xgkjtF89-44 

## Trajectory / Alignment

Context window auch deswegen klein schneiden, damit mehr Regeln und Guidelines reinpassen, die zu den Zielen passen.
Ein großes Dokument mit vielen Zielen (und teils Zielkonflikten) wird zu Problemen führen
TODO Verlinkung 2026-08-21

## Feed exactly the context the agents need

Not more, not less. In practice this spans a range of granularities:

- **Standing policy** — `AGENTS.md` / `CLAUDE.md`: conventions, boundaries, how this repo works.
- **Task framing** — the goal, the acceptance criteria, the plan.
- **Working material** — the source files, interface descriptions, test output actually in play.

Everything in that list is a cost as well as a benefit: it competes for attention with everything
else in the window. The discipline is subtraction as much as addition — remove stale intent rather
than override it.

## Proposal: 1kpx fine grained ctx management

For adoption in companies and also privately.
No matter where the agent lives, whether Slack or terminal, it would be optimal to plug in the
right things on demand.
For example, I would have an agent for product A. It would have its own repo and a few interface
descriptions for product B. You would then have to assign certain slices of B to this agent. The
agent for A should see the interfaces of B, but not its whole repo — otherwise it fills its window
with material that only makes it slower and worse.
