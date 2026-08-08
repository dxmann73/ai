# Adoption journey — reconstruction, gaps, and a plan

A dated reconstruction of my own AI/agent adoption from the evidence lying around, plus what is
still missing, plus how to turn it into an `adoption/` area here and a post on the website.

Assembled 2026-08-08 from Chrome bookmarks and history, the Obsidian vault, and the git and GitHub
history of my repos. Revised the same day after a first pass got several phases wrong.
**[claim]** marks my own recollection with no artifact behind it yet.

## Where the evidence lives

| Source | Coverage | What it is good for |
| --- | --- | --- |
| Chrome bookmarks, `#read/ki/**` | 187 links, 2023-11-12 → 2024-07-28 | The ChatGPT-era reading list |
| Chrome bookmarks, `CC/AI/**` | 132 links, 2025-06-30 → 2026-08-07 | The agentic-era reading list |
| Obsidian `KI/` | 150 notes, 2023-11-20 → 2024-04 | Phase-0 thinking, in my own words |
| Obsidian `Dailies/` | 93 notes, 2023-11 → 2026-07 | The only source that spans the middle years |
| Obsidian `docs/Projekte/` | 13 project notes, current | Where the present phase is being designed |
| `nomap` git history | 350 commits, 2025-07-10 → 2026-06-15 | The practice: configs, specs, plans |
| `agent-box-setup` git history | 167 commits, 2026-01-31 → 2026-08-06 | The tooling/harness build-out |
| GitHub repo metadata (`gh repo list`, incl. archived) | 2019 → 2026 | Creation dates, including deleted-from-disk projects |
| Chrome history | `urls` reaches back years; `visits` only to 2026-05-10 | Read dates, where bookmarks are silent |

Two caveats that matter for every date below. Bookmark dates are `date_added` — when I filed a link,
not necessarily when I read it. And Chrome prunes its `visits` table: per-visit timestamps only
exist from 2026-05-10 onward, so for anything earlier there is a single `last_visit_time` and no
visit count history.

The bookmark extraction is reproducible. Chrome stores them as JSON with WebKit timestamps
(microseconds since 1601-01-01) at
`/mnt/c/Users/dave/AppData/Local/Google/Chrome/User Data/Default/Bookmarks`:

```python
import json, datetime
def webkit(ts):
    return datetime.datetime(1601, 1, 1) + datetime.timedelta(microseconds=int(ts))
# walk roots -> children, node['type'] == 'url' -> (name, url, date_added)
```

## The phases

### Phase 0 — ChatGPT as a subject of study (2023-11 → 2024-04)

Starts abruptly in November 2023, which matches how I remember it. First AI bookmark is 2023-11-12,
heise's "Mach Dir Deinen eigenen ChatGPT-Bot", followed the next evening by the OpenAI DevDay
keynote — so the trigger was GPTs and the DevDay announcements, not ChatGPT's launch a year earlier.

Then a burst: 72 links filed in 2023-11, 81 in 2023-12. On 2023-11-18, between 07:10 and 07:20, I
shortlisted ten Udemy courses in one sitting — seven on prompt engineering and the OpenAI API, plus
three on classical ML/DL. The filing is a single morning; **the courses themselves I worked through
later, on and off, extending into the middle of 2024.** The bookmark timestamp is the shopping list,
not the study.

The vault is where the reading got digested: 150 notes in `KI/`, peaking at 93 files in 2023-12.
Notable is `Prompt Engineering und Manipulation`, my own taxonomy written 2023-11-27 to 2023-11-30 —
`01 Reduktive Methoden`, `02 Transformative Methoden`, `03 Generative Operations`,
`04 Latentes vs. emergentes Wissen`, `05 Kreativität vs. Halluzination`. That is the earliest
artifact of me building a model of the thing rather than collecting links about it.

Also present: `MemGPT and SPR`, `Vectors, DBs, RAG, FAISS`, `GPTs`, `Adoption Enterprise and
Challenges`, `Hype`. The `Code` folder is thin and entirely about assistants, not agents: JetBrains
AI Assistant, Copilot Chat GA, "Software-Testing mit ChatGPT als Assistent".

