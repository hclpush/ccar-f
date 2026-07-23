<!--
  Bundled from the cca-f-exam-prep study system.
  Copyright (c) 2026 Abi Odedeyi (CodeFreeIQ) - MIT License.
  See LICENSE-THIRD-PARTY at the repository root for the full license text.
-->

# Distractor Pattern Library (22 named patterns)

The CCA-F exam is scenario multiple-response questions (multiple choice, *select all that apply* — one or more options correct). The options that are *not* keyed correct are plausible distractors, and each falls into a recognizable pattern. Naming the pattern lets you eliminate distractors faster and more reliably than reasoning each scenario from scratch — and on a multi-select question you must judge every option independently, since knowing one answer no longer rules the others out.

Use this library as both a **teaching reference** and a **tagging tool** during quizzes. When a new pattern emerges in a scenario that isn't yet named, NAME it (short, evocative phrase), define the mechanism, give 1–2 examples, state the diagnostic "rule," and append it here.

The library is not closed. It grows with experience.

---

## 1. Project = shared, User = personal

One law, multiple files across Claude Code:

- `.mcp.json` (project) vs `~/.claude.json` (user)
- `.claude/CLAUDE.md` (project) vs `~/.claude/CLAUDE.md` (user)
- `.claude/commands/` (project) vs `~/.claude/commands/` (user)
- `.claude/skills/` (project) vs `~/.claude/skills/` (user)

**Rule:** anything in user-level config is personal to that user and does NOT reach teammates via version control. If a requirement says "every developer automatically," it must be project-level.

---

## 2. Fabricated-feature

Made-up configs, flags, or environment variables that don't actually exist.

Examples: `CLAUDE_HEADLESS=true`, a `--batch` flag, `.claude/config.json`, a `package.json` key called `claude.commands`, "enable multi-turn tool calling in batch config," "raise the batch processing window to 48 hours."

**Rule:** if you've never seen the option in the official docs, suspect it on sight. Fabricated features sound authoritative; that's the trap.

---

## 3. Sledgehammer-on-thumbtack

Heavy mechanism applied to a small problem.

Examples: "switch to a larger model" for a prompt-clarity issue; "train a fine-tuned classifier" when a description rewrite would do; "enter plan mode" for a one-line null check.

**Rule:** match the tool to the size of the job.

---

## 4. Wrong-lever (right feature, wrong problem)

A real feature applied to a problem it doesn't solve.

Examples: `tool_choice "any"` for parallelism (it controls whether/which tool, not concurrency); `< /dev/null` for CI interactivity; extended thinking for a self-review bias problem; bigger context window for an attention-dilution issue.

**Rule:** even real features can be wrong answers. Ask what mechanism the problem actually needs.

---

## 5. Symptom-not-root-cause

Post-processor, regex cleanup, or downstream patch instead of fixing the architectural source.

Examples: "regex to strip invented values from output"; "post-processor to insert citations after synthesis."

**Rule:** fix the source, not the cleanup.

---

## 6. Prompt-when-deterministic-needed

Soft prompt instructions for business-critical rules that require guaranteed compliance.

Examples: "Tell the agent to always verify the customer before processing a refund"; "Add 'always cite sources' to the prompt."

**Rule:** compliance requirements need hooks, prerequisite gates, or schema-level enforcement — not prose imperatives.

---

## 7. Unix-workaround / wrong-layer

A real shell trick applied to the wrong abstraction layer.

Example: `< /dev/null` to make Claude Code non-interactive (the right answer is the documented `-p` flag).

**Rule:** the trick is real; it's just not at the layer where the actual mechanism lives.

---

## 8. Soft self-restraint

"Be conservative," "be careful," "only report high-confidence findings."

These are probabilistic self-instructions that don't improve precision. The model has no calibrated sense of "conservative."

**Rule:** replace with explicit categorical criteria.

---

## 9. Punt-to-user

"Ask the user every time," "request clarification before every tool call," "escalate every X."

Pushes ambiguity cost onto the user instead of building the agent's capability.

**Rule:** build the agent's competence; don't outsource every uncertain decision to the human.

---

## 10. Still-prose

More words to a problem that words couldn't fix.

Examples: "Make the prompt more explicit with bold formatting"; "Add stronger imperatives like 'ALWAYS verify.'"

**Rule:** if prose failed once, more prose isn't the fix. Reach for examples, schemas, hooks, or structured mechanisms.

---

## 11. Paying-for-failure-repeatedly

More retries on a structurally unfixable problem.

