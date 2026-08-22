# T3 Code – agent control plane

An open-source client/server GUI that runs several coding agents on one or more machines and gives
them a single window, including from a phone.

[Site](https://t3.codes/) · [GitHub](https://github.com/pingdotgg/t3code) · [docs](https://github.com/pingdotgg/t3code/tree/main/docs/user)

## What it is

T3 Code does not contain a model or an agent harness. It drives provider CLIs that are already
installed and authenticated on the machine: Codex, Claude Code, Cursor CLI, Grok Build and OpenCode
are the supported ones as of v0.0.33. What it adds is everything around the agent — a place for the
threads to live, terminals next to them, git state, and a UI that survives the client going away.

The split that matters is client/server:

- the **server** owns projects, files, git state, terminals and provider sessions. It runs where the
  work happens.
- the **desktop app**, the **mobile app** and the **hosted web app** at `app.t3.codes` are clients.
  They attach and render.

That is the same structural move as a terminal multiplexer, one level up: instead of persisting
terminal panes, it persists agent threads and the processes behind them.

## Environments

A client holds several servers at once, each saved as an *environment*.
Projects are added per environment, and the usage view aggregates provider spend
across all of them.

This is the feature that makes it a control plane rather than an app. One window can cover a laptop,
a VM, and a build box, with each thread bound to the environment its code lives on.

Three ways to reach a remote environment, per the
[remote access docs](https://github.com/pingdotgg/t3code/blob/main/docs/user/remote-access.md):

- expose the desktop app's own backend on the network
- run a headless server with `t3 serve` and pair a client to it
- have the desktop app launch a server on another machine over SSH and forward the port back

Pairing is token-based and one-time: a device exchanges the token for a session and never needs it
again. The recommended transport is a tailnet — a stable address and transport security without
exposing anything publicly. Tailscale Serve is supported directly (`t3 serve --tailscale-serve`).

## Headless is the interesting mode

`t3 serve` plus the Linux background service (`t3 service install`, a systemd user unit with
lingering) turns a machine into an agent host with no GUI on it at all: it boots, the server comes
up, agents and dev servers run, and any client attaches later.

That is exactly the shape an isolated agent VM wants — see
[`agent-box-setup/vm/03-t3code.md`](https://github.com/dxmann73/agent-box-setup) for the concrete
setup on this box: headless server in the VM, desktop app on the host holding both the VM
environment and a host-local one.

## Permission modes

Four modes, set per thread: **Supervised**, **Auto-accept edits**, **Auto**, and **Full access**,
which is the default. Each provider translates the mode onto its own approval and sandbox settings,
so the guarantee is only as strong as the weakest provider — OpenCode falls back to asking where
Codex delegates to an AI reviewer.

The honest reading: **Full access** is only sane where the blast radius is already bounded, in a
throwaway worktree or a VM. Where the mode is the only boundary, it is not one.

## Compared to herdr

[herdr](./herdr.md) and T3 Code solve the overlapping problem from opposite ends.

| | herdr | T3 Code |
| --- | --- | --- |
| Primitive | terminal pane | agent thread |
| Agent awareness | detects agents running inside panes | starts and owns the agent process |
| Interface | TUI, mouse-first, in a terminal | Electron app, web app, mobile app |
| Remote | attach over SSH to the server | native environments, pairing, tailnet, SSH launch |
| Scripting | CLI and local socket API, so agents can drive it | not the pitch |

herdr is a multiplexer that learned about agents; T3 Code is an agent UI that also gives you
terminals. herdr's socket API makes it something an agent can manipulate — a shared runtime for
humans and agents. T3 Code's environments make it something that spans machines — one pane of glass
over several boxes. Pick by which of those two you actually need.
