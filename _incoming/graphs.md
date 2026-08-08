---
status: seed
tags: [loops, graphs, orchestration, beads]
---

# Graphs, and what they mean next to loops

"Graph" is being used for two different things right now, and only one of them is the successor to
the loop.

Context: the vocabulary moved fast. Steinberger posts
[design loops that prompt your agents](https://x.com/steipete/status/2063697162748260627) on
2026-06-07; six weeks later he posts
[are we still talking loops or did we shift to graphs yet?](https://x.com/steipete/status/2078277297791189132)
(2026-07-18), and the replies are fatigue rather than argument. Yegge opens the loops-and-graphs
section of [The Shape of Things to Come](https://yegge.ai/essays/the-shape-of-things-to-come) with
the same confusion, honestly stated: "I didn't know what the hell they meant any more than you did.
But that not knowing was bothering me. I was supposed to know this stuff."

## Sense 1 — the graph is the org: parallel agents, and the whole graph loops

Agents stop being a single worker in a loop and become a topology of workers. The edges are who
delegates to whom, who reviews whom, who reconciles conflicts. The loop does not disappear; it wraps
the whole graph, which iterates until the goal is met.

Shubham Saboo's [wtf is a graph](https://x.com/Saboo_Shubham_/status/2078301249376825397)
(2026-07-18) is the crispest one-liner on this reading:

> Loops made Agent behavior programmable. Graphs make agent orgs programmable.
> Dynamic Agent Org is where the graph rewrites itself while the work is happening.

The last sentence is the interesting part. A static topology is just a pipeline with more boxes. The
claim is that the shape gets rewritten mid-run — spawn a reviewer here, collapse two workers there —
which is what separates it from a workflow engine.

Cursor's agent swarm work in [`loops.md`](loops.md) is this sense in practice without using the
word: planners that never implement, workers that never plan, a neutral third-party agent that
resolves merge conflicts, stacked decorrelated review lenses. That is an org chart, and the
interesting findings are all org problems — contention, split-brain, ossification, megafiles.

## Sense 2 — the graph is the work: a dependency graph instead of one big plan

This is Yegge's, and it is the one that changes the artifact rather than the runtime. The unit is
not a plan document that goes stale but a graph of issues with dependency and parent/child edges,
which agents claim from and extend as they go.

From the same essay:

> Any sufficiently large project is a graph, so to create a lot of work, you need to create a big
> graph. Beads is your unlock here. Beads *is* a graph, one that includes dependency and
> parent/child edges.

And the part that matters more than the data structure:

> As your work unfolds, knowledge accumulates in your beads. Your project's knowledge-graph ledger
> is built up dynamically as your work-graph is traversed.

So the graph is both the queue and the memory. Closed beads become the project record; findings stay
issue-local instead of being promoted into a global document. Yegge's stated ingredient list for a
long-running factory is short — an infinite token source, [Beads](https://github.com/gastownhall/beads),
a small Markdown project brain, coding agents — and the primitives he names are orchestration ones:
atomic claiming, leasing, gates, triggers.

His diagnosis of the previous generation is the clearest statement of why a plan is not a graph: Gas
Town "wasn't really a 'loop' in the sense that I thought Boris might be suggesting. It was more like
a chariot, with you driving." A plan needs a driver. A graph with claiming and gates does not.

## How the two relate

They are orthogonal, and the discourse keeps collapsing them. Preston Holmes says so directly in a
[reply to Saboo](https://x.com/ptone/status/2078302489275953499): "Two (at least) graphs matter. The
one of long lived agents doing zone defense as you show, and then the dynamic and shifting graph of
work to be done."

- **Agent graph** — who works, in what arrangement. Solves throughput and quality-through-review.
- **Work graph** — what is to be done, in what order, with what unfinished. Solves the plan going
  stale and the loop running out of things to do.

Yegge needs both and says so in sequence: the token tap is what makes the loop run all night, and
then "you need to give them enough work" — which is what the work graph is for. Sense 1 without
sense 2 is a swarm with nothing to chew on. Sense 2 without sense 1 is a well-organized backlog
being worked by one agent at a time.

## My take

The work graph is the part I can actually adopt. It costs a tool and a habit, not $87k/month, and it
addresses a failure I have documented evidence of in my own repos: plans written, used once, and
deleted by rule for sixteen weeks, with the reasoning only recoverable from git. A dependency graph
that accumulates its own record is a direct answer to that, independent of how many agents run
against it.

The agent graph is downstream of budget. It only pays once there is enough parallel work to justify
the coordination overhead, and the Cursor swarm findings read as a catalogue of coordination
overheads. Worth understanding now, not worth building now.

Open question: whether a work graph needs to be a database at all at my scale, or whether Markdown
files with explicit dependency links get most of the value. Yegge is emphatic that nothing else
comes close to Beads, but he is also describing a twelve-Max-account operation, and his complaint
that Beads "strains databases pretty hard" is a symptom of a scale I do not have.
