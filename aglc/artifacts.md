---
status: draft
tags: [aglc, artifacts]
---

# Artifacts

Every document the lifecycle produces, what it is for, and how long it is allowed to live.

The companion to [`feature-lifecycle.md`](feature-lifecycle.md), which describes the process this
catalogue serves. That note says when each artifact is written and read; this one says what belongs
inside it and what is allowed to delete it.

The organising question is **lifespan**. Communicating with an agent produces a great deal of
writing, and the useful distinction is not formal versus informal but transient versus permanent.
A transient artifact exists to be consumed and then removed. A permanent one is continuously
revised and never deleted. Confusing the two in either direction is the source of most
documentation failure: keep the transient and agents reason from stale requirements, delete the
permanent and the reasoning is gone.

The permanent set is larger than it first looks. What survives is everything that describes the
system — why it exists, what it does, how it is designed, and why it was designed that way. What is
consumed is everything that describes a *change* to the system: the request, the requirement, the
plan, the review finding. Once the change has landed, its description is redundant with the system's
own description, and keeping both is how they drift apart.

## Transient artifacts

### Wish

**What it is:** an unshaped request. A sentence in a support channel, an overheard complaint, a
half-formed objection.

**Why it exists:** because the alternative is that it dies where it was said. The wish factory only
works if the cost of adding is near zero.

**Contains:** whatever the person had. No format is required and none should be demanded.

**Lifespan:** until it becomes a feature request or is judged not to be one. Lives in
`_incoming/wishes/`.

### Feature request

**What it is:** a specific thing someone wants built, already shaped enough to be argued about.

**Why it exists:** it is the unit the intake gate acts on — the thing that gets a yes, a no, or a
different shape.

**Contains:** the want, the context it came from, and ideally who to tell when it lands.

**Lifespan:** until the PRD exists and the work is closed out. Lives in `_incoming/features/`,
deleted at closeout.

### Bug report

**What it is:** a claim that the software does not do what it already promised.

**Why it exists:** to enter the fast track. It is the only lane with no admission question, because
whether the software should work was settled when the behaviour was specified.

**Contains:** on arrival, a symptom. After the fast track: a reproduction, a cause, a severity, and
usually a proposed fix — the reproduction being the part that matters.

**Where it comes from:** people, and machines. Logging, monitoring, and observability are a bug
source in their own right — an exception surfacing in a log scan files a bug even when nothing
user-facing broke. Those arrivals are the ones with nobody attached to explain them, which is
exactly why automatic investigation earns its cost.

**Lifespan:** until the fix lands. Lives in `_incoming/bugs/`.

### Backport bead

**What it is:** a defect fixed on the release branch, travelling to main as a bead carrying both the
error and the remedy.

**Why it exists:** because it is not a cherry-pick. Main has moved since the cutover, so the fix is
re-derived in main's context rather than transplanted. The bead is also the identity of the defect
across both lines — without it, the two fixes are unrelated work and nothing notices when only one
of them lands.

**Contains:** the error, the fix as made on the branch, the severity, and a reference to the branch
work it came from.

**Lifespan:** until the fix lands on main. Closing it on main is what ends it.

### PRD — Product Requirements Document

**What it is:** a description of a specific product outcome — the *what* and the *why*, not the
*how*.

**Why you need it:** if you were building a city, this is the plans for one building: plumbing,
wiring, materials, cost, timeframe. You want it before hiring contractors and buying machines.

**Contains:** context, assumptions, goals; personas and use cases; functional and non-functional
requirements; success metrics; risks, dependencies, non-goals, open questions.

**Lifespan:** transient by design. It exists to be turned into work and then dissolved — permanent
parts redistributed at closeout, the rest deleted. Keeping one past implementation guarantees an
agent eventually finds it and reasons from requirements superseded eighteen months ago.

### Plan

**What it is:** the implementation guideline for an agent, built from the artifacts above and the
prompt. Effectively the agent's starting instruction.

**Why you need it:** to get the agent aligned before it starts, and as the means of offloading and
parallelising work.

**Contains:** the steps and approach the agent intends to take, and its success criteria.

**Lifespan:** the session it was written for. Plans only have meaning at their point in time, like
last week's shopping list. Worth keeping only if you are building evals or grading human ↔ agent
communication.

### Gate findings

**What it is:** the output of any review, security scan, or drift check.

