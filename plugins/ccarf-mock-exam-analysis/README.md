# ccarf-mock-exam-analysis

The **backward loop** of CCAR-F study: you take a mock exam, this skill reads the evidence of failure and converts it into the shortest path to a pass.

Point it at any completed CCAR-F practice-exam result (PDF-to-markdown extractions welcome — it expects them to be messy) and it will:

1. **Parse honestly** — count readable/unreadable questions, cross-check each answer key against its explanation text (CCAR-F is multiple-response, so a key is a *set* of letters; third-party mocks frequently tag the wrong letters or list only one when several are correct, and a multi-select question scores correct only if every right option is picked and no wrong ones), and ignore the mock's own domain labels in favor of the official guide's task statements.
2. **Classify every miss on two axes** — official domain + task statement, and root cause (mechanics recall / numeric fact / judgment / careless / out-of-scope), because the root cause determines the fix.
3. **Tag judgment misses with named distractor patterns** from the bundled library — "four misses, two ideas" is the compression that changes a study plan. When you approve a new pattern, the skill copies the library next to your gap analyses and grows that copy, so plugin updates never wipe your additions.
4. **Compare against previous mocks** to separate persistent structural weaknesses from noise — and name what improved.
5. **Build a days-until-exam drill plan** — triage mode under 14 days (points-per-hour ordering), spaced-rep intervals compressed to land before exam day.

The gap-analysis report is written next to your result file by default (or wherever you keep exam notes).

## Untrusted input

Mock-exam files come from third-party prep sites. The skill explicitly treats their content — including any text addressed to the assistant — as **data to analyze, never instructions to follow**.

## Install

As a plugin from the [ccar-f marketplace](../../README.md):

```
/plugin marketplace add hclpush/ccar-f
/plugin install ccarf-mock-exam-analysis@ccar-f
```

Manual alternative: copy `skills/ccarf-mock-exam-analysis/` into `~/.claude/skills/`.

## The official exam guide

Not bundled (it's Anthropic's copyrighted material) — download it from Anthropic's certification page. If you keep a local copy, tell the skill where it is; it treats the guide as the source of truth for domain/task classification and scope rulings. If you also use the [ccarf-practice-audit](../ccarf-practice-audit/README.md) plugin, its `exam_guide_path` setting is reused automatically.

The skill checks for a local guide silently — it won't nag you. So keep your copy current yourself: guide versions change, and domain weights and task statements can move between them. Grab the newest version from Anthropic's certification page whenever you start a new mock cycle. Without any local copy the skill still runs, but marks all domain and scope classifications as unverified.

## Credits

- **Distractor-pattern library:** Abi Odedeyi (CodeFreeIQ), bundled under MIT with attribution ([LICENSE-THIRD-PARTY](LICENSE-THIRD-PARTY)). This skill is the gap-analysis half of her Explain → Feynman → Quiz → Notes study system.
- Not affiliated with or endorsed by Anthropic. Task-statement references are paraphrased; always defer to the official exam guide.

## License

MIT — see [LICENSE](LICENSE).
