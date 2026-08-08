# AGENTS.md

## What this repo is

A personal knowledge base of Markdown notes about AI agents: communication, the agentic
development life cycle, business adoption, and societal impact. It is **prose, not code**. There is
no build, no framework, no package manager, and none should be added without being asked.

## Layout

```text
#incoming/   raw unsorted capture, trends toward empty
#log/        processed incoming pieces, archived as YYYY-MM-DD-<slug>.md
communication/ aglc/ business/ society/ tools/ reference/
```

Each topic folder has a `README.md` index listing its notes.

## Intake workflow

When asked to process `#incoming/`:

1. Read the piece and decide which topic note it belongs to.
2. Integrate the substance into that note — merge with existing text, do not just append a blob.
3. Update the topic folder's `README.md` index if a new file was created.
4. Move the original into `#log/YYYY-MM-DD-HHmm-<slug>.md` using `git mv`, and add two lines at the
   top:

   ```markdown
   > Integrated into [path](../path) on YYYY-MM-DD HH:MM.
   > Post: none yet.
   ```

   The stamp is the moment of integration, 24-hour clock, no colon in the filename — several pieces
   get processed per day and each needs its own sortable slot.

5. Never delete an incoming piece. If it turns out to be worthless, log it with a note saying so.
6. Then run the posting check below. Always ask; never publish unprompted.

## Posting workflow

The website at `../website` is the publishing target. Blog posts are **composed from the pool of
unpublished `#log/` entries** — not necessarily one post per integrated piece, which would produce
a stream of thin posts nobody wants to read.

After every move into `#log/`:

1. List the unpublished pool: `grep -l 'Post: none yet' '#log/'*.md`.
2. Judge whether a theme has accumulated enough substance for one coherent post. Two or three
   related entries is usually the threshold; one entry rarely is. Say so plainly when the answer is
   no — "nothing publishable yet" is the expected outcome most of the time.
3. Report the recommendation to the author and let them decide. Do not start writing unasked.

When they say go:

1. Write the post directly into `../website/src/content/posts/en/<slug>.md`, following that repo's
   `AGENTS.md` — its frontmatter schema, its EN-always rule, `draft: true` until it is finished.
   Do not create a DE copy; the website's fallback handles a missing translation.
2. The post is prose in the author's voice drawing on the log entries. It is not a concatenation of
   them, and it is not a copy of the topic note.
3. Update every `#log/` entry the post drew on, replacing the pointer line:
   `> Post: [<slug>](../../website/src/content/posts/en/<slug>.md), drafted YYYY-MM-DD.`
4. Leave the log entries otherwise untouched. Their body stays verbatim forever.

The website holds the canonical published post; this repo holds the notes and the provenance. Never
maintain a second copy of a post here — that only creates two versions to keep in sync.

Entries that never make it into a post simply keep `Post: none yet`. That is a normal end state, not
a backlog to burn down.

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
