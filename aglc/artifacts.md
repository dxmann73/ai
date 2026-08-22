---
status: draft
tags: [aglc, artifacts]
---

# Artifacts

TODO neue Artefakte:

- proof of work als Video oder Screenshots

- BDD Tests und UI Tests als Teil der PRDs
- Architekturplan als dauerhaftes Artefakt, welches interaktiv sein kann
- Architekturtests als deterministische Variante (ArchUnit?)

Spezifikation: nicht im Sinne von „der User sieht die folgenden drei Felder, das erste Feld ist ein
Pflichtfeld, der User klickt auf den OK-Button, der Dialog schließt sich“ (es ist sichtbar ein XY).
Das ist nicht handelbar, zu beweglich, kann nicht aufrecht erhalten werden, muss immer in sync
gehalten werden, der Code ist sowieso die Wahrheit und kann und wird abweichen. Was ich damit meine
ist eine Liste/Beschreibung der Features, die ich vom Produkt erwarte, also "User sind in der Lage,
dies oder das zu tun". Das ist, was ich mit Spezifikation meine. Sowohl Uncle Bob als auch Matt
Pocock stimmen darin überein (und ich stimme dem ausdrücklich zu), dass die Spezifikationen wie im
ersten Sinne wenig bis überhaupt keinen Sinn machen und auch nie gemacht haben, als das wass wir
früher Grob- und Feinkonzept nannten und was regelmäßig den ersten Feindkontakt nicht überlebt hat
(TODO Verlinkung Sun Tsu).

## TLDR

Describes the documents the aglc produces, their purpose, content and lifecycle. Companion to
[`feature-lifecycle.md`](feature-lifecycle.md) which describes when/how they are used. Two types of
artifacts: transient for plans/ideas and implementation, permanent for
alignment/description/processes.

Three axes:

- Specification defines everything the product is supposed to do, in prose
- Code is the artifact (hopefully) reflecting this description
- Tests are the mould against which the code keeps being verified to work

## Transient artifacts

### Wish

**What:** unshaped request — support-channel line, overheard complaint, half-formed objection.
**Why:** alternative is: it dies unsaid. The wish factory only works if adding costs ~nothing.
**Where:** Lives in `_incoming/wishes/` **Contains:** whatever the person/agent had. No required
format. **Lifecycle:** created by personas, needs to be scrutinized, may become a bug or feature
request.

### Feature request

**What:** a specific ask, shaped enough to argue about. **Why:** this is the main driver of product
development. **Where:** Lives in `_incoming/features/` **Contains:** the want, its context, ideally
who to notify when the feature lands. **Lifecycle:** scrutinized, then is transformed into a PRD and
deleted.

### Bug report

