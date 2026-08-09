# Adopting Beads

Status as of 2026-08-08: decided and planned, not yet installed. This note records the reasoning and
the sequence; it gets updated as steps land rather than rewritten afterwards.

The mechanics of the tool — storage model, routing, hydration, cross-repository references — are in
[`tools/beads-where-issue-data-lives.md`](../../tools/beads-where-issue-data-lives.md). This note is
the adoption side: why now, what it replaces, and what the rollout actually looks like.

## Where this came from

The path to the tool is spread over ten weeks and is dated from browser history: a search for
"beads framework steve yegge" on 2026-05-24, "Beads Best Practices" on 2026-06-21, "Gas City" and
"Welcome to Gas Town" in mid-July, the Beads repository on 2026-07-19, all of `yegge.ai` on
2026-08-04 and 2026-08-05, and both parts of **"The Shape of Things to Come"** on 2026-08-08.

Part 1 — *The Continuous Thunderdome* — is the piece that turned reading into a decision. Its
relevant claim is small and specific inside a much larger argument: the working setup is **loops and
graphs**, where the graph is Beads and the loop runs against it, sitting next to a small Markdown
"project brain" that holds the reasoning the graph does not.

Most of the rest of that piece does not transfer, and it is worth being explicit about why. Yegge
reports roughly $87k/month of token burn, about 69 billion tokens in July at a 96% cache-hit rate,
and 20–25% of all project work spent on the harness itself. He also reports that his reusable
harness burned down and that harnesses will all end up bespoke. That is a frontier position, and his
own framing — "I am not special, I'm just ahead of you" — is the part to be most careful with. The
honest question for anyone on a normal budget is which components survive the loss of that budget.

Beads survives it. It is a local CLI over a versioned database in the repository, with no per-token
cost of its own. It is the cheapest piece of that setup and the one with the clearest standalone
benefit, which is why it is the piece being adopted first and alone.

## What it replaces, and why that failure is well documented

The problem is not the absence of a task tracker. It is what happened to plan files.

From **2026-02-25**, a rule was written into `nomap`'s `AGENTS.md`: *"NEVER use markdownlint on
plans. Plans will deleted later. We do not need to lint them."* Plans were explicitly scaffolding.
Across the following sixteen weeks, 44 plan files were created and nearly all were deleted in the
commit that finished the work. On **2026-06-15** the rule was reversed — "chore: track plans in git
and drop throwaway-plan rule" — and plans became tracked artifacts.

The reversal fixed the deletion but not the underlying problem, which surfaced next as plans that
were kept and not indexed. Yegge describes ending up with more plans than he could make sense of.
This is the stage before that: plans treated as disposable, with the cost only becoming visible when
use-case-level work needed the previous month's reasoning back.

So the adoption is not "add a task tracker". It is a **split**, and the split is the whole point:

| Content | Destination |
| --- | --- |
| Task state, sequencing, what is ready to work on | Beads |
| Why a choice was made, what was rejected, when to revisit | `docs/SDD/` and `docs/ADR/` |
| Measured state of a live system | Its own inventory file |

Task state was the part that was safe to delete all along. Reasoning was not, and it was the part
that kept getting deleted with it because both lived in the same file.

## The rollout

Ordering is deliberate. The tool cannot track its own installation, so the first steps are done by
hand and only then does the work move into the graph.

1. **Install `bd` box-level.** It is a machine tool, alongside `gh` and `jq`, installed once per
   machine via the upstream script into `~/.local/bin`. Not via `npm -g`, which would scope it to
   one nvm-managed Node version and vanish on a version switch.
2. **Document the procedure once**, in the box-setup repository: how to attach Beads to a project,
   what `bd init` writes, which per-agent integrations exist, and the upgrade path past the
   schema-version guard. The procedure is reusable and belongs at box level. The rollout work
   itself does not — each repository carries its own adoption tasks.
3. **Add the agent rule** globally: in a Beads-enabled repository, prime at session start, work from
   the ready queue, claim atomically, and never use the interactive editor command, which an agent
   cannot drive.
4. **Pilot in one repository.** A documentation repository with a single one-pass migration, no
   deployed code, and therefore nothing that breaks if the tool disappoints. Its existing plan files
   split along the table above: checklists into Beads, rationale into `docs/SDD/` and `docs/ADR/`.
5. **Then the second repository**, once the first has proven the split holds in practice.

Cross-repository work is handled by per-repository databases plus hydration, not by a central
store — the reasoning is in the tools note. One constraint from that is worth repeating here because
it is easy to get backwards: `.beads/` and its config are committed, so hydration may run **from a
public repository into a private one, never the reverse**.

## What would count as this having worked

- Reasoning written six months ago is findable without `git log --diff-filter=D`.
- The ready queue is trusted enough to be the actual answer to "what now", rather than a second
  place to look after the markdown.
- No plan file is deleted to clean up. Nothing needs deleting, because task state and reasoning no
  longer share a file.

## What would count as it having failed

- Rationale drifting back into bead notes until the design documents go stale and nobody trusts
  them. This is the predictable failure and it is not hypothetical — it is the same failure as the
  throwaway-plan rule, wearing a different hat.
- A schema migration across binary versions costing more than the tool saves. The tool is young and
  carries a real database underneath.
- The graph becoming write-only: beads created diligently, never read, and the ready queue quietly
  replaced by memory.

## Open questions

- Does an in-repo task graph actually change behaviour for a solo practitioner, or is its value
  mostly that agents can read it?
- Yegge's setup treats the graph as the coordination substrate for many parallel agents. At one or
  two agents, is the graph earning its keep, or is it the markdown project brain doing the work?
- The reasoning split is a discipline, not a mechanism. Is there a mechanism that would enforce it?
