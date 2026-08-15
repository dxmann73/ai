# herdr - agent runtime + clients

[Docs](https://herdr.dev/docs/)
[Comparison](https://herdr.dev/compare/) to tools like tmux etc.

A mouse-first terminal multiplexer that knows which agent in which project is working, blocked, or
done. See the [Herdr agent guide](https://herdr.dev/agent-guide.md).

## What it is

Structurally a multiplexer like tmux: a background server owns the real terminal processes, clients
only attach and render. Panes keep running when you detach, close the terminal, or lose the SSH
connection.

Two things separate it from tmux.

- mouse-first — panes, tabs, workspaces, split borders etc. are  clickable
- agent-aware: detects coding agents running inside panes and shows each one's state in a sidebar

A CLI and a local socket API let scripts and agents drive Herdr themselves, which makes it a control
surface for agents, not only a window manager for humans.

## Key concepts

- **Session** — a persistent server namespace. Bare `herdr` attaches to the default one; named
  sessions are fully separate runtimes. Most setups need only the default.
- **Workspace** — the project-level container, one per repo or investigation. Owns tabs and panes,
  and the sidebar rolls agent state up per workspace.
- **Tab** — a layout inside a workspace, for separating views like `agents`, `logs`, `server`.
- **Pane** — a real terminal, splittable right or down, surviving client detach.
- **Agent** — a process Herdr recognizes inside a pane. States: `working`, `blocked`, `done`,
  `idle`, `unknown`. Optional per-agent integrations add lifecycle state, native session restore,
  or both, on top of screen detection.
- **Modes** — terminal mode sends keys to the focused pane, prefix mode (`ctrl+b`, then one key)
  sends a single command to Herdr, navigate mode is a persistent navigation surface.

## How users work with it

Assume three concurrent projects: a product, its website, and a third side project. Each gets one
workspace. Inside a workspace, tabs separate the views that belong to that project — an `agents` tab
holding one pane per running agent, a `logs` tab, a `server` tab for the dev server. Splitting a
pane gives a second agent on the same task without leaving the tab.

The payoff is the sidebar. Instead of cycling through terminal windows to find out who needs
attention, you see all three projects at once with their agents rolled up: the product agent is
`working`, the website agent is `blocked` waiting for an answer, the side project is `done`. That
turns supervising parallel agents from polling into responding — you go where the state says to go.

Everything survives detaching, so the workspace layout is a durable place the projects live in
rather than a session rebuilt each morning. Reattach with `herdr`; the whole thing stops only on
`herdr server stop`.

Because Herdr exposes a CLI and socket API, an agent that has the Herdr skill installed can do this
itself: split a pane, run a command without stealing focus, read the output back, wait on another
agent to finish. At that point the workspace is less a UI and more a shared runtime that humans and
agents both manipulate.