**What:** claim from people and/or machines (LMO) that software fails an existing promise. **Why:**
separate/automated intake track **Where:** Lives in `_incoming/bugs/` **Contains:** on arrival, a
symptom. After intake: repro, cause, severity, maybe proposed fix. **Lifecycle:** symptom, then
repro and severity. Analysis of cause, proposed fix. Fix deletes report; repro moves into suite as
[regression test](#test-concept).

### Backport request

**What:** defect found in live/[release branch](releasing.md) that needs to travel back to main.
**Why:** main moved since cutover, so fix is re-derived in main's context rather than transplanted.
**Where:** Lives in `_incoming/backports/` **Contains:** error, fix as made on branch, severity,
reference to source branch work. **Lifecycle:** like a bug. deleted when fix lands on every line it
owes — main and current RCs.

### PRD — Product Requirements Document

**What:** a specific product outcome — _what_ and _why_, not _how_. Replaces the plan doc. **Why:**
if you were building a city, this is the plans for one building: plumbing, wiring, materials, cost,
timeframe. You want it before hiring contractors and buying machines. The PRD is being created in
tandem with the agent asking questions, ultimately achieving alignment. **Where:** Ephemeral, lives
in `_incoming/prds/` if needs iterations **Contains:** context, assumptions, goals; personas, use
cases; functional/non-functional requirements; success metrics; risks, dependencies, non-goals, open
questions; and documentation obligations. **Lifecycle:** feature+questions = alignment. The result
is turned into a PRD, which is mapped to an issues graph — beads and edges are the PRD in executable
form, including documentation obligations.

### Gate findings

**What:** output of any ongoing work, issues arising from reviews before gates. **Why:** keeps the
finding out of the working agent's context as to not sidetrack it. **Where:** As beads.
**Contains:** the finding(s), with information if they apply to main or just the current work.
**Lifecycle:** findings become beads immediately, usually worked on in the same iteration as PRD
beads, not deferred to cleanup.

## Permanent artifacts

### Vision document

**What:** high-level product vision answering _why does this exist_. **Why:** aligns the overall
goal with the agent. **Where:** `VISION.md` in the repo root. **Contains:** Outline of what's being
built, for whom, what it should/shouldn't do. **Lifecycle:** permanent, rarely revised.

### Product Specification

**What:** describes what the product does, its features, journeys, api, agent surface **Why:** there
needs to be a place that keeps track of all intentions/plans that went into it and how/for what they
are used. ALso Informs regression and e2e testing. Answers "what's this supposed to do" without
reading code. **Where.** under docs/spec, with README.md as index, split along several axes:

```text
spec/
  README.md                   index
  features                    => feature axis
  features/add-to-cart.md     => maps to tier and user journey
  journeys/
  journeys/checkout-guest.md  => maps to tier (e.g. MVP) and feature
  journeys/checkout-saved.md  => same for MLP
```

**Contains:** features, journeys, tier descriptions, assignment to index of MVP and MLP
**Lifecycle:** Amended and revised every time a feature/bug/wish lands on main.

#### Features

**What:** description of a feature **Why:** splits the specification into parts to not clog up the
context. **Where:** under `docs/features` **Contains:** granular feature descriptions **Lifecycle:**
see specification

#### User journeys

**What:** description of a user journey **Why:** focus on the user perspective (combination of
features). **Where:** under `docs/journeys` **Contains:** step by step description for the users,
what the product enables them to do. **Lifecycle:** see specification

#### MVP and MLP definitions

**What:** **MVP** (minimum viable product) = features/user journeys without which the work is not
really useful. **MLP** (minimum lovable product) = MVP plus addons that makes it worth choosing.
**Why:** prevents early scope creep. informs prod issue severity (P1 = MVP broken, P2 = MLP broken).
**Where:** part of the specification. **Contains:** bespoke lists MVP/MLP on which tell a story by
themselves, kept in sync with journeys/features. Mapped by field `tier` on both journeys and
features **Lifecycle:** permanent and **operational**. read during incidents, so must stay current

#### TODO API spec

#### TODO Agent interface

MCP Server, AGENTS.md, skills, ...

### Planning and delivery artifacts

### Roadmap

**What:** planning artifact with outlined what's intended to release, and in what order. **Why:**
overall coordination. Features may be p4 but still pulled onte the roadmap. **Where:** tbd
**Contains:** tbd. Should be beads? **Lifecycle:** permanent, continuously revised.

### Changelog

**What:** what changed, per release. **Why:** Have a means to know which features have been released
**Where:** `CHANGELOG.md` in project root **Contains:** Feature/version matrix with links to the
specification **Lifecycle:** permanent, append-only.

### Engineering / Architecture Documentation

**What:** general documentation of the architecture, split up into multiple pieces, similar to the
specification **Why:** focus on the user perspective (combination of features). **Where:** under
`docs/journeys` **Contains:** general documentation of the architecture, SDDs, ADRs, description of
architecture spanning frontend, backend, persistence, infrastructure, integrations, security,
observability **Lifecycle:** Amended and revised every time a feature/bug/wish lands on main.

#### SDD — Software Design Document

**What:** how the system is designed **as it now stands**— patterns, shape, how parts fit. **Why:**
the parts in a PRD pertaining to architecture need to be kept as documentation as well as important
input to the taste gate, i.e. to answer "does this fit the system's shape". Input/feedback for PRDs
**Where:** under `docs/architecure/SDDs` **Contains:** description of the parts of the system as it
stands now, patterns, data model, APIs, flows, services. **Lifecycle:** see engineering /
architecture documentation

### ADR — Architecture Decision Record

**What:** record of the principles behind the work's structure; major choices made during
implementation. **Why:** PRDs define requirements, which turn into the structure that executes them.
Need to remember what and why to repair or extend it, i.e. input for the "taste" gate **Where:**
under `docs/architecure/ADRs` **Contains:** architecture decision and background: why this way not
that way, alternatives, pros/cons, the reasoning behind it, decision, when to revisit.
**Lifecycle:** permanent. may be deleted when superseded.

### Test Concept

**What:** definition of the test classes and how they are handled throughout, especially wrt/ gates.
**Why:** needed for alignment on testing both during implementation and execution/stabilization.
Closes the loop the [bug report](#bug-report) leaves open: report is transient, fix deletes it, but
its durable fact — the repro — should usually survive as a test. **Where:** under `docs/testing`
**Contains:** Description of the test classes and how tests are to be written and executed
**Unit/component tests:** Describe how a module's contracts/invariants are to be tested, i.e.
container based and/or mocked, which level of integration, which tools to use for it. **Regression
tests.** Describes what tests should emerge from feature descriptions and bug repros. Defines a
regression test suite and how it is to be executed and kept in sync with the work. **Journey/e2e
scenarios.** Mirrors the [specification](#product-specification). Written from the perspective of a
user journey or feature, it describes examples and steps to verify these still work. **Lifecycle:**
see engineering / architecture documentation

### Operations manual

**What:** how to run the product — deployment, recovery, runbook for known failures, observability
notes. **Why:** enable agents as well as humans to do daily operations and maintenance. **Where:**
under `docs/operations` **Contains:** How deployments are done, infrastructure, IAM/RBAC, runbooks,
maintenance info **Lifecycle:** permanent, revised during ongoing operations; living document.

### Instruction files

**What:** `AGENTS.md`, `CLAUDE.md`, equivalents — things that need to be included **before**
inference, regardless of the task at hand. **Why:** these artifact describes input that is
frequently added to the context; Trigger is observed frequency **Where:** One AGENTS.md in the
project root, optional in subdirectories, e.g. if pertaining to `data-model` or `frontend` there may
be an entry in the subdirectory for that. **Contains:** rules and pointers — routes to spec,
architecture docs. Should never copy content, but reference it. The cost is per-invocation, so use
sparingly. A rule earns its place only until it becomes a mechanism: lint rule, hook, test, type.
**Lifecycle:** permanent, continuously revised. Try to move instruction into mechanisms instead,
then delete the instructions in here.

## The graph

Beads are the exception to the transient/permanent divide, because they are both used as a permanent
documentation on what went on when implementing, but they cease to factor into the ongoing work as
soon as they are closed. Task state, sequencing, dependencies, what's ready — all live in the graph
and nowhere else. Nothing deletes them (closing isn't deleting) and the graph keeps its own history.

The split is the reason for adopting beads.

## Lifecycles at a glance

| Artifact                          | Lifecycle                                              | Deleted by                     |
| --------------------------------- | ------------------------------------------------------ | ------------------------------ |
| Wish                              | Until shaped or dismissed                              | Promotion to a request         |
| Feature request                   | Until PRD exists                                       | Closeout                       |
| Bug report                        | Until the fix lands; repro survives as a test          | The fix                        |
| Backport bead                     | Until the fix lands on every line it owes              | Closing on main                |
| PRD (the plan)                    | Until implemented                                      | Closeout                       |
| Gate findings                     | Until fixed or promoted to a bead                      | The fix                        |
| Beads / task state                | Until closed; graph keeps history                      | Nothing — it is the graph      |
| Vision (`VISION.md`, repo root)   | Permanent, rarely revised                              | Never                          |
| Specification / user journeys     | Permanent, continuously revised — one file per journey | Never                          |
| MVP and MLP definitions           | Permanent, operational — a tier field on each journey  | Never                          |
| SDD                               | Permanent, continuously rewritten to present state     | Never                          |
| ADRs                              | Permanent, superseded not deleted                      | Never                          |
| Journey / e2e tests               | Permanent — the specification, executable              | Deletion of the journey        |
| Unit / component tests            | Permanent — the design, executable                     | Deletion of the module         |
| Regression tests                  | Permanent, append-only — a bug that must not recur     | Effectively never              |
| Changelog                         | Permanent, append-only                                 | Never                          |
| Roadmap                           | Permanent, continuously revised                        | Never                          |
| Operations manual                 | Permanent, revised                                     | Never                          |
| Instruction files (`AGENTS.md`)   | Permanent, revised, size-capped                        | A rule replaced by a mechanism |
| Observability notes for a feature | Until the feature is boring                            | Judgment                       |

Underlying rule: **a document is deleted only once every durable fact it holds lives somewhere with
a longer Lifecycle.** Deletion is a move, not a loss.

## Why the deletions matter

Instinct is to keep everything — storage's free, an old PRD feels like history. It isn't free: every
retained transient artifact is a future context-window entry an agent may treat as current, cost
landing the day a stale requirement contradicts a live one.

Inverse failure, already documented in this repo: throwaway plans deleted in the commit that
finished the work, taking a month of reasoning with them
([`_incoming/2026-08-07 1130 adoption-journey.md`](../_incoming/2026-08-07%201130%20adoption-journey.md)).
Same root both ways — task state and reasoning sharing a file. The catalogue above works only if
they never do.

## Open questions

- One file per journey assumes everything promised is a journey. Performance, security, other
  cross-cutting properties aren't — they belong to the spec as promises, not design. Where do they
  live, what tiers them?
- If unit tests are the executable design, a redesign rewrites them — during that rewrite only the
  journey suite asserts behavior didn't change. Enough, or does redesign need something design tests
  can't provide by construction?
- Rollup shrinks the ADR working set and nothing forces it. Absence is silent, symptom (agents
  reasoning from a partially read corpus) indistinguishable from ordinary error. What makes a
  missing rollup visible?
