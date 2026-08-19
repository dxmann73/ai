# Business

Bringing agentic methods into an organization: what changes, what breaks, and what it costs.

Die Unternehmen fragen sich natürlich auch: Wird mein Businessmodel überleben?
Wird die Konkurrenz Wege finden, besser zu sein als ich?
Wie kann ich diese ganzen neuen Sachen hebeln? Wo ist der Haken?

## Scope

- AI Adoption is a cultural change
- Adoption paths: pilots, champions, mandates, and why each succeeds or stalls.
  => Video about this in the backlog, AI Leaders or smth
- Org design: team shape, role changes
- Economics: token cost versus salary cost, measuring
- Risk and governance: IP, data handling, compliance, audit trails, liability for agent output.
- Vendor strategy: lock-in, model portability, build-versus-buy for internal tooling.
- Measurement: what to track when lines of code and velocity stop meaning anything.

## Human factor

- Burnout, agents get the easy parts done, what remains is hard decisions
  [Tim O'Reilly and Steve Yegge](https://youtu.be/CQKuitliNmc?t=787)
- Comprehension rot

Bring humans aboard

- AI is here to stay; adapt or stay behind
- Car analogy:
  - You don't NEED to use it, you could still ride horses
  - Yes it is dangerous, you need to know what you are doing
- Book analogy:
  - You don't need to read those newfangled things called books, keep telling each other
    stories from memory. Really, there is no need. Keep missing out though.
- Fire analogy:
  - You don't have to cook your stuff over a fire, it's dangerous and you can hurt yourself
  - You can totally keep eating raw stuff, digestive problems and all. It works fine!

## Strategy shift

### Own your verification

https://arxiv.org/html/2602.20946v2
For companies, the core strategic insight is that verification is no longer a mere compliance function, but a primary production technology—and increasingly, their most defensible one. This dictates a structural shift:

- investing heavily in observability
- expanding verification-grade ground truth
- and reorganizing around a “sandwich” topology (human intent → machine execution → human verification and underwriting)

In an economy where raw output is commoditized, competitive advantage migrates to the scarce talent
and data capable of reliably steering and certifying agentic systems—generating network effects not
in sheer output, but in trusted outcomes.

### Nadella: Own your loop with humans+AI

[A frontier without an ecosystem is not stable](https://x.com/satyanadella/status/2066182223213293753)

This means the real opportunity is not in picking the best model but instead in building a learning loop on top of models where human capital and token capital compound. You can offload a task, or even a job, but you can never offload your learning. The future of the firm is the ability to compound that learning across people and AI.

This requires a new architectural approach where every business is able to build agentic systems that
improve over time, while still retaining control over their IP.
A company should be able to switch out a “generalist” model without losing the “company veteran” 
expertise built into their learning system. This is the key “test” of your control and sovereignty
.
Companies need to turn their workflows, domain knowledge, and accumulated judgment into AI systems
 that improve with each use. Private evals should capture whether a model is actually improving 
 against outcomes that matter to the business (not just external benchmarks!). 

The companies that build this early will have an advantage that is hard to replicate

### SaasPocalypse

https://catalini.substack.com/p/surviving-the-ai-moatpocalypse

> Of course, better models cut both ways, as the best SaaS providers can expand their offering and out-iterate internal teams by observing evolving needs across a wider customer base. They can also use this historical opportunity to switch from selling seats to selling the end-to-end work—assuming they are willing to underwrite the risks and become liability-as-a-service (LaaS) providers.
> In many cases, you’ll have a mix of SaaS contracts being cancelled when the internal version recreates the 10% of specific features a company actually needs, and larger contracts being signed where SaaS has become robust LaaS. Just look at how rapidly Stripe and Ramp have turned from fintechs to foundation labs focused on finance and payments: there will be no SaaSpocalypse for the firms that leverage the tools to capture more of the value chain.

> How do you survive the moatpocalypse? It’s simple.
> You obsess about what is currently measured vs. not. You use what is measured to automate your execution, and what is not measured as an opportunity to build new, unique, proprietary data in the domains that matter to your workflows. To do so, you inevitably need to hire the best global talent to perform verification, and set up systems that can feed back their decisions, evals and experience into the next training run. That loop is how you establish and defend the only type of network effect left, the verification-grade one.
