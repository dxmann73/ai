> Captured from website `src/content/ai/en/communication.mdx` on 2026-08-08. Verbatim, unedited.

---
title: "AI — Engineering"
---

# Communication

## How LLMs ally work

See [this article](https://www.0xkato.xyz/how-llms-actually-work) on how LLMs actually work. Things to keep in mind:

- lost in the middle” problem: Models use information at the start and end of long prompts more reliably
  That’s why prompt engineering tips like “put important context first” or “repeat key info at the end” actually help
- Attention has one big cost. In full attention, each token compares against all the tokens it is allowed to see,
  so doubling the prompt length roughly quadruples the work. This is why long prompts are expensive to run.
- The model does not just pick the highest-probability token every time, depending on decoding settings like temperature

## First principles

Since we are communicating with AI, we need to remember first principles

Important differences here

- no need for emotional management
- no need for "Einwandvorwegbehandlung"
- context window => want to be concise

## Control the context

AKA prompt engineering; Most important thing
steipete: If agent does not get it right, start with another prompt until it one-shots

### Context size

Matt Pocock: "Smart Zone", /handover skill

Steve Yegge [Inventing Beads - The End Of Lost Work](https://steve-yegge.medium.com/introducing-beads-a-coding-agent-memory-system-637d7d92514a)
> LLMs notice problems as they work, but fail to act on them in any way. This happens when they are pressed for space.
=> Explicit mention of issues being connected to context window size, from multiple angles throughout the article.

Theo and others: Big AGENTS.md considered harmful.

### Content

#### Prep phase

Big AGENTS.md considered harmful, but need to inject the proper things somehow, so necessary anyway?
=> TODO pre-phase where check for everything we will need FOR SURE, let the agent pick the rest as it is going along
What is available? Don't be the reason holding your agents back. Give them access to everything!

#### Find relevant Content

Reduce noise in the context:

- Firecrawl
- Caveman
- [ASTgrep](https://astgrep.com/blog/ast-grep-outline.html?ck_subscriber_id=2755325874)
- [The new rules of context engineering, by Anthropic](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models?ref=aisecret.us)


#### Connnecting external sources/sinks (=MCP)

[CLI vs. context bloat throgh MCP](https://ksinder.medium.com/the-battle-for-context-why-mcp-vs-cli-is-the-wrong-fight-3c0cb63849a4)
https://beads.gascity.com/integrations/claude-code#why-cli-+-hooks-instead-of-mcp

### Watch your language

TODO: AUTOMATE AGENTS that check this; also TRAIN HUMANS

#### Human -> agents

Concise, no filler words; no need to apologize

#### Agents -> humans

Agent answers / processing will fill the context, too!
Caveman, concise, steipete AGENTS.md
Output plan as HTML vs. md
https://www.youtube.com/watch?v=S9EGx6ik-18&t=64s
What to say, what not to say (plans: What are we NOT doing does not belong in there)
