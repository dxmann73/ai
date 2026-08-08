# #log

Processed pieces from [`#incoming/`](../%23incoming/), archived after their substance was integrated
into the main body of work.

Append-only. This is the record of what was considered, when, and where it went — so an idea can be
traced back to the moment and the source it arrived from, even after it has been rewritten beyond
recognition in a topic note.

It has a second job: these entries are the raw material for blog posts on the website. Each one
carries a marker saying whether a post has drawn on it yet.

## Format

Files are named `YYYY-MM-DD-HHmm-<slug>.md`, stamped with the moment they were integrated (not
captured) — 24-hour clock, no colon, so several pieces a day stay distinct and sort in order. Each
starts with two pointer lines:

```markdown
> Integrated into [aglc/verification-loops.md](../aglc/verification-loops.md) on 2026-08-08 14:32.
> Post: none yet.
```

Below that, the original piece is preserved unedited — forever, including after a post uses it.

Rejected items live here too, with `> Reviewed and not used on YYYY-MM-DD:` plus a one-line reason.
Knowing what was discarded, and why, is worth as much as knowing what was kept.

## Posting

Posts are composed from the pool of entries still marked `Post: none yet`, usually two or three
related ones at a time — never one post per entry. The unpublished pool is:

```bash
grep -l 'Post: none yet' '#log/'*.md
```

When a post draws on an entry, its second line becomes a pointer to the draft in the website repo:

```markdown
> Post: [orchestration-tax](../../website/src/content/posts/en/orchestration-tax.md), drafted 2026-08-12.
```

The website holds the post; this folder holds the provenance. Entries that never feed a post keep
`Post: none yet` indefinitely — that is a normal end state, not a backlog.

See [`AGENTS.md`](../AGENTS.md) for the full workflow.
