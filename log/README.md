# log

Processed pieces from [`#incoming/`](../%23incoming/), archived after their substance was integrated
into the main body of work.

Append-only. This is the record of what was considered, when, and where it went — so an idea can be
traced back to the moment and the source it arrived from, even after it has been rewritten beyond
recognition in a topic note.

## Format

Files are named `YYYY-MM-DD-<slug>.md`, dated by the day they were integrated (not captured). Each
starts with a pointer line:

```markdown
> Integrated into [aglc/verification-loops.md](../aglc/verification-loops.md) on 2026-08-08.
```

Below that, the original piece is preserved unedited.

Rejected items live here too, with `> Reviewed and not used on YYYY-MM-DD:` plus a one-line reason.
Knowing what was discarded, and why, is worth as much as knowing what was kept.