**Lifespan:** until fixed or promoted to a bead. Findings are never a document that accumulates —
a standing list of known problems is a list nobody reads.

### Release branch

**What it is:** not a document, but an artifact with a lifespan and worth listing as one. Cut from
main at a roadmap cutover, closed to new features, stabilized until green.

**Lifespan:** until released and fully backported. The last backport landing on main is what ends
it.

## Permanent artifacts

### Vision document

**What it is:** the high-level vision for the product — a description of the system being built,
what and who it is for, what it should and should not accomplish.

**Why you need it:** alignment of the overall goal with the agent. It is the only artifact that
answers *why does this product exist* rather than *what does it do*, and it is the thing every
other document is downstream of.

**Where it lives:** `vision.md` in the repository root. Not filed under a docs tree — root, where
nothing can miss it and no agent has to be told where to look.

**Lifespan:** permanent, rarely revised. Frequent revision is a signal in itself: the vision
changing is a much larger event than a requirement changing.

### Specification and user journeys

**What it is:** the description of what the product does, expressed as user journeys wherever that
fits. It carries the two scope definitions below.

**Why you need it:** it is the destination of every closeout, the source for regression testing, and
the document that answers "what is this supposed to do" without reading the code.

**Lifespan:** permanent, continuously revised. Never deleted, because it is the only artifact that
holds current intended behaviour.

#### MVP definition

**What it is:** the set of workflows without which the product is not a product. Part of the
specification, not a document beside it — the workflows it names are specification entries, and the
MVP marks a subset of them.

**Why you need it:** it is what makes severity a lookup rather than an argument. P1 means an MVP
workflow is broken in live, which anyone can check — including an agent, mid-incident, under
pressure.

**Lifespan:** permanent and **operational**. This is the distinction that gets missed: it is not a
planning artifact from launch, it is a document read during incidents, so it has to be current. A
workflow that has quietly become essential and was never marked means its outage is silently scored
a P2, and nobody involved will notice they are reading a stale document.

#### MLP definition

**What it is:** the minimum lovable product — the MVP plus the features that make the product worth
choosing rather than merely usable. Also part of the specification, marking a wider subset.

**Why you need it:** it defines the P2 boundary the same way the MVP defines P1.

**Lifespan:** permanent and operational, on the same terms.

### SDD — Software Design Document

**What it is:** how the system is designed — the patterns it uses, the shape it has, the way its
parts are meant to fit together. Present tense and present state, describing the system as it now
stands and as it is to be extended.

**Why you need it:** a PRD gets split into slices implemented independently, and the risk is that
the result is incoherent. The SDD is what makes coherence checkable. It is also the primary input
to the taste gate: "does this fit the shape of the system" is not answerable without a written
account of what that shape is.

**Contains:** architecture, patterns to be used, data model, APIs, flows, services, storage,
integrations, security, observability.

**Lifespan:** permanent, continuously rewritten. Updated at closeout to describe the system as it
now is, not appended to.

**Relationship to ADRs:** they answer different questions and the distinction is worth holding
firmly, because collapsing it is how both documents go bad. An **ADR** records a decision — we went
this way rather than that way, here are the alternatives, the pros and cons, and the reasoning. It
is a record of a moment, append-only, superseded but never deleted. An **SDD** records the
resulting design as it currently stands. It is rewritten so that it always reads as true today, and
it carries no history at all.

So: the ADR corpus is where you go to ask *why is it like this*. The SDD is where you go to ask
*what is it like*. An SDD that starts accumulating "previously we did X" has turned into a bad ADR
log; an ADR that gets edited to match the current design has stopped being a record.

### ADR — Architecture Decision Record

**What it is:** a record of the structure of the body of work and the principles it is designed
along, capturing the major choices made during implementation.

**Why you need it:** PRDs define requirements, which turn into the structure that executes them.
You need to remember what the structure is and why it was built that way in order to repair it or
fit new work to it.

**Contains:** the decision, its context, what was rejected, and when it would be worth revisiting.

**Lifespan:** permanent. Superseded, never deleted. This is the northern star — input and feedback
for PRDs, implementation, maintenance loops, and reviews.

**Second job:** together with the SDD, ADRs are what makes the taste gate delegable. Taste that has
been externalized is enforceable by anything that can read; taste that lives only in someone's head
is not. The share of the architecture gate that needs a human is a measurement of how much has been
written into these two documents.

### E2e and user-journey scenarios