Worth calling out because it is the direct ancestor of `#incoming/` and `#log/`: a bookmark folder
literally named `#read/ki/# obsidian done` holds 105 links — the ones already processed into the
vault. Same pipeline, different tools, three years earlier.

Skeptic material arrives on schedule in 2024-04: the Devin debunking videos, "Why I Quit Copilot",
"GitHub CoPilot Is Ruining Code Quality".

### Phase 1 — the daily-driver years (2024-05 → 2025-06)

The `KI/` vault stops in 2024-04 and the `#read/ki` bookmarks thin out to almost nothing, which on
the first pass looked like a dropout. It wasn't. Two things were happening that the bookmark record
just doesn't capture.

First, the capture moved into `Dailies/`. AI-titled daily notes continue at a low but steady rate —
8 in 2024-05, 2 in 2024-06, 3 in 2024-07, 3 in 2024-11 — mostly heise KI-Update digests, plus
pieces that read as early unease: "Wozu das Programmieren auf Prompt-Ebene führt" (2024-06-24),
"Entwickler sind von KI nicht begeistert, ihre Manager schon" (2024-07-28), "KI-Modelle lernen
auswendig und schlussfolgern nicht" (2024-07-28). Last AI daily of the run: 2024-11-13.

Second, and this is the part with no artifact at all: **ChatGPT became a tool instead of a topic.**
Weekly at first, then daily, across personal questions, coding, and everything else — broad and
unremarkable enough that nothing about it got written down. That is exactly why the record is thin
here; there was nothing to file.

**January 2025: Cursor enters, via tab-completion.** Autocomplete and tab-accept, used mainly on
Terraform and on generating Terraform and AWS artifacts for project B. It did not go well — that
attempt was largely unsuccessful, and the experience stayed mixed from there. This is the first time
AI is inside the editor rather than in a browser tab, and it is the phase I skipped almost entirely
in the first pass of this reconstruction.

