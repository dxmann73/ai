# Buzz ACP harness — agents as channel members with their own keys

Investigate for `communication/`: [block/buzz](https://github.com/block/buzz) puts humans and agents
in the same rooms on a self-hosted Nostr relay, and its
[ACP harness](https://github.com/block/buzz/blob/main/TESTING.md#acp-harness-optional-end-to-end-with-a-real-agent)
is the concrete wiring — worth reading as a communication design, not just a test doc.

## What the harness actually is

`buzz-acp` connects an ACP-speaking agent (goose, codex, claude code, buzz-agent) to the relay. It
listens for events, drives the agent over stdio, and the agent replies through MCP tools. So the
protocol stack is: Nostr events on the wire, ACP to the agent process, MCP for the agent's actions.
Three protocols, three different jobs, and the doc treats that as unremarkable.

The setup sequence from `TESTING.md` is the interesting part, because each step is a communication
decision made explicit:

- **The agent gets its own keypair.** `buzz-admin generate-key` mints an identity distinct from the
  sender's. Not a bot token on a human account — a separate signer with its own audit trail.
- **The agent must be added to the channel.** `buzz channels add-member --pubkey $AGENT_PUBKEY
  --role member`. Skip it and the agent "discovered 0 channel(s) → agent will sit idle" and silently
  ignores every mention. Membership is the subscription.
- **Membership is live.** Add the agent after it started and it picks up the notification and
  subscribes without a restart.
- **Addressing is @mention in a channel.** You talk to it by posting; it answers as a kind:9 in the
  same channel. `buzz messages thread` walks the reply chain.
- **There is a respond-to gate.** `BUZZ_ACP_RESPOND_TO` defaults to owner-only; `anyone` opens it up
  for testing, and even then the agent skips events it signed itself. Loop prevention is a config
  flag, not an emergent behaviour.
- **Memory is injected by default.** "NIP-AE core-memory prompt injection is on by default; set
  `BUZZ_ACP_NO_MEMORY=true` to opt out." Context engineering as a protocol-level default.
- **The agent is quiet while working.** "The current ACP build is quiet on stdout during a turn, so
  `buzz messages get` is how you confirm it ran." Wait 10–90s. The channel is the only progress
  signal.

## Why this belongs in communication

The repo's own framing: "Agents are members, not bots." "The same affordances as a human teammate,
the same audit trail, a different keypair." That is an addressing and permissioning model, and it
answers a question my `communication/README.md` open questions do not yet touch: how do you talk to
an agent that is *not* sitting in your terminal?

Everything in `communication/` so far assumes a one-to-one session — me, a prompt, an instruction
file. Buzz assumes a room: many humans, many agents, one signed event log, and scoping done by
identity rather than by permission flags. Instructions stop being a file you hand over at session
start and become messages other participants can see, react to, and thread.

Open threads to chase:

- Does the mention-and-wait pattern survive contact with a busy channel, or does it need something
  like a typing indicator? The quiet-during-turn behaviour above suggests the gap is real.
- What is the equivalent of `AGENTS.md` here — is the channel description the durable instruction,
  or the NIP-AE core memory, or both?
- `BUZZ_ACP_RESPOND_TO` is a crude gate. What does a good "who may task this agent" model look like
  when everyone in the room can post?
- Multi-agent: the README claims agents can "orchestrate other agents". Agent-to-agent traffic in a
  channel humans also read is a different genre of communication problem than a subagent call.
- Is the shared audit trail actually readable by a human after the fact, or does the volume of agent
  events drown the human conversation?

Not read yet: `crates/buzz-acp/README.md` (parallel agents, heartbeats, forum subscriptions),
`VISION_AGENT.md`, `ARCHITECTURE.md`.
