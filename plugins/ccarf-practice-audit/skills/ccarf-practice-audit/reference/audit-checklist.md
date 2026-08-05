# CCAR-F Auditable Practices Checklist

Each item: **what to check** in the project → **the practice** it tests → **exam anchor** (task statement + typical distractor patterns, cited by their library names). Weights matter: D1 27%, D2 18%, D3 20%, D4 20%, D5 15% — but in a local project D3 and D5 usually have the most auditable surface, so don't force D1 findings that aren't there.

Not every project exposes every item. Skip absent surfaces explicitly (one line). A documented, deliberate deviation is ℹ️, not ⚠️.

---

## Domain 3 — Claude Code Configuration & Workflows (20%) — audit this first, richest surface

### 3.1 CLAUDE.md hierarchy, scoping, modularity
- Does a project CLAUDE.md exist? Is it lean and instructive (conventions, commands, quirks) rather than a dumping ground?
- Scoping discipline: personal preferences in `~/.claude/CLAUDE.md`, shared project conventions in the repo, subdirectory-specific conventions in directory-level CLAUDE.md files. Anything in the wrong layer is the classic exam trap: **Project = shared, User = personal**.
- If CLAUDE.md is long: are topic-specific chunks split into `.claude/rules/` or pulled in via `@import`, or is it monolithic?
- Exam anchor: TS 3.1. Distractors: Project = shared User = personal; Sledgehammer-on-thumbtack (moving everything to user level to fix one scoping problem).

