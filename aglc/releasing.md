---
status: draft
tags: [aglc, releasing, process]
---

# Releasing

Two kinds of line — main and the release branches carrying candidates — and a tag that says a
commit is going live.

The companion to [`feature-lifecycle.md`](feature-lifecycle.md), which places the cutover in the
process, and to [`artifacts.md`](artifacts.md), which catalogues everything else the lifecycle
writes.

## Two kinds of line

Stability is not maintained continuously on main — it is produced deliberately, on a line cut for
the purpose.

- **`main`** is open and unstable. Features land here continuously.
- **`release/*`** are cut from main at a [roadmap](artifacts.md#roadmap) cutover and closed to
  features by construction.

## Going live is a tag

Releasing is **tagging a commit on a stable release branch**, and the tag is what makes it go live.
The tags are immutable, so every release ever made stays nameable.
How a tag reaches the running system is not currently part of the AGLC.

## The release branch

A release branch has two working states.

1. **Stabilizing.** Its own loop: full integration environment, full e2e, every user journey, fix,
   repeat. Each green head is a release candidate, tagged as one. Nothing new lands here.
2. **Supported.** A candidate has been tagged and the line is in the field. The branch stays open
   for patches — each patch is stabilized the same way and produces a new candidate, which is how a
   hotfix reaches live without anyone hand-editing what is running.
3. **Retired.** When no version anyone can still be running comes from this line, the branch can be
   deleted. The tags stay.

More than one branch exists because the two states overlap: the shipped line is taking patches
while the next line stabilizes.

## Promotion and rollback

When a candidate is green and the decision to ship is made, it gets a release tag. That is the
promotion.

Rolling back is going back to the previous release tag.

## Where fixes go

A defect found in the field or during stabilization is fixed on the release branch it belongs to.

Every such fix travels to main as a [backport bead](artifacts.md#backport-bead) carrying the error
and the remedy.

## The process lives in `RELEASING.md`

This note is the model — the lines, what they are for, and what a tag means. The *procedure* is
per-repository and belongs in the repository, as `RELEASING.md` in the root.

It is a permanent artifact, present tense, rewritten to match how releasing works today. What
belongs in it is everything the model deliberately does not know:

- the artifacts this product ships
- the branch naming, the release tag scheme, and the candidate tag scheme this repository uses
- what a release tag actually triggers, and how a rollback is performed
- the procedure and commands for cut, stabilize, tag, and roll back
- how supported versions are communicated
- who is allowed to release — that is, who is allowed to push a release tag

## Lifespans

| Artifact | Lifespan | Ended by |
| --- | --- | --- |
| `main` | Permanent | Never |
| Live release tag | Permanent | Never |
| Release branch, stabilizing | Until a candidate is released | The release tag |
| Release branch, supported | Until nothing from this line is in the field | Deletion |
| Release candidate tag | Until its line retires | Deletion with the branch |
| Backport bead | Until the fix lands on every line it owes | Closing on main |
