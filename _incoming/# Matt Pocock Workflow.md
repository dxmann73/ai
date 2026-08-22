# Matt Pocock Workflow

[Overall idea](https://www.youtube.com/watch?v=-QFHIoCo-Ko&t=3175s)
Approach: Top loop until PRD, bottom loop implementation and QA

Conditions for success:

- Context window, "Dumb zone" => need to keep stuff within 250k
- build [layer by layer](https://youtu.be/-QFHIoCo-Ko?t=2582)

## Example: Loose spec to code

- Get a loose question from Sarah Chen PM

/grill-me: 
- Refine and clarify requirements
- explore codebase, answer questions
- ALIGN the LLM with the idea
- Result: Gold = Tokens that contain the research
- use caveman here already
- need to keep things that are out of scope, helps with DoD

https://youtu.be/-QFHIoCo-Ko?t=1857
/write-a-prd: summarize the design concept
- Summarize everything into a PRD (templated)
- Create User Stories / Tickets from it (templated)
- No real use in reviewing: means reviewing the ability of the LLM to summarize

https://youtu.be/-QFHIoCo-Ko?t=2418
/prd-to-issues
- create kanban board with issues
- goal: break up work, make dependencies visible/explicit
- favor vertical slices to make loops tighter / earlier feedback 
- prepare for parallelization (DAG) of work

https://youtu.be/-QFHIoCo-Ko?t=3241
Implementation
- totally autonomous, parallel agents (basically a Ralph loop)
- grab issues, grab last 5 commits; run ralph loop
- within loop, develop using docker sandbox
- use TDD approach (red/green), coding before that will pollute the context window, make the LLM cheat on tests more
- final phase is Code Review/QA generated code, but NOT in the same context window (=dumb zone)
- feedback loop very very important, will inform result ceiling

https://youtu.be/-QFHIoCo-Ko?t=4191
Manual QA
- always important
- introduce taste, otherwise slop
- ! QA is making sure requirements stay stable / solved 

https://youtu.be/-QFHIoCo-Ko?t=4536
Codebase architecture
- Make sense of dependencies over a lot of modules
- Context window problem here!
- Well testable / make sense of for LLMS: [deep modules](https://softengbook.org/articles/deep-modules)
  Reasoning:
    People are working harder that before with AI and know the codebase less well.
    https://www.youtube.com/watch?v=-QFHIoCo-Ko&t=4765s
    Deep Modules allow to manage focus / context for humans as well; delegate what's inside the module

https://youtu.be/-QFHIoCo-Ko?t=4870
/improve-codebase-architecture
- intended here to deepen modules (and do other stuff)
- enforce any constraints
- !! could do this differently by keeping the context from each module as seen from every module
  mille plateaux

[How to enforce coding standards, design etc.](https://youtu.be/-QFHIoCo-Ko?t=5271)
- pull (skills) vs. push (AGENTS.md)
- Implementer should pull, Reviewer should push
- Example [Sandcastle loop](https://youtu.be/-QFHIoCo-Ko?t=5559)
  => Sonnet for coding, Opus for loop

[Do we keep the PRDs?](https://www.youtube.com/watch?v=-QFHIoCo-Ko&t=5004s)
- no clear answer, code will change under the PRD
- even the requirements may have changed
- doc rot => do NOT keep the PRDs, mark the issue as closed
- BUT we might want to keep the intent, right?


[grill-with-docs](https://www.youtube.com/watch?v=6BB6exR8Zd8)
- grill together using shared memory of domain language
- ubiquitous language (from DDD) skill
- use grill with language https://youtu.be/6BB6exR8Zd8?t=296




