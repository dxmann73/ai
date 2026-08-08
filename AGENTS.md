# AGENTS.md

## What this repo is

A personal knowledge base of Markdown notes about AI agents: communication, the agentic
development life cycle, business adoption, and societal impact. It is **prose, not code**. There is
no build, no framework, no package manager, and none should be added without being asked.

## Layout

```text
#incoming/   raw unsorted capture, trends toward empty
log/         processed incoming pieces, archived as YYYY-MM-DD-<slug>.md
communication/ aglc/ business/ society/ tools/ reference/
```

Each topic folder has a `README.md` index listing its notes.

## Intake workflow

When asked to process `#incoming/`:

1. Read the piece and decide which topic note it belongs to.
2. Integrate the substance into that note — merge with existing text, do not just append a blob.
3. Update the topic folder's `README.md` index if a new file was created.
4. Move the original into `log/YYYY-MM-DD-<slug>.md` using `git mv`, and add a line at the top:
   `> Integrated into [path](../path) on YYYY-MM-DD.`
5. Never delete an incoming piece. If it turns out to be worthless, log it with a note saying so.

## Writing conventions

- One topic per file, lowercase slug filenames.
- `# H1` title, then a one-line summary sentence.
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
