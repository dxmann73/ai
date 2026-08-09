---
status: draft
tags: [aglc, artifacts, process]
---

# The feature lifecycle

The path a request takes from an unstructured wish to living documentation, and which artifact is
supposed to survive each step.

The question this note answers is not "what are the phases" — everyone has a phase diagram. It is
**where does each piece of writing end up, and what is allowed to delete it**. Most process
descriptions are vague exactly there, which is why documentation rots: nobody ever said which
document owns a fact, so the fact gets written in four places and updated in one.

Artifact names below link to [`artifacts.md`](artifacts.md), which holds the catalogue: what each
one contains, why it exists, and how long it lives.

## The shape

```text
_incoming/wishes/   → alignment → PRD ─┐
_incoming/features/ → alignment → PRD ─┤
     └ taste gate                      │
                                       ↓
_incoming/bugs/ ────────────────────→ graph → implementation → merge queue → main → closeout
     └ fast track: repro, cause,       beads    code + tests      gates      unstable   docs
       severity — no taste gate                                                │
                                                          roadmap cutover ─────┤
                                                                               ↓
                              release branch → stabilize → candidate → tag → live
                                                   │                         watch
                  _incoming/bugs/ ← backport beads ┘
```

What comes out the far end is everything that describes the system: the
[vision](artifacts.md#vision-document), the
[specification](artifacts.md#specification-and-user-journeys), the
[SDD](artifacts.md#sdd--software-design-document), the
[ADRs](artifacts.md#adr--architecture-decision-record), and the
[operations material](artifacts.md#operations-manual). What is consumed is everything that described
the *change*: the request, the requirement, the review finding. The full catalogue is in
[`artifacts.md`](artifacts.md).

Gates cluster in two places: one at intake, a dense band in front of the merge queue. They are the
same question asked twice — *should this exist* before the work, *is this the thing we meant* after
it. Both are taste, and both are the steps a pipeline under delivery pressure quietly drops first.

Bugs skip the first of those and enter the graph directly, carrying a severity. There is nothing to
decide about whether the software should work.

The loop back from the release branch is the normal way defects found during stabilization reach the
main line, and it is one of the reasons the diagram is a cycle rather than a line.

## Intake

Everything unstructured lands in a project-level `_incoming/` folder. No format required, no
argument at the door — the cost of adding must be near zero or people stop adding.

This is Steve Yegge's **wish factory** — customers and support get something to talk to, and their
wishes enter the loop rather than dying in a channel (*The Shape of Things to Come*, part one, on
`yegge.ai` — **[source needed]** for the direct URL). "Customer" here reads broadly: a paying user,
a project manager, an engineer who noticed something on the way past.

The folder trends toward empty. It is a queue, not an archive.

### Three lanes

One door, three slots, because the three kinds of arrival need different first questions:

| Lane | What it holds | First question |
| --- | --- | --- |
| `bugs/` | [Bug reports](artifacts.md#bug-report) — the software does not do what it promised | How bad, and is it really a bug? |
| `features/` | [Feature requests](artifacts.md#feature-request) — a specific thing someone wants built | Should this exist? |
| `wishes/` | [Wishes](artifacts.md#wish) — vague, unshaped, possibly not actionable at all | What is actually being asked for? |

The split matters because **only two of the three lanes get the taste gate**. A bug does not need a
decision about whether the software ought to work; that was settled when the behaviour was
specified. Sending bugs through the same admission question as feature requests is how a broken
MVP workflow ends up queued behind a discussion about whether to build something else.

Backports from the release branch land in `bugs/`, marked as backports.

The lane is a claim, not a fact. Whoever files picks the slot, and the predictable abuse is a
feature request filed as a bug to skip the taste gate — "it's broken" doing duty for "it doesn't do
what I want". Validating the lane is therefore the first thing that happens to anything in `bugs/`,
before severity and before investigation. Moving an item out of the bug lane must be cheap and
unremarkable, or the validation will not happen.

### Machines file bugs too

`bugs/` is not only a human channel. Logging, monitoring, and observability are a first-class bug
source: a log scan turns up an exception, and that files a bug even when nothing user-facing broke
and no one has complained. It gets investigated, triaged, and prioritized like any other.

This is the case the severity scale handles badly on first reading. An exception with no user impact
is outside both the MVP and the MLP, so it scores a P3, and P3 is correct — it is not urgent. The
reason to investigate it anyway is that
**severity measures current impact, not eventual cost**. An unexplained exception is a leading
indicator: its real value is the probability that it becomes a P1 later, discovered now while it is
cheap and while the change that caused it is still recent.

Machine-filed bugs also change what lane validation means. For a human filing, the failure mode is a
feature request wearing a bug costume. For a monitor, it is an alert that fires on behaviour which
turns out to be correct — and the fix for that is to the alerting, not to the code. Both are
"this is not a bug"; they route to completely different places.

*This repo runs the same pattern on itself — [`_incoming/`](../_incoming/) here holds notes rather
than requests and needs no lanes, but the mechanics are identical: free capture, deliberate
processing, and deletion once the substance lives somewhere with a longer lifespan.*

### The fast track

Items in `bugs/` do not wait for a human. An agent starts on arrival: confirm it is a bug, produce
a reproduction, find the cause, propose a fix, assign a severity from the definitions below. By the
time a person looks, the expensive part is done and what they are reading is an investigation
rather than a complaint. For machine-filed arrivals there was never a person attached in the first
place, which is where this earns most of its cost back.

Investigation runs before triage, not after, and that inversion is deliberate. Severity depends on
which workflow is affected, and that is frequently not knowable from the report — the reporter
describes a symptom. Triaging first would mean triaging on a guess. The cost is that compute is
spent on every arrival including the noise, which is the right trade while an agent hour is cheaper
than a human hour.

Two constraints keep the fast track from making things worse.

**A fix without a reproduction is a guess with a diff attached.** The reproduction is the
deliverable; the proposed fix is optional and secondary. An agent that cannot reproduce should
report exactly that and stop, rather than producing something plausible.

**Fast-tracking skips the queue, never the gates.** Cause and evidence are presented separately
from the proposal so the human evaluates the problem before the answer — and the fix
itself still passes every pre-merge gate. "Fast track" naming a shorter path through review is how
a P0 fix causes the next P0.

## The intake gate

For the two lanes that get one, a decision that is not a formality sits between intake and PRD:
**do we build this, and in which shape**. Yegge's phrasing in the same piece is that there is a lot
of taste involved, and it applies to both feature selection and architecture — we will do it this
way and not that way, and no, we will not implement this at all.

The gate is where the human's remaining leverage concentrates. Generation is cheap; deciding what
deserves generating is not. A pipeline without an explicit gate does not become faster, it becomes
a machine for producing features nobody asked to keep.

For `bugs/` the gate still runs, but its job is different: lane validation and severity, decided in
seconds rather than days. No admission question, because there is nothing to admit.

## Alignment

A surviving request is discussed until the requester and the system agree on what is actually
wanted. `grill-me` and similar interrogation skills exist for this: they force the branches of the
decision tree to be resolved out loud instead of being silently assumed by whoever writes the first
line of code.

Alignment is the expensive step and the one worth protecting. Everything downstream inherits its
errors.

## PRD

Alignment output goes into a [PRD](artifacts.md#prd--product-requirements-document) — the *what* and
the *why*, not the *how*. The PRD is the plan; nothing else gets written between agreeing what is
wanted and decomposing it into work.

It also names which documentation artifacts this work is expected to change. These obligations
decompose into beads with everything else, so the doc change lands in the same merge as the behaviour.

The PRD is **transient by design**. It exists to be turned into work and then dissolved: its
permanent parts are redistributed at closeout (see below) and the remainder is deleted. Keeping a
PRD around after implementation guarantees that an agent will one day find it and reason from
requirements that were superseded eighteen months ago.

## The graph

Once alignment is achieved the PRD is mapped into [beads](artifacts.md#the-graph) so agents can
start — the graph is the PRD in executable form, and that mapping is what ends it. Task state,
sequencing, dependencies, and what is ready to work on live in the graph and nowhere else — the
split is the whole reason for adopting it
([`adoption/journey/beads-adoption.md`](../adoption/journey/beads-adoption.md)).

One bead is special. **The last bead in the chain is the closeout bead**, and it is written when
the feature is decomposed, not bolted on afterwards. If closeout is not a tracked unit of work with
a dependency edge, it does not happen — this is the same failure mode as the test that was going to
be written later.

## Implementation

Ordinary agentic work against the ready queue: TDD, verification loops, review passes. A bead
carries its own requirement, so there is nothing to plan on top of it — whatever an agent writes for
itself inside a session is session scratch and dies with the session.

Work of any size reads the [SDD](artifacts.md#sdd--software-design-document) going in, because it is
the account of the shape the work has to fit. Work large enough to change that shape extends it, and
that extension is written before the slices are implemented rather than reconstructed afterwards —
which is the whole point of having one, since independently implemented slices are exactly what
risks incoherence.

"The agent says it is done" is not an exit condition. It is the point at which the gates start.

## The exit gates

More gates sit here than anywhere else in the lifecycle, and that is the correct distribution.
Generation is cheap and verification is the bottleneck, so the process should be thin where work is
produced and thick where it is judged. Each gate has an owner, a failure mode it exists to catch,
and somewhere specific that a failure sends the work back to.

They split around the merge, and the split only makes sense once one assumption is abandoned,
stated first because everything below depends on it.

### Main is not stable

The convention worth dropping is that the main line is green. It cannot be, at volume: when many
agents merge many features a day, the window in which main is simultaneously integrated, tested,
and correct closes. Defending that window means serialising the merges behind the slowest suite in
the build, which is how a pipeline stops being fast.

So work takes a slot in a **merge queue** and merges (Yegge, *The Shape of Things to Come*, part
one — **[source needed]** for the direct URL). Main is the integration point and the record of what
has been accepted. It is not a promise that anything works, and no process step should be written
as though it were.

Stability is not abandoned, it is relocated: it moves to a release branch, below. Main is where
work accumulates; the branch is where it is made true. The consequence for everything after the
merge queue is that **nothing running on main is a gate** — it produces signal and beads, and it
blocks nobody.

### Before the merge

1. **Compliance.** Build, lint, unit tests, CI green. Fully automated, no human, no negotiation.
   Agents forget the boring things constantly, so this runs before anything else looks at the work.
   Fails back to the same bead.
2. **Code review in a fresh context.** A reviewing agent that never saw the implementation
   transcript. Reviewing your own work with the reasoning still in context is not review, it is
   proofreading — the same rationalizations are still loaded.
3. **Stacked review lenses.** No single lens catches everything, but decorrelated ones stack: a
   reviewer given the worker's full transcript, one given only the output, one given nothing but the
   codebase, and reviewers running on different models with different training and different
   temperaments. Cursor reports this stacking as a major contributor to sustained quality across
   long autonomous runs, and the economics are unarguable — review is much cheaper than the work it
   audits ([agent swarm model economics](https://cursor.com/blog/agent-swarm-model-economics)).
4. **Architecture review — the taste gate.** Does this fit the shape of the system, or does it
   merely work? Read against the [SDD](artifacts.md#sdd--software-design-document) for what the
   shape is, and the [ADRs](artifacts.md#adr--architecture-decision-record) for why it is that
   shape. This is the second appearance of taste, and the one most often skipped because the tests
   are green and nothing appears to be wrong. What it catches is the class of change that is
   individually fine and collectively corrosive: the duplicated abstraction, the fallback that
   papers over an unclear design, the module that now knows about something it should not. Outcome
   is a verdict, and one legitimate verdict is *rebuild it differently*.

   This gate is delegable to an agent, and the thing it depends on is the guard rails. Taste that
   has been externalized — a written SDD, recorded ADRs, stated invariants — is enforceable by
   anything that can read. Taste that lives only in someone's head is not, and no amount of model
   capability fixes that, because there is nothing to check against. So the degree to which this
   gate needs a human is a measurement of how much has been written down, and it is a number that
   goes down over time if those two documents are kept honest. What does not delegate is the
   escalation: an agent can find that a change violates the design, but deciding the design itself
   was wrong is a different act, and it belongs back at the intake gate.
5. **Blast radius.** Which existing features does this touch or alter? Those get re-verified, not
   assumed. The bug in the changed code is the easy one; the bug is usually next door
   ([Nolan Lawson on reviewing what AI touched](https://nolanlawson.com/2026/05/25/using-ai-to-write-better-code-more-slowly/)).
6. **Feature smoke.** The feature's own end-to-end journeys, and only those. Not the full
   environment and not the whole suite — enough that the change has demonstrably run once outside a
   mock. Its entire purpose is to keep the next gate from being spent on something that was never
   executed.
7. **Human review.** The last gate before merge, and deliberately last: everything mechanical has
   already been eliminated, so the human's attention lands on judgment rather than on lint. A
   bottleneck by design — the job is to keep it narrow and well-supported, not to remove it.

### On main, after the merge

None of these block anything. They run continuously against a line that is expected to be broken
some of the time, and their output is beads.

- **Whole-picture review.** A fan-out of agents looking at the codebase rather than any one diff:
  cyclomatic complexity, naming coherence, security, architectural drift. Findings become beads or evaporate.
- **Integration and e2e as signal.** Worth running on main anyway, to shorten the distance between
  a defect being introduced and being noticed. A failure here is a bead, and the
  useful thing it carries is the merge that caused it, which is cheap to identify now and expensive
  to identify at cutover.

### The release branch

Stability is produced deliberately at a chosen moment rather than maintained continuously. The
branch and its full lifecycle are in [`releasing.md`](releasing.md); what matters here is where it
attaches to the flow.

1. **Cutover.** The [roadmap](artifacts.md#roadmap) decides when: this is what we want to deliver. A
   [release branch](releasing.md) is cut from main at that point.
2. **Stabilization loop.** The branch runs its own loop: full integration environment, full e2e,
   every user journey, fix, repeat, until green. Each green head is a release candidate; nothing new
   lands here.
3. **Release.** Notes, version, deploy, and then watch this version specifically — report and
   analyse changed behaviour immediately rather than waiting for someone to complain.
4. **Promotion, and the life after it.** Going live is tagging the candidate.
   The release branch stays open for patches for as long as that version is in the field,
   each patch stabilized and tagged the same way.
5. **Backport.** Every fix made on a release branch travels back to main as a
   [backport bead](artifacts.md#backport-bead) carrying both the error and the fix, entering through
   `_incoming/bugs/`. Beads outlive branches: they live in the graph and carry the fix as text.

That last step is the load-bearing one, and it is **not a cherry-pick**. Main has moved since the
cutover, so the patch that worked on a frozen
branch may not apply, may apply and be wrong, or may collide with a change that arrived in between.
Sending the defect back as a bead with a known cause and a known remedy lets it be re-derived in
main's context and pass through the ordinary gates. Slower per fix, and it is the difference
between one codebase and two.

The bead is also the identity of the defect across both lines. Without it, the branch fix and the
main fix are two unrelated pieces of work, and nothing detects the case where one landed and the
other quietly did not.

Routing backports through `_incoming/` keeps one door for everything, which is worth something on
its own. The cost is that the intake gate now sees items whose *should this exist* question was
answered on the branch, under release pressure. They should be marked as backports and treated as
formalities there — a backport that sits in intake waiting to be judged is a fix that shipped to
users and not to main.

Formality on admission, though, not on triage. The gate still has one job for a backport, and it is
the subject of the next section.

### Priority beats order

Backports are bugs and get treated like any other bug, on either line: **prioritized by user
impact**. P0 and P1 are fixed and merged immediately, ahead of everything queued. Lower priorities
join the ordinary flow and take their turn.

This is what replaces a queue-level gate on the cutover. The backport queue as a whole never blocks
a branch being cut; individual severity does. An open P0 is a stop. An open P3 is inherited by the
next branch and that is an acceptable outcome, because low user impact is precisely what the label
means.

### The severity definitions

Severity is not a feeling. It is a lookup against defined product scope:

| Level | Definition | Anchored to |
| --- | --- | --- |
| **P0** | Everything is on fire. Nothing works, or a large fraction of users are affected. | Blast radius |
| **P1** | One of the **MVP** workflows does not work in live. | MVP definition |
| **P2** | Something in the **MLP** beyond the MVP does not work. | MLP definition |
| **P3** | A defect outside both, including anything with no user-facing effect at all. | Neither |

P0 and P1 measure different things — P0 is breadth across the system, P1 is depth against a named
workflow — so the scale is not a single ordering and should not be forced into one. A single MVP
workflow down for every user is the boundary case, and the useful question there is whether the
rest of the system is still standing.

P3 is the inference that completes the scale rather than something inherited from the definitions
above it; if the intent is that everything outside the MLP is simply unprioritized, that is a
different and also defensible model.

The scale only works because the two scope definitions it points at are real, written artifacts:
the [MVP and MLP definitions](artifacts.md#mvp-and-mlp-definitions), both permanent, both living in
the specification as a tier field on each user journey.

The payoff is mechanical. P1 stops being an argument and becomes a set membership test: is this
workflow on the MVP list? Anyone can check, including an agent, in the middle of an incident, under
pressure, without waking anybody up.
For the fast track it means severity assignment is a lookup an agent can do, leaving the human only the
cases the lookup does not settle.

Drift does not disappear, it relocates. Scope creeps: over time everything becomes MVP, everything
becomes a P1, and the scale flattens exactly as it would have without the anchor. The improvement
is that the creep now happens in a version-controlled document a human argues about deliberately,
instead of in a thousand individual triage decisions nobody reviews. Slower, and visible. Not
cured. The obligation that comes with it — keeping both definitions current enough to be read
during an incident — is in [`artifacts.md`](artifacts.md).

### Rebase notification

When a fix merges — to the release branch or to main — every agent with work in flight is now
building on a base that is known to be wrong. They are told to rebase. This is a push, not
something an agent is expected to discover.

Severity grades the response as well as the fix. A P0 or P1 means rebase now, mid-work, because
continuing means either re-introducing the defect or colliding with its remedy. Lower priorities
mean rebase at the next natural boundary, where the cost of the interruption exceeds its benefit.

Two things stop this from being a stampede.

**Notify onto a commit, not onto a branch.** Twenty agents told to "rebase onto main" chase a line
that keeps moving, and some of those rebases are invalid before they finish. The merge queue
already serialises landings, so it can name the target: rebase onto this commit. The target holds
still even if main does not.

**Do not make the working agent resolve the conflict.** An agent mid-implementation, holding a
large diff and its own reasoning, is the worst-placed party to absorb someone else's change — in
practice it either overwrites the other work or abandons its own. Cursor's answer was a neutral
third-party agent whose only goal is to be impartial and efficient, in the way a merge queue is
([agent swarm model economics](https://cursor.com/blog/agent-swarm-model-economics)). The same
answer applies here, and it applies hardest exactly when the rebase is urgent.

### Why this order

Ordering protects the scarcest input at each point, and there are two scarce inputs, not one.
Machine cost — CPU, infrastructure, wall clock — and human attention. They do not rank together,
which is why "cheapest first" is too crude a rule on its own.

Before the merge, human attention is the scarcer of the two, so the gates in front of it exist to
make sure it is never spent on something a linter, a reviewing agent, or a single smoke run would
have rejected. On the release branch nobody is queued behind the work, so machine cost stops
competing with anything and the expensive suites can take as long as they take.

That is the real reason full integration is not a merge gate. In front of the queue, an environment
spin-up blocks every branch behind it and the people attached to them; on the release branch it
blocks nothing that was going anywhere.

The load this shifts is onto the pre-merge gates. They are now the only quality signal that acts on
main at the moment work lands, and everything they miss becomes stabilization cost at cutover,
where it is discovered furthest from the change that caused it. A cheap merge queue does not mean a
cheap process — it means the expense moved.

Findings from any gate are transient. They are either fixed immediately or converted into a bead;
they are never a document that accumulates.

The reason a finding becomes a bead rather than a message to the working agent is context. The agent
that produced the code is in the middle of something, and a finding pushed into its window
sidetracks it, so we save it for later. Findings
from the feature's own gates are worked in the same iteration as the PRD beads that produced them.

## Closeout

Closeout exists to make sure documentation is clean and up to date. It reads the original request
and the PRD next to what was actually built. That gap, intent versus outcome, is the thing nobody
usually has time for, and it is the whole reason to make it a bead.
It is written when the feature is decomposed, before any of the work starts, so the checklist exists
before there is anything to check.

Its checklist, each item an obligation the PRD named and a bead should already have discharged:

- Fold the implemented behaviour into the
  [specification](artifacts.md#specification-and-user-journeys), as a user journey where that fits.
- Rewrite the affected parts of the [SDD](artifacts.md#sdd--software-design-document) to describe
  the system as it now stands. Rewrite, not append — the SDD carries no history, so a design that
  changed leaves no trace of what it used to be.
- Record the major choices made during implementation as
  [ADRs](artifacts.md#adr--architecture-decision-record), including the ones the SDD rewrite just
  erased the evidence of. Structural decisions taken while the code was being written are exactly
  the ones that get lost, and the SDD is now the document that loses them.
- Update the [e2e scenarios](artifacts.md#tests), the
  [changelog](artifacts.md#changelog), the [roadmap](artifacts.md#roadmap), and the
  [operations manual](artifacts.md#operations-manual).
- Re-verify neighbouring documentation. What did this change make wrong? Stale docs are worse than
  missing ones, because an agent believes them.
- Reference the originating bead from each document it touched, so provenance survives the deletion
  of the PRD.
- Note the deployment risks and what needs
  [observing on this feature](artifacts.md#observability-notes) specifically. The go-live date is not
  knowable here — it belongs to whichever cutover picks the work up.
- Notify the requester when it goes live. The wish factory only keeps working if wishes come back.

An obligation that turns out not to have been discharged becomes a bead like any other finding.

Then the feature request and the PRD are deleted. They have no remaining content that is not held
somewhere durable, and that deletion is closeout's own diff.

### Closeout under a swarm of agents

Documentation landing inside the feature's own merge is what makes this survive volume. There is no
separate documentation step for several features to contend over — the doc change is in the diff,
and the merge queue serialises diffs already. A second feature touching the same user journey hits
an ordinary conflict on an ordinary file and rebases, exactly as it would for code.

## Where things live

Every artifact named above — what belongs in it, what it is for, and how long it is allowed to
live — is catalogued in [`artifacts.md`](artifacts.md), together with the lifespan table and the
reasoning about why the deletions matter.

The rule the whole lifecycle rests on: **a document is deleted only once every durable fact it
holds lives somewhere with a longer lifespan.** Deletion is a move, not a loss.

## Open questions

- Who runs the intake gate when the requester is a customer rather than a colleague, and how does a
  "no" travel back out without burning the channel?
- An investigation arriving with a proposed fix anchors the reviewer on that fix. Separating
  evidence from proposal is a convention. Is there a mechanism?
