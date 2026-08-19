> Captured from website `src/content/ai/en/automation.mdx` on 2026-08-08. Verbatim, unedited.

---
title: "AI — Automation"
---

# Automation

https://hbr.org/2025/06/what-gets-measured-ai-will-automate
> anything that can be measured will be automated.
And it should be.

Humans in the loop create a lot of friction. But agents tend to forget what they are told,
first and foremost "after every major task, check everything is still correct".
There is a lot of movement on this at the moment. Some of it involving loops.

This means first and foremost to clearly define what done means, and to set boundaries on how long the loop is supposed to
try without asking a human, see [details in here](https://www.kirupa.chat/p/loop-engineering-explained-with-dirty)

## Noodle

[Noodle](https://poteto.github.io/noodle/introduction.html)

- aims to orchestrate running skills that get repeated often when working manually
  - agents forget to check guidelines before submitting PRs
  - same for test / review cycles
  - at the top end, users regularly add instructions for which tasks/bugs to tackle / triage
- you put all these steps into skills
- add separate skills with a ``chedule:` frontmatter part
- noodle will run them in a loop until finished
- skills need clear exit signal for this to work properly

Issue: Agents tend to go overboard frequently when doing adherence and coverage checks.
The question here is if it is wise to let them loop. The expectation seems to be that it
will be fine. Maybe though it just means you have to steer more, but later, after many tokens
have been burned already.

## Google AX (agent executor)

[Google AX GitHub](https://github.com/google/ax)

- distributed runtime / orchestrator with skills and harness support
- resumable streams, event log, audit logging

## Archestra

https://archestra.ai/
Agent runtime, MCP and LLM proxy, RBAC, Cost control

## Flue und FSM

Flues is designed to implement custom agent flows, customizable with [hooks](https://flueframework.com/docs/guide/agent-hooks/)
Pair this with an [FSM](https://blog.davemo.com/posts/2026-02-14-deterministic-core-agentic-shell.html) and you're good.

## check this out

https://medium.com/orgcraft/streamlining-the-product-development-lifecycle-with-generative-ai-agents-b006c07f02be
https://dust.tt/

## Software Factories

https://techcrunch.com/2026/08/18/warps-new-system-is-an-out-of-the-box-software-factory-for-ai-development/


