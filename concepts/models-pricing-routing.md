# Models, Pricing and Routing

There is a lot of models with different capabilities and costs.
Important distinction here:

## Token costs, pricing

- free model vs. free inference: The model/weights may be free, but inferencing with it will
cost compute.
- token cost: input tokens way less expensive than output tokens (read cheaper than generate)
  => one reason to keep code generation focused
- token cost: New models more expensive, but results may require less tokens.
  If you want to optimize, measure first by using your own evals.
- token cost vs. speed: Quick answers keep the human in the zone, may result in less tokens spent
- MCP vs. CLI; huge AGENTS.md: Lots of tokens upfront that may never repay their cost
- use token-efficient output instead of JSON, caveman instead of prose etc.

## Which model to use per task

Very opinionated. Taking into account only a couple of people here. This is moving very rapidly and highly biased. 

### Kun Chen

This is taken from the video of [Kun Chen with David Ondrej](https://www.youtube.com/watch?v=8ZgpAXe5V5w)

| Task | Model | Harness | Why |
| --- | --- | --- | --- |
| Orchestration | GPT-5.6 Sol @ x-high | Pi | Juggles huge context and needs real reasoning. X-high is his sweet spot; surprisingly fast, and it doesn't burn quota the way Ultra does. |
| High-complexity tech / product design | Fable | Claude Code | Depth and creativity. |
| Day-to-day coding | GPT-5.6 Sol, reasoning dialed per task | Pi | Little reason to use a smaller model: dialing Sol's reasoning level down gives a smarter model at lower cost. |
| Adversarial code review | GPT-5.6 Sol @ medium | No Mistakes | GPT models since 5.5 are the most thorough edge-case reviewers he's compared. |
| News / X research | Grok 4.5; "Opus on fast mode" | Grok Build | Bundled with his existing X subscription. |

## Dos and don'ts

- Anthropic models are restricted to Claude Code; using other harnesses means you have to pay API pricing
- Grok Build gives free X API read/search access that would otherwise cost money
- Ultra modes tend to aggressively fan out sub-agents, and every sub-agent is also ultra. A token bonfire.
- Weak models on hard problems. On the DeepSuite benchmark (new enough to be uncontaminated),
  Sonnet 5 at max reasoning costs more than Fable while being less capable.
  A model that isn't smart enough just burns tokens failing.
- Codex CLI as a harness: Good out-of-the-box image generation, but weak at background processes
- API pricing: For individuals, subscriptions are the only sane option
  stack subscriptions across vendors
- Local / open-source models: Resource issue. A local model would compete with everything else
  cloud-hosted open models don't save enough to be worth switching (for individuals)

## Gisting

https://shopify.engineering/gisting
Means compressing the system prompt into the model. Only applies if you RL train your own models
