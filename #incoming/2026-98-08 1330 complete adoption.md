# Complete adoption

For every entry, compare against the adoption journey in adoption/README.md

## Source inventory

Surveyed 2026-08-08 while chasing the deleted `plans/` directories for
[`2026-08-07 1130 adoption-journey.md`](2026-08-07%201130%20adoption-journey.md). This is the map of
where the raw material for this repo actually lives, so a future intake run does not have to
rediscover it.

Status values: **captured** (a file exists in `#incoming/`), **ingested** (integrated into a topic
note and logged), **not started** (known source, nothing pulled yet), **n/a** (evidence, not
material — cite it, do not import it).

### Evidence sources

| Source | Where | Span | Status |
| --- | --- | --- | --- |
| `nomap` git history | `~/projects/nomap`, 350 commits, 4 roots | 2025-07-10 → 2026-06-15 | not started |
| `blocks-fe` history | inside `nomap`, subtree parent `bd3f438c` (also the main line) | 2025-07-10 → 2026-02-15 | not started |
| `blocks-be` history | inside `nomap`, subtree parent `798b2452` | 2025-10-05 → 2026-02-22 | not started |
| `blocks-docs` history | inside `nomap`, subtree parent `63658415` | 2025-11-07 → 2026-02-22 | not started |
| Deleted `nomap` plan files | recoverable: `git log --diff-filter=D --name-only -- '*plans/*'` | 2026-02-22 → 2026-05-16 | not started |
| `agent-box-setup` | `~/projects/agent-box-setup`, 167 commits, 45 skills | 2026-01-31 → 2026-08-06 | not started |
| Obsidian `docs/Projekte/00-adlc.md` | vault | current | captured, partly — see `aidlc.md`, `2026-08-07 1300 integrate ADLC resources.md` |
| Obsidian `docs/Projekte/08-AgentOrchestration.md` | vault | current | not started |
| Obsidian `docs/Projekte/00-Ohana.md` | vault | current | not started |
| Obsidian `KI/` (150 notes) | vault | 2023-11 → 2024-04 | not started |
| Obsidian `Dailies/` (93 AI notes) | vault | 2023-11 → 2026-07 | not started |
| Chrome bookmarks `#read/ki/**` (187) | Chrome profile JSON | 2023-11-12 → 2024-07-28 | not started |
| Chrome bookmarks `CC/AI/**` (132) | Chrome profile JSON | 2025-06-30 → 2026-08-07 | not started |
| Chrome history | `visits` only from 2026-05-10 | partial | n/a |
| ChatGPT conversation export | not requested yet | 2024 → 2026 | not started |
| Smaller repos: `dave-tax-advisor`, `website`, `website-old`, `macros`, `infra`, `spcsim`, `social-linkedin`, `dave-box-setup` | `~/projects/*` | 2026-02 → 2026-08 | n/a — corroborating dates only |

`k8s` has no git repo. `blocks-*` on GitHub are archived; the local `nomap` clone is the primary
copy of all three histories, so the archives are a backup rather than the source.

### Subject areas found, and where they go

Everything below is a topic the survey turned up that this repo does not yet cover. All topic
folders currently hold only their `README.md`, so nothing here has been ingested.

| Area | Found in | Target | Status |
| --- | --- | --- | --- |
| Throwaway plans as a rule, and its reversal | `nomap` `AGENTS.md` history, `c0872c9` (2026-02-25) → `74ad495` (2026-06-15) | `aglc/` | not started |
| Plan artifacts as a first-class deliverable — naming, dating, proof screenshots next to the plan | `nomap` `plans/`, 44 files | `aglc/` | not started |
| Spec-driven development in practice: SDD tree, ADR numbering, PRDs, use cases | `nomap` `docs/sdd/` (2026-03-11 onward), `blocks-docs` `specs/` | `aglc/` | not started |
| Specs living in a separate repo from the code, and why that failed | `blocks-docs`, 36 commits 2025-11 → 2026-02 | `aglc/` | not started |
| Agent config consolidation: `.cursorrules` + `.instructions` → `AGENTS.md` | `nomap` `3f88eb6` (2025-12-09), `2026-02-25` "AGENTS.md detox after Theo video" | `tools/` | not started |
| Rule-file hygiene — what belongs in `AGENTS.md` vs an ADR vs a skill | `nomap` `81155cf` "Move guidelines to ADRs where possible" (2026-04-22) | `aglc/` | not started |
| Skills as a portfolio: 45 skills, install scope, project-level split | `agent-box-setup` `configs/agents/skills/`, `TODOs.md` | `tools/` | not started |
| Linting policy for agent-authored Markdown, and the plan exemption | `agent-box-setup` `markdownlint` skill, `AGENTS.md:72` | `tools/` | not started |
| Repo-setup sync as a recurring chore (`sync-repo-setup`, "sync run") | `agent-box-setup`, `nomap` 2026-02-22 sync commits | `tools/` | not started |
| Agent attribution in commit trailers (`Co-Authored-By`, `Made-with: Cursor`) | `social-linkedin` 2026-02-01, `nomap` 13 Cursor-trailer commits | `aglc/` | not started |
| Markdown-only projects where the agent instructions are the program | `macros`, `website` | `aglc/` | not started |
| Monorepo migration via `git subtree` without squash, and what it preserved | `nomap` 2026-02-22, three merges | `tools/` | not started |
| AIDLC design: personas, artifacts, state transitions, PDLC as process-as-code | vault `00-adlc.md`, `aidlc.md` | `aglc/` | captured, not ingested |
| Agent orchestration, maturity gates, sandboxing | vault `08-AgentOrchestration.md` | `aglc/` | not started |
| Multi-agent families, A2A messaging, model welfare | vault `00-Ohana.md` | `communication/`, `society/` | not started |
| Loops vs graphs, harness economics | `loops.md`, Yegge "The Shape of Things to Come" | `aglc/` | captured, not ingested |
| Individual adoption trajectory, phases, failure modes | `2026-08-07 1130 adoption-journey.md` | new `adoption/` | captured, not ingested |
