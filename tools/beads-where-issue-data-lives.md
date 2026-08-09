# Beads: where issue data lives when work spans repositories

Notes from evaluating [beads](https://github.com/gastownhall/beads) as a task tracker, 2026-08-08,
against `bd` v1.1.2. Fast-moving tool; specifics below will age.

An issue tracker that lives inside the repository solves the problem agents actually have — the task
graph is readable at the point of work, versioned with the code it describes, and does not require a
network round-trip to a SaaS board. It creates a new problem in exchange: when a piece of work spans
two repositories, the data is in one of them and the work is in the other.

The instinct at that point is to reach for a central database. This note is about why that instinct
is wrong, and what to do instead.

## What beads actually stores, and where

Storage is **per repository**. `bd init` creates `.beads/` with an embedded Dolt database at
`.beads/embeddeddolt/`. Dolt is a version-controlled SQL database, so the issue history has its own
version control underneath the repository's. Cross-machine sync rides the git remote over
`refs/dolt/data`.

There is no "one database for every project" mode. The nearest-looking option,
`bd init --shared-server`, shares a Dolt *server process* at `~/.beads/shared-server/` while each
project keeps an isolated database named after its issue prefix.

The CLI is installed once per machine. Upstream is explicit that you do not clone the beads
repository into your project.

## Routing: which repository receives a new bead

Routing is opt-in and answers one question — where does `bd create` write? Precedence:

1. `--repo <path>` — explicit override, always wins.
2. `routing.mode: auto` — route by detected role, maintainer or contributor.
3. `routing.default` — everything else, defaulting to `.`.

The motivating case is the OSS contributor: you fork a project that uses beads, and every planning
bead you create pollutes your fork's issue database and shows up in your pull requests. Auto routing
redirects `bd create` to a separate planning repository, `~/.beads-planning` by default, which is
never pushed upstream.

Role comes from git config — `bd config set beads.role contributor|maintainer`. Leave it unset and
`bd` falls back to a deprecated heuristic based on remote URLs and prints a warning on every create.
Set it explicitly and the heuristic never runs.

With no routing configuration at all, every bead lands in the current repository. Single-repo
workflows are unaffected by any of this.

## Hydration: the unified view

Routing writes beads somewhere else, which means the local database does not contain them.
**Hydration** is the other half: it imports beads from other repositories into the local database,
each tagged with a `source_repo` field, so `bd list` and `bd ready` show one merged view.

```bash
bd repo add ~/projects/other      # append to repos.additional in .beads/config.yaml
bd repo list                      # primary plus additional
bd repo sync                      # import from all additional repos
bd repo remove ~/projects/other   # remove, deleting its hydrated beads
```

`bd repo sync` reads each additional repository's `.beads/issues.jsonl` export and imports the beads
with their original prefixes and `source_repo` set, skipping repositories whose export has not
changed. `bd doctor` warns when a routing target is missing from `repos.additional`.

The important consequence: once hydrated, foreign beads are ordinary rows in the local database.
They can be filtered by provenance and linked with normal dependencies.

```bash
bd list --json | jq '.[] | select(.source_repo == "~/projects/other")'
```

## Four ways to reference work that lives in another repository

**Hydrated hard dependency.** After hydration, a normal blocking edge works across repositories:

```bash
bd dep add ai-42 infra-10 --type blocks
```

**Non-blocking association.** `bd dep relate` creates a bidirectional "see also" link. This is the
right type for "this note documents that work" — related, but nothing is waiting on anything.

```bash
bd dep relate ai-42 infra-10
bd dep unrelate ai-42 infra-10
```

**Capability reference.** `bd dep add` accepts `external:<project>:<capability>` targets. These are
stored as-is and resolved at query time against the `external_projects` config, blocking the issue
until that capability ships in the target project.

```bash
bd dep add ai-42 external:infra:beads-adopted
```

This is the honest encoding of "my write-up depends on the rollout actually having happened"
without hard-wiring a bead ID that may be split, closed, or superseded.

**Prose.** Still valid, and preferable when the reference is for a human reader rather than for the
scheduler. `bd ready` does not need to know about every relationship that exists.

## Why per-repository is right even when you want a central view

The pull toward a central database is really a pull toward a **unified view**, and hydration
supplies the view without surrendering the storage model. The storage model is worth keeping:

- **Task data travels with the thing it describes.** Clone the repository, get its issue history.
  Archive the repository, archive its backlog with it. A central database re-introduces exactly the
  decoupling that in-repo tracking exists to remove.
- **Lifetimes differ.** Work items about a one-pass migration die when the migration completes.
  Notes about a long-lived knowledge base outlive it by years. Merging them into one store means the
  shorter-lived set never gets cleared, because clearing it is now someone's chore rather than a
  side effect of the repository going quiet.
- **Visibility classes differ, and that is not negotiable.** See below.

Upstream's own position is that multi-repo is not needed for solo work on your own projects. That
is true right up until two repositories acquire a real dependency on each other, which is the case
this note is about.

## Visibility sets the direction of hydration

`.beads/` and `.beads/config.yaml` are committed. Hydration copies foreign beads into the local
database, and `config.yaml` records the source paths. Both get pushed.

So hydration between repositories of different visibility is **one-directional**: a private
repository may hydrate from a public one, never the reverse. Hydrating a private repository into a
public one publishes the private task graph and leaks local filesystem paths along with it.

This also disposes of the central-database idea on independent grounds. One database across
repositories means one visibility class across those repositories. If any repository in the set must
stay private, the whole set does.

Where the direction is wrong for the view you want, use `external:` references and prose instead.
They carry the relationship without copying the data.

## Honest costs

- **Hydration is a snapshot, not a live join.** `bd repo sync` reads the other repository's
  `issues.jsonl` export. The other side must have exported, and the local side must re-sync. Stale
  views are the failure mode, and nothing warns you that a view is stale.
- **The configuration is asymmetric and committed.** Whichever repository wants the merged view
  carries the config. Wanting it in both directions means maintaining it in both, and hydrated
  copies then exist in both databases.
- **`issues.jsonl` is an export, not a backup.** Upstream is explicit that it is not the source of
  truth. Treating it as one will eventually cost data.
- **The tool is young.** Roughly ten months old at the time of writing, with a high open-issue count
  relative to its age, and a real dependency on Dolt underneath. Schema migrations across binary
  upgrades are the most likely source of operational pain, particularly once more than one machine
  holds a clone of the same database.

## The tradeoff being made

In-repo tracking trades browsable, greppable, reviewable-in-a-diff markdown for a database that
needs a CLI to read. That is a genuine loss and it is only partly recoverable — the export helps
viewers and interchange, while the database stays the artifact of record.

The offset is only partial, and it works by moving *reasoning* somewhere durable and human-readable
— design documents and ADRs — so that only the *task state* lives in the database. The split holds
only while it is maintained. The predictable failure is rationale drifting back into bead notes
until the design documents are stale and nobody trusts them.

## Open questions

- Does a merged `bd ready` across repositories actually change behaviour, or does the working
  context already imply which repository's queue matters?
- At what number of repositories does `repos.additional` stop scaling as a mental model?
- What is the recovery story when a Dolt schema migration lands mid-flight across two machines?
