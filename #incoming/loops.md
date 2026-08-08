> Captured from website `src/content/ai/en/loops.mdx` on 2026-08-08. Verbatim, unedited.

---
title: "AI — Loops"
---

# Loops and loop engineering

The idea is to run agents in a loop towards a certain goal, until they are finished.

More specifically, the idea is to let agents run the complete cycle from feature/requirement to merge / production,
with the loops running indepentently until the desired result pops up.
This is basically an attempt to remove the human in the loop.
Despite the fact that this incurs immense token cost, it is not clear how stable results are supposed
to emerge when the current process just isn't delivering them even with human intervention.
For that reason this is only proven to work when the requirements are solid or can be inferred from a working
example:
- Rust rewrite
- Excel reimplementation
- SQLite rewrite with [agent swarms](https://cursor.com/blog/agent-swarm-model-economics) (see below)

## Sources

Steipete: I'm just orchestrating loops now
Dario: My job now is to engineer loops LOL
Theo: [I've tried loops and they kinda worked, but are so expensive](https://www.youtube.com/watch?v=iJVJwmCKW9o)
Armin Ronacher: [Will be the future, not there yet](https://lucumr.pocoo.org/2026/6/23/the-coming-loop)
> Present-day models tend to produce code that is too defensive, too complex, too local in its reasoning.
> They avoid strong invariants. They add fallbacks instead of making bad states impossible.
> They duplicate code, invent bad abstractions, and paper over unclear design with more machinery.
BUT: Great when result can be judged by (this or another) LLM

## Example agent loops

Matthew Berman: [Try these loops](https://www.youtube.com/watch?v=F4a8aMLb678)

# Agent Swarms

Cursor has used [agent swarms](https://cursor.com/blog/agent-swarm-model-economics) to implement autonomously from
a spec, with an existing test suite. Important insights:

## Separation of work into planning and execution

- Planner agents, powered by the smartest models, split a goal into pieces and delegate them.
- Worker agents, generally powered by faster and less expensive models, execute those pieces.

A planner never implements, so its context never fills with low-level detail.
A worker never plans, so it can spend all its context on one narrow piece of work.

## VCS for agents

Git is intended for humans, with a swarm it breaks down due to frequent conflicts. They've implemented a new type of VCS
which addresses these issues.

Split-brain design:
Two planners, unaware of each other, implement the same concept. Fixed this through prompting:
Planners are required to ensure that no two delegated subtrees decide the same question.

Contention between planners:
Two planners fight through back-and-forth changes over the same files.
The problem is two pictures of reality, and merge tooling can't fix a disagreement.
Solution: Agents record decisions in shared design docs.
Code that depends on a decision carries a compile-checked reference back to its doc.
When planners contradict each other, a reconciler merges the docs and the references propagate the resolution downstream.

Merge conflicts:
Agents constantly collide on the same files.
In order to resolve a collision they would have to stop, absorb the other agent's context, and merge around it.
Worker agents are bad at this and, in practice, either overwrite the other change or abandon their own.
Solution: A neutral third-party agent intervenes on merge conflicts and resolves them on behalf of all parties.
Its only goal is to be impartial and efficient, similar to the way merge queues work in engineering teams.

Megafiles:
Some files are particularly popular places for agents to work.
Each agent adds only a small amount of code, and no single agent is responsible for keeping the files small.
These “megafiles” choke everything. They’re expensive to transport, diff, and merge, and become the site of constant collisions.
Solution: Flag bloated files, block new commits and let an outside agent decompose the overgrown file into smaller modules.

Ossification:
Touching core code propagates changes throughout the code base, so agents avoid and try to work around that.
Solution: License intentional breakage. An agent judges a core change worthwhile, makes a focused patch outside its scope and leaves a comment explaining why it did it.
Now everything depending on the old design fails to build.
Each agent that hits one of those errors finds the comment, reads the reasoning, and updates its own piece of work to match.

Review lenses:
The more agents run, the more errors and deviations accumulate. This needs correction before small mistakes become foundational.
Solution:
Stack different kinds of review lenses:
- giving a review agent the worker's full transcript
- giving a review agent the only worker's output
- giving a review agent nothing but the codebase
- ahve reviewers running on different models, with different training, different personalities.

No single lens catches everything, but decorrelated lenses stack.
The compute spent on review is high return, since review is much cheaper than the work it audits.
We suspect this stacked review system was a major contributor to the sustained quality of the runs.

Letting agents shape the environment:
Agents lose memory from session to session, because this knowledge has no place.
Solution: Let agents institutionalize rules like “keep notes” and “document decisions” for their future selves and teammates.
Use a shared context called the "Field Guide", i.e. a folder owned entirely by the agents, whose index.md is automatically injected into every agent at start.
It is the agents’ job to curate what goes into the guide and their only constraint is a line budget.

