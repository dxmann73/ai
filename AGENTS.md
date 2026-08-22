# AGENTS.md

## What this repo is

A knowledge base of AI facts, concepts, adoption strategies, engineering, communication, the agentic
development life cycle, business adoption, and societal impact.

## Layout

```text
_incoming/   raw unsorted capture
_blog/       staging for blog posts
README.md    root document that leads to the topic folders
<topic folder> topic folder has a `README.md` index listing its notes to make this work on github.
```

## Intake workflow

When asked to process `_incoming/`:

1. Read the piece and decide which topic notes it belongs to.
1. Integrate the substance into that notes. Merge with existing text, do not just append a blob.
1. Update the topic folder's `README.md` index if a new file was created.
1. Gate: The human will rewrite and rephrase.
1. When everything has been integrated, run the posting check below.
1. Delete the incoming piece.

## Posting check

While working on this repo, we try to gather material for website posts. The website at `../website`
is the publishing target. `_blog/` contains drafts and raw material.

Therefore, every time an item is processed from `_incoming`, we should either:

- add to an existing draft when it fits there, with date/time and a gist of what we did
- create a new draft with a summary of the changes if there is no fitting draft

Draft format is in `_blog/README.md`.

Then, judge whether a theme has accumulated enough substance for one coherent post.

Ask the human about it. When they say go:

1. Draft a post in this repo, following that repo's rules.
1. Use prose written fresh in the author's voice, with the drafts as raw material.
1. The human will then iterate on the post.
1. When the post is finished and no longer a draft, delete every `_blog/` entry it drew on.

## Writing conventions

- currently mixed english/german, the intended project language is german, tranlsated later 
- One topic per file, lowercase slug filenames: `context-engineering.md`.
- `# H1` title, then a one-line summary sentence.
- Every topic folder keeps a `README.md` index; add new notes to it.
- Optional frontmatter: `status: seed|draft|solid`, `tags: [...]`.
- Wrap prose following the `markdownlint` skill; config is `.markdownlint.json`.
- Attribute claims that came from somewhere else, with a link.
  If a claim needs a source and none is at hand, mark it `[source needed]`.
- Do not invent citations, statistics, or quotes.

## Editing rules

- Preserve the author's voice.
- Prefer editing an existing note over creating a near-duplicate one.
- Keep edits focused; do not reorganize folders unless asked.