### 3.2 Slash commands and skills
- Are repeated workflows captured as project-scoped commands/skills (`.claude/skills/`, `.claude/commands/`) instead of being re-prompted from memory each time?
- Skill quality: does frontmatter carry a triggering-rich `description`? Are `allowed-tools`, `argument-hint`, `context: fork` used where they earn their keep (restricting a risky skill's tools; isolating verbose output)?
- The judgment call the exam loves: **skill vs CLAUDE.md standard** — always-on conventions belong in CLAUDE.md/rules; on-demand workflows belong in skills. Flag content sitting on the wrong side.
- Exam anchor: TS 3.2. Distractors: Wrong-lever (a skill when a rule was needed, or vice versa); Fabricated-feature (frontmatter keys that don't exist).

### 3.3 Path-specific rules
- Conventions that only apply to a file subset (tests, migrations, one language in a mixed repo): are they in `.claude/rules/*.md` with `paths:` globs, or bloating global CLAUDE.md where they cost context on every session?
- Glob advantage worth teaching: path-scoped rules match patterns codebase-wide (e.g. `**/*.test.tsx`), which subdirectory CLAUDE.md files can't.
- Exam anchor: TS 3.3. Distractors: Sledgehammer-on-thumbtack (global rule for a local convention); Unix-workaround / wrong-layer.

### 3.4 Plan mode vs direct execution
- Do CLAUDE.md/docs/backlog give any guidance on which changes need design-first (specs, plan mode, "read the design doc before structural changes") vs direct execution? A repo that says "read the approved design before structural changes" is practicing TS 3.4.
- Exam anchor: TS 3.4. Distractors: Sledgehammer-on-thumbtack (plan mode for a one-line fix); Symptom-not-root-cause.

### 3.5 Iterative refinement
- Test-driven iteration: is there a fast regression gate a coding agent can run per change? Are concrete input/output examples documented (golden tests, fixtures, expected outputs)?
- Exam anchor: TS 3.5. Distractors: Still-prose (vague acceptance criteria instead of executable examples); Repeat-without-escape.

### 3.6 CI/CD integration
- If CI exists: could/does it run Claude non-interactively (`-p`, `--output-format json` / `--json-schema`)? Are testing standards documented so generated code meets them? Is review context isolated per run to avoid duplicate comments?
- If no CI: skip, but note the teaching hook (headless mode is exactly TS 3.6's surface).
- Exam anchor: TS 3.6. Distractors: Fabricated-feature (invented CLI flags); Prompt-when-deterministic-needed (prompting for what a CI check should enforce).

## Domain 1 — Agentic Architecture & Orchestration (27%) — audit the *dev workflow*, not just app code

In a project that isn't itself an agent system, D1 shows up in how the humans + Claude work the repo:

### 1.4 / 1.6 Decomposition and workflow enforcement
- Is work decomposed into ordered, verifiable steps (backlog with priorities, pipeline stages with explicit rebuild order, "gate" tests between stages)? A data pipeline whose steps must run in order, each producing a checked artifact, is a workflow-enforcement pattern worth citing.
- Exam anchor: TS 1.4, 1.6. Distractors: Repeat-without-escape (retry loops with no exit); Self-defeating goal.

### 1.5 Hooks for deterministic interception
- Are deterministic policies enforced by hooks/scripts (pre-commit, Claude Code hooks, CI gates) rather than by prompt instructions and hope? Anything in CLAUDE.md phrased as "never do X" that a hook could actually enforce is a finding.
- Exam anchor: TS 1.5. Distractors: **Prompt-when-deterministic-needed** (the single most auditable pattern in real repos); Soft self-restraint.

### 1.7 Session state, resumption, forking
- Can a fresh session resume the work? Handoff docs, state files, resumable long-running scripts (checkpointing), memory files. A pipeline script documented as "resumable" or a HANDOFF.md is TS 1.7 in the wild.
- Exam anchor: TS 1.7. Distractors: Recovery-without-communication; Silent reconciliation.

## Domain 2 — Tool Design & MCP Integration (18%)

### 2.1 / 2.2 Tool interfaces and structured errors (audit any APIs/wrappers the project defines)
- Do the project's own service wrappers (API clients, helper modules) have clear boundaries, docstrings that state contracts, and **structured, distinguishable errors**? "Busy upstream" vs "no results" vs "invalid input" must be separable by the caller — collapsing them is the exam's favorite D2 failure.
- Exam anchor: TS 2.1, 2.2. Distractors: Punt-to-user (surfacing raw errors to end users instead of handling); Recovery-without-communication.

### 2.3 / 2.4 Tool distribution and MCP configuration
- Are MCP servers scoped sensibly (project `.mcp.json` for repo-relevant servers, user-level for personal ones)? Are agents/skills given only the tools they need (`allowed-tools`)?
- Exam anchor: TS 2.3, 2.4. Distractors: Project = shared User = personal (again — MCP scope version); Sledgehammer-on-thumbtack.

### 2.5 Built-in tool selection
- Do project scripts/hooks/docs push the right primitive for the job (dedicated Read/Grep/Glob semantics vs shelling out; deterministic script vs asking the model)?
- Exam anchor: TS 2.5. Distractors: Unix-workaround / wrong-layer; Prompt-when-deterministic-needed.

## Domain 4 — Prompt Engineering & Structured Output (20%)

If the project makes no LLM calls, most of D4 audits the *data discipline* instead — the same principles pre-date LLMs:

### 4.1 Explicit criteria
- Are quality bars written as explicit, checkable criteria (spec docs with thresholds, test assertions with named bands) rather than vague prose ("should be good")?
- Exam anchor: TS 4.1. Distractors: Still-prose; LLM self-confidence poorly calibrated.

### 4.3 Schemas over prose
- Where the project produces structured data: are schemas explicit (typed columns, enums/slugs, nullable-by-design fields) and language/display concerns separated from data? Category slugs in storage + translation at display time is exactly the enum-plus-presentation split TS 4.3 rewards.
- Exam anchor: TS 4.3. Distractors: Still-prose (freeform strings where enums belong); Fabricated-feature.

### 4.4 Validation, retry, feedback loops
- Do pipelines/API calls validate outputs and retry with backoff — and crucially, **stop retrying** and escalate when retries can't fix the failure class? Unbounded or blind retries are the finding.
- Exam anchor: TS 4.4. Distractors: Paying-for-failure-repeatedly; Repeat-without-escape.

### 4.6 Independent review passes
- Is verification independent of generation (tests written against fixtures rather than against the code's own output; second-pass review of critical artifacts)?
- Exam anchor: TS 4.6. Distractors: LLM self-confidence poorly calibrated (self-review as sole gate).

## Domain 5 — Context Management & Reliability (15%) — small weight, but real projects shine here

### 5.3 Error propagation
- Are failures propagated with context and **visibly degraded**, never silently swallowed? "Fallback must always be visible to the user" is textbook TS 5.3. Distinguish access-failure from empty-result everywhere (an API timeout is not "no data").
- Exam anchor: TS 5.3. Distractors: Recovery-without-communication; Silent reconciliation; Cause-as-cure.

### 5.4 Context management in exploration
- Scratchpads, handoff docs, memory files, structured state that survives a crash or a context window — does the repo help a fresh session get productive without re-reading everything?
- Exam anchor: TS 5.4. Distractors: Sledgehammer-on-thumbtack (re-reading the whole codebase instead of targeted state).

### 5.5 Confidence calibration and review thresholds
- Are low-evidence outputs gated rather than presented with false confidence (minimum-evidence thresholds, "insufficient data" states, human-review triggers)? An evidence gate that refuses to score sparse data is TS 5.5 made concrete.
- Exam anchor: TS 5.5. Distractors: LLM self-confidence poorly calibrated; Baby-with-the-bathwater (discarding the whole output instead of gating the weak part).

### 5.6 Provenance and uncertainty
- Does data carry its lineage (source, method, precision, era columns; label_source-style provenance)? Is uncertainty preserved to the surface ("no data ≠ safe") instead of being flattened into a confident answer?
- Exam anchor: TS 5.6. Distractors: Silent reconciliation (merging conflicting sources without annotation); Symptom-not-root-cause.

### 5.2 Escalation and ambiguity
- When inputs are ambiguous (geocoding a vague address, overlapping identifiers), does the system clarify or surface options rather than guessing silently?
- Exam anchor: TS 5.2. Distractors: Punt-to-user (escalating everything) vs its mirror, never escalating; Soft self-restraint.

---

## Out-of-scope reflex (applies to every finding)

The official guide excludes fine-tuning, model training, and custom classifiers. Never recommend them in a finding's fix — and when a finding is adjacent (e.g., "classification quality"), say explicitly that the exam-correct lever is prompting/rules/schemas, and name **Fine-tune / train a custom model** as the distractor that option would be.
