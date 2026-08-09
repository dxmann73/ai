# _blog

Staging for posts. Entries here are drafts and raw material waiting to become something on the
website.

This folder trends toward empty, the same way [`_incoming/`](../_incoming/) does. An entry leaves in
one of two ways: it becomes a published post and is deleted, or it turns out to be wrong and is
deleted. Nothing accumulates here.

## Why nothing is kept

The website holds the canonical post. Git holds the history. An entry that has already fed a post is
a second copy of something published, and a second copy is a thing that drifts.

The same applies to entries that turn out to be faulty. A note that says something now known to be
wrong is worse than no note: it is retrievable, plausible, and misleading, and an agent that finds it
will believe it. Delete it. `git log --diff-filter=D` is where it went, and that is enough.

This is the [artifact lifespan rule](../aglc/artifacts.md) applied to this repo's own writing: a
document is deleted only once every durable fact it holds lives somewhere with a longer lifespan.
For a staging entry, that somewhere is the topic note it was integrated into and the post it fed.

## Format

Files are named `YYYY-MM-DD-HHmm-<slug>.md`, stamped with the moment they were written — 24-hour
clock, no colon, so several a day stay distinct and sort in order. Each opens with its title, then a
pointer block saying where its substance already lives:

```markdown
# Outline — the artifact is the unit

> Drawn from [aglc/artifacts.md](../aglc/artifacts.md).
> Post: none yet.
```

## Posting

Posts are composed from the entries still marked `Post: none yet`, usually two or three related ones
at a time. One post per entry produces a stream of thin posts nobody wants to read.

```bash
grep -l 'Post: none yet' _blog/*.md
```

When the post is drafted into `../website/src/content/posts/en/<slug>.md` and is no longer a draft,
every entry it drew on is deleted. Until then they stay, because an unpublished draft has nowhere
else to live.

See [`AGENTS.md`](../AGENTS.md) for the full workflow.
