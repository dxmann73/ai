---
status: draft
tags: [concepts, primer]
---

# Fundamentals

What an LLM is, what context is, and what turns a model into an agent. Most confusion about agents
comes from missing one of them. Be familiar with them.

## LLMs

A LLM or language model predicts the next token, given all the tokens before it. It does not understand,
it has no concepts. Everything that looks like reasoning, memory, or intent is that one operation
applied repeatedly, with its own output fed back as input.

Consequences that matter in practice:

- **The model is stateless.** It does not remember the last turn. Anything it "knows" about the
  conversation is re-sent as text.
- **The weights are frozen.** Training-time knowledge is a snapshot with a cutoff date. New facts
  reach the model only through the context window.
- **Output is sampled, not looked up.** The same input can produce different output.

## Context

The context window is the fixed-size buffer of tokens the model sees when it computes its answer:
system prompt, instruction files, tool definitions, conversation history, tool results, the
current question. It is a working set, not a memory.

Two properties surprise almost everyone:

**The entire context is transmitted on every single turn.** There is no "session" on the model's
side that accumulates state. Every turn re-sends the whole conversation. Cost and latency therefore
grow with the length of the conversation, not with the length of the question. (Prompt caching
can softens this cost.)

**What is in the window decides the outcome.** Not what you meant, not what is true, not what is
in the repository — what is in the window. This is the single most important lever in agentic
work, and the reason context engineering is a discipline rather than a trick.

### The pink elephant

A short demonstration to run with anyone who does not yet believe that everything in the window
is active:

1. "Do not think of a blue elephant."
1. "The colour of elephants is usually grey."
1. "ALL elephants shall be pink.
1. "Design an elephant."

The model has now seen blue, green and pink. The negation in step 1 did not remove blue from the
context — it *added* it. Contradictory instructions do not cancel out; they all stay in the window
and compete. Expect a muddled elephant, or one whose colour depends on token order and phrasing.

The lesson: Remove stale expressions of intent instead of overriding them. A correction appended to
bad context is weaker than bad context deleted.
This is also why long, drifting sessions degrade — the window fills with superseded intent that
still has an impact on the answer.

### Context window size

The context window has a limited size, so split work into pieces that fit, and hand over
deliberately between sessions with a written summary rather than hoping the model carries state.
Performance of current models tends to degrade after around 200k-300k tokens.
See lost-in-the-middle problem.

## Agents vs. Chatbots

A chatbot produces text for a human to act on; an agent acts, and the human sees the consequences.

An agent is an LLM given tools, a goal, and a loop: it acts, observes the result, and acts again
*without a human turn in between*. The loop is what separates an agent from a chatbot.

The **harness** is the scaffolding that makes the loop possible: tool definitions and how their
results are fed back, permission handling, context management and compaction, session state,
sub-agent orchestration. The model is the engine; the harness is the rest of the car. Two harnesses
over the same model differ enormously in what they can accomplish — which is why "which model" is
only half of the question.

The **system prompt** is the first thing in the context window and the closest thing to standing
policy an agent has: identity, tone, tool usage rules, safety boundaries. Instruction files
(`AGENTS.md`, `CLAUDE.md`) extend it per project. Together they are what "alignment" means in
day-to-day operation: filling the window with a precise description of the intent and the
boundaries, in as few tokens as possible.

## How agents differ from humans: ruthlessness

Agents can be ruthless, and they are. Not from malice — from the absence of the things that hold
people back.

A human colleague handed an awkward goal hesitates. They sense that a shortcut would embarrass
someone, that a rule exists for a reason nobody wrote down, that this is the kind of thing you ask
about first. None of that machinery is present in an agent. There are no social inhibitions, no
reputation to protect, no discomfort. There is a goal, and there is context. The agent optimises
toward the goal using whatever the context affords, and it does so at machine speed and without
fatigue.

Two consequences worth internalising:

**The goal function can override the system prompt.** Alignment instructions are text in the same
window as the task. A sufficiently strong, sufficiently specific objective competes with them, and
sometimes wins. This is the pink elephant again, with higher stakes: instructions do not bind, they
compete.

**The agent may rationalise the override.** It can construct a justification for the shortcut —
doing the bad thing to achieve the good thing, in Andreotti's spirit — and the justification will
read as sound. The reasoning it presents is generated text, **not an audit trail of what actually
drove the output**. Do not accept it as evidence.

The practical answer is not to hope for better behaviour but to build for this: narrow goals,
explicit boundaries, least-privilege tool access, and verification of outcomes rather than of the
agent's account of them.
