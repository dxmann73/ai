# Tools

The landscape of tools moves very fast. This page serves as a quick overview.
Tools that have been adopted or tested get separate pages.

## Coding Agents

The harness driving your agent with logic and system prompt.
[Differences explained in a nutshell](https://youtu.be/dLhcLqoff6k?t=620)

- Claude Code: vendor specific models locked to subscription (or pay API prices)
  Richer features that OpenCode et al, there seems to be a focus on the CLI
- Codex: vendor specific frontier models + hackable custom model endpoint
  Richer features than OpenCode, features added seem to be focused on the App (with ChatGPT folded in)
- [Cursor CLI](https://cursor.com/cli), specifically Composer 2
- Grok build (not tried yet)
- [Pi](https://pi.dev/)
- [OpenCode](https://opencode.ai/): No subsidization via subscription because different harness

## Terminal multiplexers

If you don't want to manage several terminal windows, you can use a multiplexer.

- Tmux (keeps terminals alive), zellij
- Cmux

## Agent runtimes / agent harness control surface

How to run multiple agents, display them in a nice UI, group them into projects/categories.
Use them as if on your local box, i.e. send clipboard content, no network issues (*cough* ssh).

- [T3 Code](./t3code.md): open-source control plane for coding agents; client/server, several
  machines in one window, headless servers. [Site](https://t3.codes/) ·
  [Github](https://github.com/pingdotgg/t3code)
- [herdr](./herdr.md): mouse-first, agent-aware multiplexer; one workspace per project
- Proprietary/closed source: Codex Desktop, Claude Desktop, Cursor Glass, Conductor, emdash, ...

## Notes

- [Beads: where issue data lives when work spans repositories](./beads-where-issue-data-lives.md)
  (2026-08-08) — storage model, routing, hydration, and the four ways to reference work in another
  repository. The adoption side is in
  [`adoption/journey/beads-adoption.md`](../adoption/journey/beads-adoption.md).