Example: "Increase retry count from 3 to 10" when the missing info isn't in the source document at all.

**Rule:** if a retry can't fix it, more retries can't either.

---

## 12. Repeat-without-escape

More reps inside the same contaminated context.

Examples: "Re-read the generated code three times in the same session"; "have the generator session review its own work harder."

**Rule:** structural ceilings aren't lifted by more reps within the same structure.

---

## 13. Cause-as-cure

Applying more of the harmful mechanism as the proposed "fix."

Example: "Summarize earlier conversation more often" to fix problems caused by progressive summarization.

**Rule:** if your fix is doing more of the disease, it's not a fix.

---

## 14. Baby-with-the-bathwater

Destroying valuable state or progress to fix a localized problem.

Example: "Restart the session every hour to refresh context."

**Rule:** preserve progress; fix the targeted issue precisely.

---

## 15. Recovery-without-communication

A subagent recovers locally from a failure but communicates only a generic status back to the coordinator, stripping all the context the coordinator would need to decide what to do next.

**Rule:** pair local recovery with structured error context propagation upward.

---

## 16. Self-defeating goal

The proposed action defeats the very thing being attempted.

Example: "Review 100% of extractions to confirm 97% accuracy" — defeats the goal of reducing review.

**Rule:** check that the proposed mechanism still achieves the stated goal.

---

## 17. Silent reconciliation

An agent receives conflicting inputs and silently picks one, hiding the disagreement.

Examples: "always pick the most recent source"; "use the first match when there's ambiguity."

**Rule:** surface and annotate conflicts; never silently pick.

---

## 18. LLM self-confidence poorly calibrated

"Have the agent self-report a confidence score (1–10) and gate decisions on it."

Self-confidence is poorly calibrated, especially for escalation decisions. The model is often most confident on cases where it's wrong.

**Exception:** OK when confidence is calibrated against labeled validation data AND there's a human safety net downstream (e.g., routing review attention). NOT OK for raw gating.

---

## 19. Fine-tune / train a custom model

"Train a fine-tuned classifier," "switch to a fine-tuned model," "train on past data to predict."

Out-of-scope per the official CCA-F guide.

**Rule:** instant rule-out reflex on the exam. Any answer suggesting fine-tuning or custom-model training is almost certainly wrong.

---

## 20. Model-as-oracle

Asking the model to output a fact that only the runtime system can know — SQL query execution cost, current inventory levels, live latency, actual row counts.

Example: a schema with an `estimated_cost` field the model is supposed to fill for a generated SQL query. Claude cannot know table sizes, index availability, or data distribution; the correct design has the tool run the database engine's `EXPLAIN` and return the engine's own estimate.

**Rule:** if the number depends on runtime state, the model must ask the system, not guess. Look for the option where a tool fetches ground truth.

---

## 21. Override-the-human

Options that delay, "improve on," or second-guess an explicit human request — usually dressed up as diligence.

Examples: "Escalate, but first run diagnostic checks to give the human agent context"; "attempt the remaining troubleshooting steps since the agent may still resolve the issue" — when the customer has explicitly asked for a human.

**Rule:** an explicit request for a human is a hard escalation trigger; act on it immediately. This is the mirror image of #9 Punt-to-user: #9 over-asks the human, #21 under-obeys them.

---

## 22. Right-hook-wrong-phase (timing inversion)

A real interception mechanism chosen at the wrong point in the lifecycle. The feature exists and is even the right *kind* of tool — it just fires at a moment where it cannot do the job.

Examples: `PostToolUse` when you needed `PreToolUse` to **block** a non-compliant edit before it lands (PostToolUse only sees it afterward); a `PreCompact` hook to inject context when you needed `pause_after_compaction: true` (inject *before* compaction and your content gets summarized away).

**Rule:** to prevent or gate an action, or to inject context that must survive a later step, the hook must fire **BEFORE** the operation it governs. "Post-"/"after-" hooks observe and react — they can never prevent — and anything a "pre-" hook hands forward can be overwritten by the very step you were trying to influence.

Distinct from #4 Wrong-lever (right feature, wrong *problem*) — here it's the right feature at the wrong *time*.

---

## How to extend

When a new pattern emerges during a scenario:

1. **Name it** — short, evocative phrase (e.g., "cause-as-cure," "recovery-without-communication")
2. **Define the mechanism** + give 1–2 concrete examples
3. **State the diagnostic rule** — the instinct it lets you apply
4. **Add it to this file** with the next number

Treat the pattern library as a living artifact. The richer it gets, the faster you eliminate distractors.