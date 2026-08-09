---
status: seed
tags: [reference]
---

# Glossary

Terms as used in this repo. The field's vocabulary is unstable and contested — these are working
definitions, not authoritative ones.

## AGLC — Agentic Development Life Cycle

The process by which software gets built when agents do most of the authoring and humans specify,
steer, and verify. Contrast with the SDLC. See [`../aglc/`](../aglc/).

## Agent

An LLM given tools, a goal, and the ability to loop — acting, observing results, and acting again
without a human turn in between. The loop is the distinguishing feature.

## Harness

The scaffolding around a model that makes it an agent: tool definitions, permission handling,
context management, session state. Two harnesses over the same model can differ enormously in
capability.

## Context engineering

Deciding what information is in the model's context window at a given moment, and what is
deliberately kept out. Broader than prompting, which concerns what is said rather than what is
loaded.

## Instruction file

A durable, version-controlled file of standing directives for an agent (`AGENTS.md`, `CLAUDE.md`).

## Eval

A repeatable measurement of whether an agent setup produces acceptable output on a defined task
set. The agentic equivalent of a test suite, and about as commonly neglected.

## Human in the loop

A design where a person approves or corrects agent actions before they take effect. Distinguish
from _human on the loop_, where the person monitors and can intervene but is not a required step.

## MCP — Model Context Protocol

An open protocol for connecting agents to external tools and data sources through a standard
interface, rather than bespoke integration per tool.
