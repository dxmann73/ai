# AGENTS.md

## What this repo is

A personal knowledge base of Markdown notes about AI agents: communication, the agentic
development life cycle, business adoption, and societal impact. It is **prose, not code**. There is
no build, no framework, no package manager, and none should be added without being asked.

## Layout

```text
_incoming/   raw unsorted capture, trends toward empty
_blog/       staging for posts, trends toward empty
communication/ aglc/ tools/ reference/
adoption/    business/ society/ journey/ — three aspects of adoption
```

Each topic folder has a `README.md` index listing its notes.

## Intake workflow

When asked to process `_incoming/`:

1. Read the piece and decide which topic note it belongs to.
2. Integrate the substance into that note — merge with existing text, do not just append a blob.
3. Update the topic folder's `README.md` index if a new file was created.
4. Delete the incoming piece with `git rm`. Its substance now lives in the topic note, and git holds
   the original. A piece that turns out to be worthless is deleted the same way — say so in the
   commit message rather than keeping the file.
5. Then run the posting check below. Always ask; never publish unprompted.

Nothing is kept for the sake of provenance. A second copy of an absorbed piece is a document that
can go stale against the note that superseded it, and an agent that finds it will believe it. The
same reasoning applies to anything already written down here that turns out to be wrong: delete it,
do not annotate it. `git log --diff-filter=D` is where it went.

## Posting workflow

The website at `../website` is the publishing target. `_blog/` is staging: drafts and raw material,
never an archive.

Blog posts are **composed from the pool of unpublished `_blog/` entries** — not necessarily one post
per idea, which would produce a stream of thin posts nobody wants to read.

1. List the unpublished pool: `grep -l 'Post: none yet' _blog/*.md`.
2. Judge whether a theme has accumulated enough substance for one coherent post. Two or three
   related entries is usually the threshold; one entry rarely is. Say so plainly when the answer is
   no — "nothing publishable yet" is the expected outcome most of the time.
3. Report the recommendation to the author and let them decide. Do not start writing unasked.

When they say go:

1. Write the post directly into `../website/src/content/posts/en/<slug>.md`, following that repo's
   `AGENTS.md` — its frontmatter schema, its EN-always rule, `draft: true` until it is finished.
   Do not create a DE copy; the website's fallback handles a missing translation.
2. The post is prose in the author's voice drawing on the staged entries. It is not a concatenation
   of them, and it is not a copy of the topic note.
3. When the post is finished and no longer a draft, delete every `_blog/` entry it drew on.

The website holds the canonical published post. Never maintain a second copy of a post here — that
only creates two versions to keep in sync, which is the same failure as keeping an absorbed incoming
piece.

An entry that never becomes a post is deleted when it stops being worth writing about. `_blog/`
trends toward empty; a growing pool means the decision is being deferred, not that material is
accumulating.

## Writing conventions

- One topic per file, lowercase slug filenames: `context-engineering.md`.
- `# H1` title, then a one-line summary sentence.
- Every topic folder keeps a `README.md` index; add new notes to it.
- Optional frontmatter: `status: seed|draft|solid`, `tags: [...]`.
- Wrap prose at 100 characters. Follow the `markdownlint` skill; config is `.markdownlint.json`.
- Distinguish the author's own position from summarized sources. Attribute claims that came from
  somewhere else, with a link.
- Do not invent citations, statistics, or quotes. If a claim needs a source and none is at hand,
  mark it `[source needed]` rather than fabricating one.

## Editing rules

- Preserve the author's voice. This is a personal notebook, not documentation.
- Prefer editing an existing note over creating a near-duplicate one.
- Keep edits focused; do not reorganize folders unless asked.
