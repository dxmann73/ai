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
writing, consisting of

- transient artifacts to be consumed and then removed
- permanent artifacts that are continuously revised and never deleted.

Confusing the two in either direction is the source of most documentation failure: keep the
transient, and agents reason from stale requirements; delete the permanent and the reasoning is
gone.

The permanent set is larger than it first looks. What survives is everything that describes the
system — why it exists, what it does, how it is designed, and why it was designed that way. What is
consumed is everything that describes a *change* to the system: the request, the requirement, the
review finding. Once the change has landed, its description is redundant with the system's
own description.

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
different shape. Wishes may become feature requests. The same applies for bugs.

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

**Lifespan:** until the fix lands. Lives in `_incoming/bugs/`. The fix deletes the report, and the
reproduction moves into the suite as a [regression test](#tests) — the durable part outliving the
document that carried it.

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
*how*. The PRD and the plan are the same document. There is no separate planning artifact that
restates the requirement in implementation terms: the thing agreed during alignment is the thing
handed to the agents.

**Why you need it:** if you were building a city, this is the plans for one building: plumbing,
wiring, materials, cost, timeframe. You want it before hiring contractors and buying machines.

**Contains:** context, assumptions, goals; personas and use cases; functional and non-functional
requirements; success metrics; risks, dependencies, non-goals, open questions. And the
**documentation obligations**: which permanent artifacts this work is expected to change.

**What happens to it:** once alignment is achieved it is mapped into the issues graph — the beads
and their dependency edges are the PRD in executable form. That mapping is what ends the document's
usefulness as prose. The documentation obligations map with everything else: they are part of what
the beads are, so the doc change lands in the same merge as the code and passes the same gates.

**Lifespan:** transient by design. It exists to be turned into work and then dissolved — permanent
parts redistributed at closeout, the rest deleted. Keeping one past implementation guarantees an
agent eventually finds it and reasons from requirements superseded eighteen months ago.

### Gate findings

**What it is:** the output of any review, security scan, or drift check.

**Why it exists:** to keep the finding out of the working agent's context. The agent that wrote the
code is mid-task and sidetracking it is expensive; therefore the finding is delivered as a bead
to be picked up later.

**What happens to it:** findings become beads and are worked in the same iteration as the PRD beads,
not deferred to a later cleanup pass. They join the same graph and the same ready queue; the only
difference is where they came from.

**Lifespan:** until fixed or promoted to a bead. Findings are never a document that accumulates —
a standing list of known problems is a list nobody reads.

### Release branch

**What it is:** not a document, but an artifact with a lifespan and worth listing as one. Cut from
main at a roadmap cutover, closed to new features, stabilized until green.

**Lifespan:** until the version goes live. Going live is the end of the branch: it is tagged at the
released commit and then deleted. The tag is what remains, and it is permanent — a released version
has to stay addressable for as long as anyone can be running it.

Outstanding [backport beads](#backport-bead) are not a reason to keep the branch. They live in the
graph, not on the branch, and they carry the fix as text rather than as a commit to cherry-pick, so
they survive the deletion intact.

## Permanent artifacts

### Vision document

**What it is:** the high-level vision for the product — a description of the system being built,
what and who it is for, what it should and should not accomplish.

**Why you need it:** alignment of the overall goal with the agent. It is the only artifact that
answers *why does this product exist* rather than *what does it do*, and it is the thing every
other document is downstream of.

**Where it lives:** `VISION.md` in the repository root. Not filed under the docs/ tree so agent
don't have to be told where to look.

**Lifespan:** permanent, rarely revised. Frequent revision is a signal in itself: the vision
changing is a much larger event than a requirement changing.

### Specification and user journeys

**What it is:** the description of what the product does, expressed as user journeys wherever that
fits. It carries the two scope definitions below.

**Why you need it:** it is the destination of every closeout, the source for regression testing, and
the document that answers "what is this supposed to do" without reading the code.

**Shape: one file per journey.** The specification is the artifact every feature touches, so as a
single document it is both the file no agent can hold in context and the file every closeout
collides on. Splitting it along the axis it is already written on solves both. One journey per file,
plus an index that is one line per journey:

```text
spec/
  README.md          index: journey → tier, one line each
  checkout-guest.md  tier: mvp
  checkout-saved.md  tier: mvp
  gift-wrap.md       tier: mlp
  bulk-export.md     tier: none
```

Size stops mattering because nothing reads the whole thing: a closeout reads the journeys it
changed, the taste gate reads the index. If there is contention it means two features are touching
the same behaviour, which makes resolution a disagreement to settle rather than a merge to resolve.

**Lifespan:** permanent, continuously revised. Never deleted, because it is the only artifact that
holds current intended behaviour.

#### MVP and MLP definitions

**What they are:** the **MVP** is the set of journeys without which the product is not a product.
The **MLP** — minimum lovable product — is the MVP plus what makes the product worth choosing rather
than merely usable. Both are subsets of the specification.

**Why you need them:** Obviously they serve to prevent scope creep in the early phases.
More importantly, they are what make severity a lookup rather than an argument.
P1 means an MVP
journey is broken in live and P2 means an MLP one is; either is a membership test anyone can run,
including an agent, mid-incident, under pressure.

**Where they live:** the tier is a field on the journey, and the lists are the index generated from
those fields. Both forms exist and only one of them is written by hand. That ordering is the whole
point — a curated list beside the journeys it names is a second place to update and therefore a
place that goes stale, while a field cannot drift from the journey it sits in, and adding a journey
forces someone to type a tier rather than to silently omit it.

**Lifespan:** permanent and **operational**. This is the distinction that gets missed: these are not
planning artifacts from launch, they are read during incidents, so they have to be current. A
journey that has quietly become essential and was never re-tiered means its outage is silently
scored a P2, and nobody involved will notice they are reading a stale document.

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

**Shape: two layers.** Unlike the specification, the SDD does not partition — the parts are not
independent, and the coherence it exists to protect lives precisely in the relations between them.
So it layers instead. A small top document holds the shape everything must fit: boundaries,
invariants, the patterns that are not negotiable. Beneath it sits per-component detail. The taste
gate reads the top layer every time and the component layer only for what the change touches.

The top layer has a size ceiling, for the same reason the instruction files do: it is read on every
gate, so its cost is per-invocation. Anything in it that only matters to one component belongs one
layer down.

**Lifespan:** permanent, continuously rewritten. Updated at closeout to describe the system as it
now is, not appended to. Its size tracks the size of the system rather than the age of the project —
that is the practical consequence of carrying no history, and it is what makes the growth question
an ADR question rather than an SDD one.

**Relationship to ADRs:** they answer different questions and the distinction is worth holding
firmly, because collapsing it is how both documents go bad. An **ADR** records a decision — we went
this way rather than that way, here are the alternatives, the pros and cons, and the reasoning. It
is a record of a moment, append-only, superseded but never deleted. An **SDD** records the
resulting design as it currently stands. It is rewritten so that it always reads as true today, and
it carries no history at all.

So: the ADR corpus is where you go to ask *why is it like this*. The SDD is where you go to ask
*what is it like*. An SDD that starts accumulating "previously we did X" has turned into a bad ADR
log; an ADR that gets edited to match the current design has stopped being a record.

**What catches a rewrite with no ADR:** the diff. A rewritten SDD section erases the old design from
the document, but for one commit the before and the after sit side by side in the diff — which is
precisely an ADR's context and decision. So the SDD diff generates a candidate ADR, and the
[compliance gate](feature-lifecycle.md#before-the-merge) blocks the merge until it is accepted or
explicitly rejected. Rejection is a normal outcome: a renamed heading or a clarified sentence is
editorial and says so. What matters is that saying so is an act someone performs, visible in the
log, rather than a silence nobody can distinguish from a missing decision.

Blocking pre-merge is the only moment this is cheap. The alternative is reconstructing the reasoning
days later from an agent that was not there, which is the reconstruction the ADR exists to prevent.

The weaker, standing check falls out of ADRs being attached to SDD sections: *which sections are
justified by no ADR at all* is a query rather than an audit. It catches accumulated debt rather than
the moment, and it is also what makes a skipped rollup visible, since rollup is the same link
travelling the other way.

### ADR — Architecture Decision Record

**What it is:** a record of the structure of the body of work and the principles it is designed
along, capturing the major choices made during implementation.

**Why you need it:** PRDs define requirements, which turn into the structure that executes them.
You need to remember what the structure is and why it was built that way in order to repair it or
fit new work to it.

**Contains:** the decision, its context, what was rejected, and when it would be worth revisiting.

**Lifespan:** permanent. Superseded, never deleted. This is the northern star — input and feedback
for PRDs, implementation, maintenance loops, and reviews.

**The corpus is never read whole.** ADR count grows with elapsed time rather than with the size of
the system, so an unmanaged corpus is the artifact that eventually exceeds every context window.
The corpus is not the working set, and two mechanisms separate them.

Each ADR is attached to the section of the [SDD](#sdd--software-design-document) it explains, so
*why is it like this* is a link from the design rather than a search across a decade. Superseded
ADRs then drop out of the default read set: still on disk, still findable, no longer part of what
gets read when someone touches that component.

Supersession alone is not enough, because the decisions that cause the most drag are the ones that
were never overturned — they just settled. So a set of settled ADRs is periodically **rolled up**:
the rule they arrived at moves into the SDD as design, where it belongs once nobody argues about it
any more, and the ADRs are marked settled and leave the working set. They are not edited and not
deleted; a record that is rewritten has stopped being a record. What rollup changes is what gets
read by default, not what exists.

Rollup is a deliberate act that nothing forces, which is its weak point and worth naming: a corpus
that never gets rolled up degrades slowly and silently, and the only symptom is agents reasoning
from a working set that grew past what they can hold.

**Second job:** together with the SDD, ADRs are what makes the taste gate delegable. Taste that has
been externalized is enforceable by anything that can read; taste that lives only in someone's head
is not. The share of the architecture gate that needs a human is a measurement of how much has been
written into these two documents.

### Tests

**What they are:** three artifacts wearing one name. Nobody lists tests as documentation and
everybody keeps them, which hides the fact that they do not all answer to the same document or die
of the same cause.

**Journey and e2e scenarios — the executable specification.** The same artifact as the
[specification](#specification-and-user-journeys) in a different notation. Written from a journey,
and deleted when that journey is. This is the practical argument for keeping the specification
accurate rather than aspirational: it is the source these are generated from.

**Unit and component tests — the executable design.** Their sibling is the
[SDD](#sdd--software-design-document), not the specification. They describe a module's contracts and
invariants, so they are rewritten when the design is rewritten and deleted when the module is. A
unit test that survives a redesign untouched is suspicious rather than reassuring — either the
design did not really change, or the test was asserting something other than what it claimed.

**Regression tests — a record class.** A test written from a bug reproduction is neither promised
behaviour nor chosen design; it is a failure that must not recur. Append-only, and the only test
class where deleting one requires an argument rather than a reason.

That third class closes a loop the [bug report](#bug-report) entry leaves open. The report is
transient and the fix deletes it, but its durable fact is the reproduction — and the reproduction
survives as a test. Deletion is a move, one level down.

**Lifespan:** permanent in aggregate, individually governed by whichever document each class answers
to.

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

### Instruction files

**What it is:** `AGENTS.md`, `CLAUDE.md`, and their equivalents — the place for the things agents
frequently get wrong and the things too important to leave to inference.

**Why you need it:** every other permanent artifact describes the system. This one describes how to
work on it, and it is the only artifact whose audience is the worker rather than a reader of the
product. That is also why closeout is the wrong trigger for it: a feature landing changes what the
system does, rarely how it is built.

**Contains:** rules and pointers. It routes to the specification, the SDD, and the ADRs rather than
restating them — a rule copied out of the SDD is a fourth copy that goes stale, which is the failure
this whole catalogue exists to prevent.

**What updates it:** today, a human, from memory of the last thing that went wrong. What it should
be is something watching the actual traffic — agent-to-human and agent-to-agent communication — for
patterns that recur. A correction typed three times in a week is a missing rule, and the recurrence
is the evidence. The trigger is frequency observed in the communication, not a step in the
lifecycle.

**Lifespan:** permanent, continuously revised, and **capped**. It is read on every session, so its
cost is per-invocation rather than per-revision — the only artifact where that is true. A rule earns
its place only until it can become a mechanism: a lint rule, a hook, a test, a type. Instruction →
mechanism → delete the instruction. A file that only grows is a file that stops being read.

### Observability notes

**What it is:** what needs watching about a specific feature, and what changed behaviour would mean.

**Lifespan:** until the feature is boring. Judgment, not a rule.

## Nothing is read whole

Permanence creates a second problem after lifespan, and it is the one that bites at scale: an
artifact that is never deleted is an artifact that eventually exceeds what can be read. Every
permanent artifact above therefore has a small core that is read every time and a large remainder
that is addressed on demand — but each reaches that split differently, because each grows for a
different reason.

| Artifact | Grows with | Split by |
| --- | --- | --- |
| Specification | Number of user journeys | Partition — journeys are independent, so one file each |
| SDD | Size of the system | Layering — the shape is not independent of the parts |
| ADRs | Elapsed time | Filtering — supersession and rollup shrink the working set |

The index is what makes any of the three selectable. Without one, a partitioned specification is
just a directory nobody can navigate, and a filtered corpus is a filter nobody can apply.

The failure this guards against is quiet. Documents do not announce that they have grown past
usable; agents simply start reading a fraction of them and reasoning confidently from it, and the
output looks exactly like the output from reading all of it.

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
| Bug report | Until the fix lands; repro survives as a test | The fix |
| Backport bead | Until the fix lands on main | Closing on main |
| PRD (the plan) | Until implemented | Closeout |
| Gate findings | Until fixed or promoted to a bead | The fix |
| Release branch | Until the version goes live | Going live — tagged, then deleted |
| Release tag | Permanent, while anyone can be running that version | Never |
| Beads / task state | Until closed; graph keeps history | Nothing — it is the graph |
| Vision (`VISION.md`, repo root) | Permanent, rarely revised | Never |
| Specification / user journeys | Permanent, continuously revised — one file per journey | Never |
| MVP and MLP definitions | Permanent, operational — a tier field on each journey | Never |
| SDD | Permanent, continuously rewritten to present state | Never |
| ADRs | Permanent, superseded not deleted | Never |
| Journey / e2e tests | Permanent — the specification, executable | Deletion of the journey |
| Unit / component tests | Permanent — the design, executable | Deletion of the module |
| Regression tests | Permanent, append-only — a bug that must not recur | Effectively never |
| Changelog | Permanent, append-only | Never |
| Roadmap | Permanent, continuously revised | Never |
| Operations manual | Permanent, revised | Never |
| Instruction files (`AGENTS.md`) | Permanent, revised, size-capped | A rule replaced by a mechanism |
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

- One file per journey assumes everything the product promises is a journey. Performance, security,
  and the other cross-cutting properties are not, and they belong to the specification rather than
  the SDD because they are promises rather than design. Where do they live, and what tiers them?
- If unit tests are the executable design, a redesign rewrites them — so during that rewrite the
  only thing asserting that behaviour did not change is the journey suite. Is that enough, or does a
  redesign need something the design tests cannot provide by construction?
- Rollup shrinks the ADR working set and nothing forces it to happen. Its absence is silent and its
  symptom — agents reasoning from a partially read corpus — is indistinguishable from ordinary
  error. What makes a missing rollup visible?
