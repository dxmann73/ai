# ai

A personal knowledge base about working with AI agents: how to communicate with them, how to build
with them, and what happens when organizations and societies adopt them.

Plain Markdown. No site generator, no build step. Read it on GitHub or in an editor.

## How this is organized

| Folder                        | What goes here                                                        |
| ----------------------------- | --------------------------------------------------------------------- |
| [`#incoming/`](%23incoming/)  | Raw, unsorted capture. Links, quotes, half-thoughts. No structure required. |
| [`log/`](log/)                | Processed incoming pieces, archived with the date they were integrated. |
| [`communication/`](communication/) | Prompting, context engineering, specs, feedback loops, failure modes. |
| [`aglc/`](aglc/)              | The Agentic Development Life Cycle, and how it differs from the SDLC.  |
| [`business/`](business/)      | Adopting agentic methods in a company: org design, economics, risk.    |
| [`society/`](society/)        | Schools, labor, policy, ethics, public understanding.                  |
| [`tools/`](tools/)            | Models, harnesses, MCP, evals, and the concrete tooling landscape.     |
| [`reference/`](reference/)    | Glossary, reading list, source notes.                                  |

## The intake flow

Ideas arrive faster than they can be organized, so there are two staging areas:

1. **Capture** — drop anything into `#incoming/` as a Markdown file. Speed over structure.
2. **Integrate** — later, fold the substance into the topic note where it belongs.
3. **Log** — move the original piece into `log/` as `YYYY-MM-DD-<slug>.md`, with a line at the top
   pointing to where its content ended up.

`#incoming/` should trend toward empty. `log/` is the append-only record of what was processed,
so nothing is silently dropped and the provenance of an idea stays traceable.

## Conventions

- One topic per file, named as a lowercase slug: `context-engineering.md`.
- Every note starts with an `# H1` title and a one-line summary underneath.
- Optional YAML frontmatter for `status` (`seed`, `draft`, `solid`) and `tags`.
- Each topic folder has a `README.md` acting as its index.
- Prose wraps at 100 characters (see `.markdownlint.json`).

## License

Prose is [CC BY 4.0](LICENSE). Attribution appreciated, reuse encouraged.
