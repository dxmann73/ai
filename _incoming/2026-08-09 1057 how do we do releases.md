# How do we do releases

Research capture for `aglc/feature-lifecycle.md` — the "release branch → stabilize → release →
watch" box is four words wide and carries the most operational weight in the diagram. Three
sources: block/buzz as a worked example, Yegge on why CI/CD breaks at agent volume, and what this
repo already says.

## Found source: the Yegge URL the lifecycle note marks `[source needed]`

`aglc/feature-lifecycle.md` cites *The Shape of Things to Come*, part one, three times with
**[source needed]** for the direct URL. It is
[yegge.ai/essays/the-shape-of-things-to-come](https://yegge.ai/essays/the-shape-of-things-to-come/).
Part one is subtitled *The Continuous Thunderdome*. Fix those three citations when this gets
integrated.

## block/buzz — [RELEASING.md](https://github.com/block/buzz/blob/main/RELEASING.md)

A real release doc for a multi-artifact repo. Not agentic in itself, and that is what makes it
useful: it is the machinery an agent would have to operate.

**Three independent lanes, versioned independently.** Desktop (`just release-desktop <version>` →
packaged app), relay (`just release-relay` → `ghcr.io/block/buzz` image), mobile
(`scripts/mobile-release.sh candidate X.Y.Z` → `mobile-vX.Y.Z-rc.N` tag). Each lane has its own
version authority: `desktop/package.json`, `crates/buzz-relay/Cargo.toml`, the mobile tag itself.
No monorepo-wide version number. Worth noting against the lifecycle note, which quietly assumes one
release branch for one product.

**The immutable candidate.** Desktop generates one deterministic candidate commit, records its
frozen base and the prior release ledger in candidate metadata, and opens a PR. Regenerating the
branch creates a *new* candidate and reruns every check. The squash merge is the human
authorization event. A verifier then re-derives the tag at the exact reviewed PR head — not the
squash commit — and proves every required check came from its trusted producer and was green *at
merge time*. A post-merge check rerun deliberately makes tagging fail closed.

That last detail is the interesting one. The release doesn't trust the merge; it re-proves the
merge. "CI was green" is stored as evidence attached to a specific SHA rather than believed as a
branch property. That is exactly the shape a gate needs if an agent is the one merging.

**No stabilization line at all for mobile.** Explicit trade, written down: "The simplification
trades away a separate stabilization line. Unrelated commits that reach `main` become part of every
later candidate, and there is no retained hotfix branch. Add a dedicated hotfix flow later if a
release actually needs isolation from `main`." Candidates are cut from remote `main`, an RC is
never moved, and iOS and Android may ship from *different* RC tags for the same marketing version.
There is intentionally no single final candidate.

So buzz's mobile lane is the direct counter-model to the lifecycle note's release branch: instead
of freezing a line and backporting, it cuts a new immutable candidate from main every time. Cheaper
to operate, and it only works because main is expected to be shippable-ish and the RC is disposable.

**Permissions are part of the design.** A dedicated `buzz-release-bot` GitHub App is the sole
always-bypass actor on the protected tag ruleset; direct human tag creation is denied. Operators
can push a candidate branch; only the App can mint the release tag. Authority to release is a
separate identity from authority to write code — the same "agents get their own keypair" instinct
as the buzz ACP note in `_incoming/`.

**Retry rules are stated as prohibitions.** "Do not recreate, move, or push the immutable tag
again." "Do not update the branch manually and do not weaken the ruleset." A whole troubleshooting
section written as *what not to do when it fails*, which is the part agents get wrong under
pressure and the part most release docs omit.

## Yegge — [The Shape of Things to Come, part 1][shape]

[shape]: https://yegge.ai/essays/the-shape-of-things-to-come/

**The arithmetic.** 100 devs × 1 commit/day × 30-min build = 50 hours of serial builds per day.
Merge queues fix this by batching — ten batches of ten is a 10x saving — but a red batch costs a
bisection, log(N) rebuilds, and hours added to the queue. Yegge reports ~175 real commits/day this
month on Wheelhouse, up to 250, against a ~30-minute build gate, with 40+ agents running around the
clock. His MQ passed 100 MRs and stopped making forward progress, stuck in bisection loops.

**The Pigeonhole framing**, which is the quotable part: "if you have more pigeons than holes, some
hole ends up holding more than one pigeon. Once your commit rate outruns your build slots, one
commit per green build becomes mathematically impossible. Agents multiply the commit rate by orders
of magnitude, while your build time stays fixed."

**The Land Rush.** When the MQ hits 100, abandon bisection, smash everything in as one megabatch,
then *swarm diagnosis* — not bisection — to fix main. The finding behind it: agents diagnose
red-main faster than bisection isolates it. He reports clearing batches of 120–150 commits, and
calls it clearly the future after about a week of daily use.

**Game DevOps.** Corroboration from a senior dev at a London SaaS shop, ex-game-industry: long
builds, huge asset pipelines, C++ link times, so no MQ scheme works. Everyone blasts commits to
main, they cut a release branch and roll with it, fixes on the branch propagate to main, and main
generally stays red. Multiple times per day. Yegge's note: Perforce's own game-dev material says
HEAD is never stable at AAA scale.

That is the origin story for "main is not stable" in the lifecycle note, and it arrives with the
release-branch half attached — which the lifecycle note already has. The piece the lifecycle note
does *not* have is the megabatch: it still assumes a merge queue that serialises. Under Land Rush
the queue stops being a gate at all.

**Release as a walkable chain.** From [Welcome to Gas
Town](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04): the Beads release process
is 20 steps, and agents "would *always* skip steps" because of long wait states — GitHub Actions,
CI, artifact deploys. The fix was to make the 20 steps 20 beads, chained in order, and have the
agent walk the chain one issue at a time. Side effect: the claim/close events *are* the activity
feed. Later refinement — the worker is decommissioned while the molecule sits in a Gate state
awaiting CI, and a fresh one is woken when the gate trips.

This is the concrete answer to "how does an agent do a release": not a prompt, a dependency graph
with gates, where waiting costs nothing because nobody is sitting in the wait.

**Watching the release is a standing role, not a step.** Yegge's prod constellation has the
Gargoyle (SRE), the Drawbridge (deploy-red monitor), the Warden (abuse monitor), the Herald (patch
notes). And the wiring rule underneath: **crons watch, models act** — ~45 launchd/systemd units
that wake an agent when something needs judgment.

## What this repo already has

- `aglc/feature-lifecycle.md` — the release branch section: cutover picked by roadmap not build
  state, a stabilization loop closed to features, release (notes, version, deploy, watch), and
  backport as a **bead** rather than a cherry-pick. Plus the P0–P3 severity table anchored to
  written MVP/MLP artifacts, and the rule that an open P0 stops a cutover while an open P3 is
  inherited by the next branch.
- Closeout runs at merge, not release — with one item deferred: notify the requester when it goes
  live. So a release already has to fire callbacks left behind by beads that closed cycles ago.
  Nothing in the note says what carries that.
- `_incoming/aidlc.md` under "Live prep and push": do ALL user journeys, watch this version in
  prod, report and instantly analyse changed behaviour. And "Release notes; new version" filed
  under after-merge review.

## Where the sources disagree

The lifecycle note and buzz-mobile give opposite answers to the same question, and both are
defensible:

| | Freeze a line | Cut a fresh candidate |
| --- | --- | --- |
| Model | lifecycle note, Game DevOps | buzz mobile |
| Stabilization | on the branch, closed to features | none; ship a later RC |
| Fixes reach main | backport bead, re-derived | already there — RC came from main |
| Cost | two lines to keep honest | every RC inherits unrelated commits |

The deciding variable is probably how long stabilization takes relative to the cutover interval —
which is already an open question in the lifecycle note ("two live release branches is the failure
this model has instead of a broken main").

## Open questions

- Land Rush versus the merge queue: the lifecycle note's pre-merge gates assume a queue that
  serialises landings and can name a rebase target commit. If the queue becomes a megabatch, what
  does rebase notification point at?
- Does the release itself get a bead graph, buzz-style — 20 chained steps with gate beads — or is
  it a molecule that lives outside the feature lifecycle entirely? Cheap answer: it is the one
  workflow that is identical every time, so it is the best possible candidate for a protomolecule.
- Who is allowed to release? buzz answers with a separate GitHub App identity that humans cannot
  bypass. The lifecycle note has no notion of release authority at all.
- Release notes are listed as a step but nobody owns the input. Closeout writes the changelog at
  merge; the release assembles notes from everything since the last cutover. Is that assembly, or
  is it writing?
- Multi-artifact repos: three lanes with independent versions breaks the assumption of one release
  branch. Does the lifecycle generalise, or is "one product, one line" a stated precondition?
- Post-release watching is a standing role in Yegge's setup and a bullet point in the lifecycle
  note. What actually ends the watch — a timer, a metric, or the next release?
- "Fail closed" as a release property: buzz refuses to tag rather than tagging something it cannot
  prove. Which of the lifecycle's gates currently fail *open* if their evidence is missing?
