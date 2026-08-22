# Outline — the artifact is the unit

> Drawn from [aglc/artifacts.md](../aglc/artifacts.md),
> [aglc/feature-lifecycle.md](../aglc/feature-lifecycle.md).
> Post: none yet.

Working title. The argument is that most process descriptions are vague about the one thing that
matters, and being precise about it changes the process.

## The claim

Everyone has a phase diagram. Nobody says which document owns a fact, so the fact gets written in
four places and updated in one. The useful question is not "what are the phases" but **where does
each piece of writing end up, and what is allowed to delete it**.

The rule that falls out: *a document is deleted only once every durable fact it holds lives
somewhere with a longer lifespan.* Deletion is a move, not a loss.

And the sorting rule underneath it: **what survives describes the system; what is consumed describes
a change to the system.** Vision, specification, SDD, ADRs, ops manual survive. Request, PRD, plan,
review finding are consumed. Once the change lands, its description is redundant with the system's
own description, and keeping both is how they drift.

## Beats

1. **Two opposite failures, one root.** Keeping transient artifacts means agents reason from
   requirements superseded eighteen months ago. Deleting permanent ones means the reasoning is gone.
   Both come from task state and reasoning sharing a file. The throwaway-plan story from
   `adoption/journey/beads-adoption.md` is the concrete version — 44 plan files deleted in the
   commits that finished the work, and the rule reversed sixteen weeks later.
2. **ADR versus SDD is the sharpest case.** The ADR asks *why is it like this* — a record of a
   moment, append-only, superseded never deleted. The SDD asks *what is it like* — present state,
   rewritten so it always reads as true today, carrying no history. Collapse the distinction and
   both go bad: an SDD accumulating "previously we did X" has become a bad ADR log; an ADR edited to
   match the current design has stopped being a record.
3. **Verification is where the process should be thick.** Generation is cheap. Seven gates before
   the merge queue, ordered so the scarce input at each point is protected — and there are two
   scarce inputs, machine cost and human attention, which do not rank together.
4. **Main is not stable, and that is the point.** Merge queue, unstable main, stabilization
   relocated to a release branch cut at a roadmap cutover. Nothing running on main is a gate. The
   expense does not vanish, it moves: pre-merge gates become the only signal acting on main when
   work lands, and what they miss becomes stabilization cost discovered furthest from its cause.
5. **Taste is delegable in proportion to what has been written down.** Taste externalized into an
   SDD and ADRs is enforceable by anything that can read. Taste in someone's head is not, and model
   capability does not fix it, because there is nothing to check against. Which turns "can an agent
   do architecture review" from a philosophical question into a measurement.
6. **Severity as a lookup, not a feeling.** P1 means an MVP workflow is broken, and the MVP is a
   written part of the specification. Set membership, checkable mid-incident by anyone including an
   agent. The honest caveat: drift does not disappear, it relocates into scope definition, where it
   is slower and visible instead of hiding in a thousand unreviewed triage calls.

## The turn at the end

This repo now applies the rule to itself. The folder that used to archive every processed piece
forever is gone; entries are deleted once their substance lives in a note or a published post. A
superseded note is worse than no note — retrievable, plausible, and misleading, and an agent that
finds it will believe it.

That is the test of whether the argument is any good: it cost something to follow.

## Not in the post

- The full gate list. It is a reference, it belongs in the notes, and it would flatten the piece.
- The backport mechanics and the rebase-notification detail.
- Every open question. Pick at most two, at the end, as genuinely open rather than as rhetoric.