**What it is:** the executable form of the specification.

**Lifespan:** permanent, for the same reason the specification is. They are the same artifact in a
different notation, which is the practical argument for keeping the specification accurate rather
than aspirational — it is the source these are written from.

### Changelog

**What it is:** what changed, per release.

**Lifespan:** permanent, append-only. One of the few artifacts whose purpose is a record over time
rather than present intent.

### Roadmap

**What it is:** what is intended, and in what order.

**Why you need it:** it picks the cutover point. This is the one place in the lifecycle where the
roadmap is load-bearing rather than informational — it decides when a release branch is cut, and it
decides it on intent rather than on the state of the build.

**Lifespan:** permanent, continuously revised.

### Operations manual

**What it is:** how to run the thing — deployment, recovery, the runbook for the failures that are
known to happen.

**Lifespan:** permanent, revised at every closeout that changes how the system is operated.

### Observability notes

**What it is:** what needs watching about a specific feature, and what changed behaviour would mean.

**Lifespan:** until the feature is boring. Judgment, not a rule.

## The graph

Beads are the exception to the transient/permanent split, because they are not a document at all.
Task state, sequencing, dependencies, and what is ready to work on live in the graph and nowhere
else. Nothing deletes them; closing is not deleting, and the graph keeps its own history.

The split is the entire reason for adopting the tool
([`adoption/journey/beads-adoption.md`](../adoption/journey/beads-adoption.md)): task state was
always the part that was safe to delete, reasoning never was, and the failure was that both lived
in the same file.

## Lifespans at a glance

| Artifact | Lifespan | Deleted by |
| --- | --- | --- |
| Wish | Until shaped or dismissed | Promotion to a request |
| Feature request | Until PRD exists | Closeout |
| Bug report | Until the fix lands | The fix |
| Backport bead | Until the fix lands on main | Closing on main |
| PRD | Until implemented | Closeout |
| Plan | The session it was written for | The work finishing |
| Gate findings | Until fixed or promoted to a bead | The fix |
| Release branch | Until released and fully backported | The last backport landing |
| Beads / task state | Until closed; graph keeps history | Nothing — it is the graph |
| Vision (`vision.md`, repo root) | Permanent, rarely revised | Never |
| Specification / user journeys | Permanent, continuously revised | Never |
| MVP and MLP definitions | Permanent, operational — part of the specification | Never |
| SDD | Permanent, continuously rewritten to present state | Never |
| ADRs | Permanent, superseded not deleted | Never |
| E2e and user-journey scenarios | Permanent — the specification, executable | Never |
| Changelog | Permanent, append-only | Never |
| Roadmap | Permanent, continuously revised | Never |
| Operations manual | Permanent, revised | Never |
| Observability notes for a feature | Until the feature is boring | Judgment |

The rule underneath the table: **a document is deleted only once every durable fact it holds lives
somewhere with a longer lifespan.** Deletion is a move, not a loss.

## Why the deletions matter

The instinct is to keep everything — storage is free, and an old PRD feels like history. It is not
free. Every retained transient artifact is a future context-window entry that an agent may treat as
current, and the cost lands on the day a stale requirement quietly contradicts a live one.

The inverse failure is the one already documented in this repo: throwaway plans deleted in the
commit that finished the work, taking a month of reasoning with them
([`adoption/journey/beads-adoption.md`](../adoption/journey/beads-adoption.md)). Both failures have
the same root — task state and reasoning sharing a file. The catalogue above works only if they
never do.

## Open questions

- Instruction files (`AGENTS.md`, `CLAUDE.md`) are artifacts by every test applied here, and they
  are absent from this list. They are permanent, continuously revised, and read constantly. What
  updates them, and at which step?
- The specification is permanent, continuously revised, and touched by every feature. What stops it
  from becoming the megafile that everything collides on?
- If taste is delegable in proportion to what has been written into the SDD and the ADRs, both grow
  until no agent can hold them in context. Is the answer layering, an index, or a working set — and
  is it the same answer the specification will need?
- The SDD is rewritten to present state and the ADRs keep the history, which works only if every
  rewrite is accompanied by the ADR explaining it. What catches the case where the SDD was updated
  and no ADR was written? The design silently changes and the reasoning is gone — the same failure
  as the deleted plan, one level up.
- Tests are an artifact nobody lists and everybody keeps. Are they the executable specification, or
  a separate thing that happens to overlap with it at the e2e level?