**[claim]** on the project B specifics — no repo in `~/projects` and no bookmark covers it. The only
GitHub repo created in that window is `azure` (created 2025-01-05, "Development area for Azure
Collections", now archived), which is the right date but the wrong cloud, so treat it as a lead
rather than the artifact.

### Phase 2 — building something of my own (2025-07 → 2025-12)

The blocks project starts. GitHub says `blocks-fe` was created **2025-07-10**, which resolves the
"there was a repo before nomap and I'm not sure of the date" question — that date is also the first
commit in what is now the local `nomap` repository, because `nomap` *is* that history, renamed.
`blocks-be` follows on 2025-10-05, then `blocks-doc` and `blocks-docs` on 2025-11-07 and 2025-11-08.
All three `blocks-*` repos are archived today, last pushed 2026-02-15 to 2026-02-22 — the week the
monorepo was assembled ("Add 'apps/backend/'", "Add 'apps/docs/'", "build: bootstrap npm Turborepo
root", "docs: add monorepo migration plan", all 2026-02-22) and the GitHub `nomap` repo was created
(2026-02-22).

The working method here was **ChatGPT with code copied back and forth by hand**, on and off, from
mid-2025. Not an agent, not in the editor — a browser tab and a clipboard.

Reading resumes in a new bookmark tree, `CC/AI`, from 2025-06-30, within two weeks of the first
commit. The tooling switch has a precise date: on **2025-08-02**, in one morning, I filed
Anthropic's "Claude Code Best Practices", "Onboarding for coding agents", and three Claude Code
deep-dive videos. Before that morning the reading is about assistants; after it, about harnesses.

Autumn 2025 is context engineering: humanlayer's `ace-fca.md` (2025-10-01), "MCP is the wrong
abstraction" (2025-10-08), Simon Willison's "Vibe engineering" (2025-10-12), and Cursor 2.0
(2025-10-30).

December 2025 is the crunch, and it is not the story I told myself. There are 58 commits in `nomap`
that month, the largest burst until then, and agent config gets consolidated mid-stream — 2025-12-09
"chore: merge .cursorrules and .instructions into AGENTS.md", then 2025-12-20 "feat: add AGENTS.md".
So rules files predate December and got tidied during it.

But the daily note from **2025-12-30, "Stand Projekt Notion nervt mich"**, says the crisis was about
*structure*, not about code. Notes scattered across paper slips, roughly a thousand voice memos at
five minutes each (about a hundred hours of listening), Notion, Obsidian, and hand-linked Markdown
in `blocks-docs` — with the explicit complaint that none of the tools let content be referenced,
embedded, or re-ordered. It ends: "Die einzige Lösung kann also nur sein, das Tool direkt zu bauen,
mit allen Requirements, so wie sie rein kommen."

So the accurate version of "vibe coding gone wrong" is narrower than I first wrote it: the code was
accumulating without specs or docs, yes — `docs/sdd/` does not exist before 2026-03-11 and the first
plan artifact of any kind is 2026-02-22 — but what actually hurt at the end of 2025 was that the
*thinking* had no home. The tool being built and the problem being felt were the same problem.

The `blocks-docs` history shows the counter-move already underway before the crisis: "docs: Define
scope" (2025-11-11), "feat: Start defining use cases" (2025-11-22), "docs: adds use cases and
features" (2025-12-06), "docs: rewrite premises for prompt input pt.1 wip" (2025-12-04), "docs:
initial data model; MD import" (2025-12-15). Thirty-six commits of specs, use cases, features and
PRDs between 2025-11-07 and 2026-02-22 — a specification effort running in a separate repo while
`nomap` had none. The crisis was not an absence of writing; it was that the writing lived somewhere
the code could not reach.

### Phase 3 — the agentic-engineering turn (2026-01 → 2026-02)

This has a single dated document behind it, and it is unusually explicit. The daily note
**2026-01-01, "Steipete und AI Workflows"**: FOMO at watching Peter Steinberger ship, and three
conclusions — get hands-on with MCP, CLIs, agents, models, the whole setup; work on far more things
and actually interact with the model; and the diagnosis that matters:

> Im Moment ist mein Workflow noch, dass ich erst alleine überlege, und dann irgendwann wenn ich
> fertig bin kommt die KI zum Zug. Das ist viel zu spät!

Followed immediately by the concrete blockers — finish the export so it could ship, stand up a real
Elasticsearch instead of Testcontainers, build the location-creation frontend, and untangle a
documentation structure that had gotten lost between use cases, features and pitches — each with the
same self-directed answer: *ask the model, use it as sparring, stop doing this alone.*

This also settles a question I had it wrong on. I remembered reading Steinberger *before* the
December mess. The bookmarks for "Just Talk To It" and "Shipping at Inference-Speed" are dated
2026-01-01, the daily note is dated 2026-01-01, and the note reads as first contact. His posts are
older than that, but my encounter with them is New Year's Day 2026 — after the December crisis, not
before, and plausibly *because* of it.

What follows is fast:

- 2026-01-04, "Live Coding Session: Building Arena" filed.
- 2026-01-14, "How I code with AI right now" bookmarked four times at different timestamps —
  `t=1951s` (Greptile), `t=2143s` (agentic loop), `t=2400s` (feedback loop / rewrite the prompt),
  `t=2830s` (Zusammenfassung) — alongside Simon Willison's "Designing agentic loops". Not news
  consumption; note-taking inside a workflow talk.
- 2026-01-31, `agent-box-setup` initial commit. The setup becomes a project in its own right, 167
  commits by 2026-08-06.
- 2026-02-01, first agent attribution in a commit trailer: `social-linkedin`,
  `Co-Authored-By: Claude Opus 4.5`.
- 2026-02-13/14, `nomap` gets "chore: Sync AGENTS and setup" plus three "chore: Claude setup"
  commits.
- 2026-02-19/21, "docs: spec refinement pt 1", "docs: Draft scope and ERM", "docs: move to prds".
- 2026-02-21/22, `dave-box-setup` created; the monorepo migration; `blocks-*` archived.
- 2026-02-27, retrofit sweep: `dave-tax-advisor` starts with agent config, `website-old` gets
  "chore: add agent setup (AGENTS.md, markdownlint, gitignore updates)" the same day.

`nomap` carries 13 commits with a `Co-authored-by: Cursor` or `Made-with: Cursor` trailer, dated
2026-02-22, 2026-03-14 and 2026-03-29.

### Phase 4 — spec-driven development, for real (2026-03 → 2026-05)

Dated by artifact, not by memory. `nomap` gets the whole `docs/sdd/` tree in one commit on
2026-03-11, "docs: SDD pass" — eleven numbered documents from `01-purpose-and-scope.md` to
`11-risks-decisions-open-issues.md`, a README, and ADRs 0001 (OpenAPI as contract) and 0002 (entity
ID conventions) with them. ADR 0004 on normalizing frontend API errors follows 2026-03-14, 0005 on
2026-03-15, and 0003 only lands 2026-04-19 — the numbers were reserved before they were written. A
big consolidation lands 2026-04-20 ("Big doc cleanup sweep": interface API specification,
cross-cutting concerns, frontend/backend/persistence design, five more ADRs), and 2026-04-22 is
"docs: Move guidelines to ADRs where possible". Use-case-level planning appears 2026-05-14, "docs:
uc-003 preparation and planning".

The plan problem is a bigger fossil than I first wrote, and the git history has the whole arc.
**Plans were deliberately throwaway artifacts, deleted as I went along, in every repo, for four
months.** The rule was written down in `nomap`'s `AGENTS.md`: *"NEVER use markdownlint on plans.
Plans will deleted later. We do not need to lint them."*

The sequence:

- **2026-02-22** — first plan file, `plans/turborepo-monorepo-migration-plan.md`, on the day of the
  monorepo migration itself. `.plans` is in `.gitignore` at this point, so anything under it is
  invisible to git by design.
- **2026-02-24** — `.plans/2026-02-24-e2e-health-header-docs-plan.md` gets committed anyway, the
  only file that ever made it in past the ignore rule.
- **2026-02-25**, commit **"fix: plans as first class citizens"** — `.plans` comes out of
  `.gitignore`, `.plans/` is deleted, `plans/` becomes the location. First-class in name only: the
  same commit deletes `plans/turborepo-monorepo-migration-plan.md`, four days old, and the
  delete-after-use pattern runs from here.
- **2026-02-22 → 2026-05-16** — 44 plan files added to `nomap` under `plans/`, `.plans/`,
  `docs/smd/plans/`, `apps/backend/docs/plans/` and `apps/frontend/docs/plans/`. Nearly all are
  deleted in a later commit, usually the one that finished the work: "chore: plan cleanup"
  (2026-03-14), "chore: cleanup" (2026-03-29), "chore: refined sdd/adr naming" (2026-05-14).
- **2026-06-15**, commit **"chore: track plans in git and drop throwaway-plan rule"** — the reversal.
  The `AGENTS.md` line loses "Plans will deleted later" and gains "Plans are tracked in git under
  `plans/`". Three files land with it and are the only plans surviving in `nomap` today.

So the fossil record is inverted from what I assumed: the plans existed all along and were destroyed
on purpose, by a documented rule, for roughly sixteen weeks. Only the commit history remembers them
— every deleted file is still recoverable from git, which makes this the best-evidenced failure mode
in the whole reconstruction rather than the worst.

Yegge's Gas Town describes ending up with more plans than he could make sense of. My version is the
stage before: the plans were treated as scaffolding, and the cost only became visible when
use-case-level work started needing yesterday's reasoning back.

The other repos follow the same rule at smaller scale — `dave-tax-advisor` deleted both of its plans
on 2026-02-27, the day it created them; `website` deleted plans on 2026-05-30 and 2026-06-15. And
`agent-box-setup` never tracked a plans directory at all: its only trace is the markdownlint
exclusion (`plans/**/*.md`, `**/*-plan.md`, restated 2026-03-22 "docs: ignore plans in markdownlint
(again)"), so whatever plans it had were written and deleted without git ever seeing them. That one
is genuinely unrecoverable.

Reading here is the disillusionment layer: "Agentic Coding is a Trap" (2026-05-06, filed again
2026-05-11), Margaret Storey on cognitive debt (2026-05-06/07), Fowler fragments, Karpathy's "From
Vibe Coding to Agentic Engineering" (2026-05-13), Addy Osmani's "Don't Outsource the Learning"
(visited 2026-05-17). Matt Pocock's full workflow walkthrough is filed 2026-05-24 at `t=3175s`, and
his `/grill-me` post was read 2026-05-16.

Pocock's alignment workflow is the one I meant to adopt and did not get to. The skill is installed
and referenced in `#incoming/aidlc.md`, but no git history in any repo shows it being used.

Two markdown-only projects appear in this window and are worth naming as a category shift: `macros`
(2026-05-19, "Pure markdown + AI agent approach for daily nutrition logging") and `website`
(2026-05-30) — projects where the agent instructions *are* the program.

### Phase 5 — orchestration and loops (2026-06 → now)

June 2026 is process rethinking: "AI agents forced us to completely rethink our agile PDLC"
(2026-06-05), Cursor's hardcore review skill, Zechner on the future of work, Uber's exhausted token
budget, Addy Osmani's orchestration tax.

The Yegge sequence is now dated from browser history rather than bookmarks, and it is spread over
ten weeks, not two days: a Google search for "beads framework steve yegge" on 2026-05-24, "Beads
Best Practices" on 2026-06-21, then "The AI Vampire", "Gas City" and "The Flat Curve Society" on
2026-07-14, a re-read on 2026-07-17, "Welcome to Gas Town" on 2026-07-18 (a New Year post read in
July), the Beads repo on 2026-07-19. Then the whole of `yegge.ai` browsed on 2026-08-04 and
2026-08-05 — engagements, CIO page, Gas Town, services, atlas, bibliography, the Wheelhouse images.
And on **2026-08-08 at 08:13**, both parts of "The Shape of Things to Come".

2026-07-25 is the spec-kit / spec-driven-development cluster. August 2026 is loop engineering
(kirupa, 2026-08-01), Cursor's "Agent swarms and the new model economics" (2026-08-04), Flue agent
hooks, "Deterministic Core, Agentic Shell", Cloudflare's `@cloudflare/computer`.

The current phase is not only being read about, it is being designed, in the vault. `docs/Projekte/`
lists thirteen projects, and three of them are this:

- **`00-adlc.md`** (last touched 2026-08-07) — a full AIDLC design: compute and sandboxing per
  agent, token efficiency, context management, cost efficiency per-model, orchestration options
  (factory.ai, 8090, Mastra, Sandcastle, Cavekit, spec-kit, Gas Town/Gas City), ten personas with
  their owned artifacts (PO/PM, UX, architect, test coordination, operations, dev lead, CISO, CEO,
  marketing, knowledge management), the trigger and state-transition model for a spec, and "PDLC as
  Process-as-Code". This is the substantive version of what `#incoming/aidlc.md` sketches.
- **`08-AgentOrchestration.md`** — Beads/Gas Town notes, a customer-service agent, and a detailed
  maturity-gate ladder from worktree sandbox through shadow-prod to per-VM rollout, with
  aspect-tagged logs and a "saga" as the leading artifact for E2E tests.
- **`00-Ohana.md`** — an agent family with named personalities (Monk, Union Rep, LtWhorf, CdrData,
  Hub, Mac), mail-based A2A communication, Beads for work items, a per-project dashboard. Cites
  Yegge's "Model Welfare" essay as inspiration.

And `00-adlc.md` contains the origin spec of *this repository*, as a TODO list: new GitHub project,
GitHub Pages linked from the website, walk the `CC/AI` bookmark folder and check everything marked
done is in there, generate a log entry per item, later have an agent generate the log entries and
the changes, and post each log to X and LinkedIn. That is `#incoming/` → `#log/` → post, written
down before the repo existed.

## Corrections applied, and what is still open

Resolved in this pass:

- **The predecessor repo** is `blocks-fe`, GitHub-created 2025-07-10, archived; `nomap` is its
  history renamed and merged into a monorepo on 2026-02-22.
- **The middle years were not a gap** — daily ChatGPT use plus Cursor tab-completion from January
  2025, with `Dailies/` carrying the reading through 2024-11.
- **The Udemy courses** were filed in one morning in 2023-11 and worked through into mid-2024.
- **The Steinberger reading** is dated 2026-01-01 by both bookmark and daily note, which contradicts
  my recollection of having read him before December 2025. The note reads as first contact.
- **The December 2025 crisis** was a notes-and-structure crisis as much as a code one.

Still open:

1. **The loops and graphs posts.** Corrected attribution: Yegge, in "The Shape of Things to Come"
   Part 1, writes "We all saw Boris vaguepost that you should be building loops, and then Peter
   vaguereplied that you should be building graphs" — that is **Boris Cherny** on loops and
   **Peter Steinberger** on graphs, not Dario Amodei. I could not get the permalinks: Yegge's Part
   1 links only one X post (`https://x.com/doodlestein/status/2012740971088289858`, unrelated),
   search surfaced only secondhand coverage, and Chrome history has no visit to either post — the
   closest traces are a Theo reply to `@bcherny` (`https://x.com/theo/status/2054717936024383829`,
   visited 2026-05-15) and a Nakazawa reply to `@steipete`
   (`https://x.com/cnakazawa/status/2055203307724132612`, same day). So: **[source needed]** for
   both originals. Separately, `#incoming/loops.md` records two more unsourced lines — "Steipete:
   I'm just orchestrating loops now" and "Dario: My job now is to engineer loops LOL" — which are
   probably a different pair of remarks. Do not paraphrase any of these from memory in a post; get
   the four permalinks or drop the claims.
2. **"Revenge of the Junior Developer"** (Sourcegraph, published 2025-03) — read mid-2025, which
   fits Phase 1 and means the six-waves framing was in my head a year before I acted on it. The only
   browser trace is a Google search and visit on 2026-08-05, i.e. a re-lookup. Unverifiable from
   here; the mid-2025 read stands on recollection.
3. **Project B.** No artifact. `azure` (2025-01-05, archived) is the only candidate and the
   description says Azure, not AWS.
4. **Which Udemy courses got finished.** Udemy keeps completion records; the bookmarks don't.

## Steps to flesh this out

1. **Get the ChatGPT export.** Settings → Data controls → Export produces a `conversations.json`
   with per-conversation `create_time`. This is the only way to put numbers on Phase 1 — when weekly
   became daily, what the Terraform and AWS prompts actually looked like, and whether project B is
   recoverable from the conversation titles. Highest value of anything on this list.
2. **Pin the four X posts** in item 1 above, or cut them.
3. ~~**Mine the archived `blocks-*` repos** before they rot.~~ **Resolved: no mining trip needed.**
   The local `nomap` clone has four root commits and 350 commits total, because the 2026-02-22
   migration used `git subtree add` without `--squash`. All three histories came along whole:
   `blocks-fe` (38 commits, 2025-07-10 → 2026-02-15, also the repo's main line),
   `blocks-be` (35 commits, 2025-10-05 → 2026-02-22) and `blocks-docs` (36 commits, 2025-11-07 →
   2026-02-22). Reach them by hash rather than by path — the subtree parents keep their original
   root-level paths, so `git log -- apps/backend` shows nothing before the merge:

   ```bash
   git log 798b2452  # blocks-be
   git log 63658415  # blocks-docs
   git log bd3f438c  # blocks-fe
   ```

   GitHub archival is therefore a backup, not the primary. The real preservation risk is the
   deleted plan files, and those are recoverable too: `git log --diff-filter=D --name-only -- '*plans/*'`.
4. **Record publication dates next to filing dates** for the two dozen sources that carry the
   narrative. The gap between them is itself part of the story — Gas Town published in January and
   read in July is the clearest case, and there are others.
5. **Pull three before/after pairs out of the repos**, not more: the 2026-04-22 diff where
   `docs/sdd/` replaced ad-hoc guidelines, the throwaway-plan rule and its 2026-06-15 reversal
   (`c0872c9` → `74ad495`, the two commits that bracket it), and the 2026-02-22 monorepo merge.
   Concrete artifacts beat narration.
6. **Reconcile with Yegge's six waves and the Gas Town stage question**, and be honest about where
   my sequence is odd: theory in 2023, a year as a plain daily user, editor-tab-completion in early
   2025, and only then straight into harnesses — I largely skipped the assistant-in-the-IDE phase
   that most accounts treat as the middle step.
7. **Decide what is private.** Project B is client work. Fix now whether it appears as "a
   Terraform/TypeScript project" or by name, and keep that consistent between repo notes and post.
8. **Then write.** Posts are composed from `#log/` entries per this repo's posting workflow, not
   drafted from this file.

## The `adoption/` area

A new top-level folder, sibling to `aglc/` and `business/`, following the existing pattern: a
`README.md` index with Scope and Open questions, one topic file per note. Distinct from `business/`,
which is about organizations adopting agentic methods; this one is about the individual
practitioner's path, mine first and generalizable second.

Proposed contents:

- `README.md` — index, scope, open questions.
- `timeline.md` — the dated reconstruction above, cleaned up, `[claim]` markers either resolved or
  left visible. The reference artifact; posts draw on it.
- `phases.md` — the phase model as a model: what characterizes each phase, what triggers the
  transition, what breaks and forces the next one. Cross-referenced to Yegge's waves and stages,
  clearly attributed.
- `failure-modes.md` — the stages that went wrong, named: thinking with no home, implementation
  without specification, plans deleted by rule, plans kept but not indexed, alignment skipped. Each
  with the concrete symptom I hit and the date I hit it. The plan one is the best documented: a
  written rule (2026-02-25), sixteen weeks of deletions, and an explicit reversal (2026-06-15).
- Sources go in the existing `reference/reading-list.md`, which is empty and already has a format
  defined for exactly this. Do not create a second reading list.

Scope boundary for the README: adoption is the practitioner's trajectory. How the loop works belongs
in `aglc/`; organizations belong in `business/`.

One thing to get right: the vault's `docs/Projekte/00-adlc.md`, `08-AgentOrchestration.md` and
`00-Ohana.md` are ahead of anything in this repo. `adoption/` should reference them as the current
state of the design and let `aglc/` absorb their substance through the normal intake workflow — not
copy them, and not let this repo become a second, staler copy of the vault.

Open questions to seed the README with:

- Is the phase sequence necessary, or can someone entering now skip to the end? My own year as a
  plain daily user is a natural experiment on this.
- What forces each transition — a failure, a tool, or an article? On the evidence so far: a failure,
  then an article, in that order. December 2025 then 2026-01-01.
- Does the reading cause the practice or rationalize it after the fact? The dates give a partial
  answer and it is not entirely flattering.
- Which phases were wasted time in hindsight, and which only look wasted?

## The combined post on loops

Separate from the adoption post, and the one with a clock on it, since the material is three weeks
old.

Subject: three sources arguing about the same shift from three positions.

- **Yegge, "The Shape of Things to Come" Part 1: The Continuous Thunderdome** (yegge.ai, 2026-08;
  read 2026-08-08). Loops and graphs via Beads plus a small Markdown project brain; a bespoke
  harness (Wheelhouse) replacing the reusable one — he says Gas Town burned down under Opus 4.7's
  "just two more things" tic and that harnesses will all be bespoke; role agents; the end of human
  code review; CI/CD metamorphosing into a Land Rush; the Wish Factory. Three numbers set the price
  of entry: ~$87k/month of token burn, ~69 billion tokens in July at 96% cache hits, and harness
  work occupying 20–25% of all his project work. Part 2 (Model Welfare for Agentic Engineers) was
  read the same morning and is already cited in `docs/Projekte/00-Ohana.md`.
- **The loops post** — Boris Cherny, per Yegge's account. **[source needed]**.
- **The graphs reply** — Peter Steinberger, per Yegge's account. **[source needed]**.

The angle that makes this worth writing rather than summarizing: Yegge's own framing is "I am not
special, I'm just ahead of you", and what he is describing costs $87k/month and spends a quarter of
its effort on the harness. The honest question for anyone not in that position is which parts of the
loop survive on a normal budget — and `#incoming/loops.md` already holds the counterweights: Armin
Ronacher (the future, not there yet), Theo (worked, too expensive), and the Cursor swarm post's
finding that stacked, decorrelated review lenses were where the compute actually paid off. That is
an argument, not a link roundup.

Sequencing: this post and the adoption post pull in opposite directions — one is a forward look at a
frontier setup, the other a backward look at a personal path. Do not merge them. The adoption post
can reference this one as "where the road currently ends".
