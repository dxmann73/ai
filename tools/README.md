# Tools

The landscape of tools moves very fast. This page serves as a quick overview.
Tools that have been adopted or tested get separate pages.

## Coding Agents

The harness driving your agent with logic and system prompt.

- Claude Code, Codex: vendor specific frontier models
- [Cursor CLI](https://cursor.com/cli), specifically Composer 2
- Grok build (not tried yet)
- [Pi](https://pi.dev/)
- [OpenCode](https://opencode.ai/)

## Terminal multiplexers

If you don't want to manage several terminal windows, you can use a multiplexer.

- Tmux (keeps terminals alive), zellij
- Cmux

## Agent runtimes / agent harness control surface

Apps that run multiple agents, display them in a nice UI and let you group them into projects/categories.
Some of them solve the problem to send clipboard content to agents running on another machines.

- Codex Desktop, Claude Desktop, Cursor Glass, Conductor, emdash, superset
- T3 Code
- [herdr](./herdr.md): mouse-first, agent-aware multiplexer; one workspace per project

## Notes

- [Beads: where issue data lives when work spans repositories](./beads-where-issue-data-lives.md)
  (2026-08-08) — storage model, routing, hydration, and the four ways to reference work in another
  repository. The adoption side is in
  [`adoption/journey/beads-adoption.md`](../adoption/journey/beads-adoption.md).
