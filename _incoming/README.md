# _incoming

Raw capture. Drop things here without thinking about where they belong.

Anything goes: a link with one line of context, a quote, an argument overheard in a meeting, a
half-formed objection. Speed over structure — the whole point of this folder is that it costs
nothing to add to.

## Rules

- One file per idea. Name it whatever; a rough slug is fine.
- No formatting requirements. A single sentence is a valid note.
- This folder should trend toward empty.

## Processing

When an item gets integrated into a topic note, delete it with `git rm`. Its substance lives in the
note now, and git holds the original. A piece that turns out to be worthless is deleted the same
way, with the reason in the commit message.

If it is also worth writing about publicly, stage an entry in [`_blog/`](../_blog/) before deleting.
See [`AGENTS.md`](../AGENTS.md) for the full workflow.
