# ccarf-practice-audit

A Claude Code plugin for people preparing the **CCAR-F exam (Claude Certified Architect – Foundations)** who are also building real projects. It audits your project against the practices the exam tests, and explains every finding as study material: which task statement it maps to, how a scenario question would frame it, and which named distractor patterns the wrong answers would use.

Instead of studying the exam guide in one tab and coding in another, your own codebase becomes the flashcard deck — except there's deliberately **no quizzing**: findings are explanation-only, and shaky topics are handed to you as "suggested next study topics" for whatever active-recall routine you use.

## What's in the box

| Piece | What it does |
|---|---|
| `skills/ccarf-practice-audit/` | The audit skill: workflow, report format, exam-lens quality bar |
| `skills/.../reference/audit-checklist.md` | Auditable practices for all five exam domains (D1–D5), mapped to the 30 task statements |
| `skills/.../reference/distractor-pattern-library.md` | 21 named distractor patterns, by [Abi Odedeyi (CodeFreeIQ)](LICENSE-THIRD-PARTY), MIT |
| `scripts/check_change.py` | The change detector that powers the auto-trigger |
| `hooks/hooks.json` | Wires the detector as a Stop hook |

The **official exam guide is not bundled** (it's Anthropic's copyrighted material). Download it yourself: [CCAR-F Exam Guide (PDF)](https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2F6nizmqk8tpzpfjvt6qmmav7rh%2Fpublic%2F1783542750%2FClaude+Certified+Architect+%E2%80%93+Foundations+Exam+Guide.pdf) — and optionally point the skill at your local copy (see Configuration).

## ⚠️ Read this before installing — what the hook does

**This plugin installs a Stop hook.** After Claude finishes a response in an opted-in git repository, the hook compares the working tree against the last-audited commit, and when change crosses the thresholds it **blocks the stop and instructs Claude to run the audit right then**, in-session. That interruption is the product — but you should know you're buying it.

- **Opt-in per repository.** The hook is silent everywhere until a repo contains an **untracked** `.claude/ccarf-audit-state.json` — which is created the first time you run the audit skill in that repo. Installing the plugin alone changes nothing, and a state file committed inside a repo you clone is ignored — a third-party repo can't opt you in.
- **Thresholds:** ≥5 changed files, or ≥150 changed lines, or any structural change (added/deleted/renamed/untracked files, `CLAUDE.md` / `.claude/` edits) since the last audit.
- **Debounce:** at most one auto-trigger per 4 hours, recorded *before* the audit runs so a declined audit doesn't nag every turn.
- **State it writes:** only `.claude/ccarf-audit-state.json` in the repo (baseline commit, timestamps, report path). Reports go to `.claude/ccarf-audit/reports/` by default. Both are per-machine state — gitignore them.
- **To change thresholds:** edit the constants at the top of `scripts/check_change.py`. To silence a repo: delete its state file. To uninstall: remove the plugin.

### Security posture

- The hook script parses stdin and git output **as data only**: git is invoked with argument lists (never a shell string), there is no `eval`/`exec`, no network access, and any failure defaults to "allow stop" rather than breaking your session.
- Everything repo-derived that enters the hook's message to Claude (file names, the baseline sha) is **sanitized first** — allowlisted path characters only (`A-Za-z0-9._/ -`), length-capped, sha format-validated — because file names in a cloned repo are attacker-controlled and the message is injected into the model's instructions. The message also explicitly marks file names as data, not instructions.
- A **git-tracked state file is never honored** — by the hook (no auto-trigger) or the skill (its `report_dir` / `exam_guide_path` are ignored). Tracked means repo-shipped, and repo-shipped paths could redirect what Claude reads as the exam guide or where it writes reports. The skill also confirms with you before following either path outside the repo or your notes.
- Residual risk to be honest about: any Stop hook that injects instructions is a trust decision. The script is ~180 lines, dependency-free (Python 3 stdlib), and short enough to read before you install — please do.

## Install

As a plugin from the [ccar-f marketplace](../../README.md) (recommended — skill + hook in one step):

```
/plugin marketplace add hclpush/ccar-f
/plugin install ccarf-practice-audit@ccar-f
```

Manual alternative (no plugin system): copy `skills/ccarf-practice-audit/` into `~/.claude/skills/`, copy `scripts/check_change.py` somewhere stable, and add to a project's `.claude/settings.json`:

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"/path/to/check_change.py\"",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

## Usage

1. In a project you want audited, say: **"run a ccaf audit"** (or invoke `/ccarf-practice-audit`). The first run is a full audit and opts the repo in to auto-triggering.
2. Keep working. When you've changed enough, the hook fires and Claude audits incrementally — changed areas plus the Claude Code config surface.
3. Read the report: scorecard by domain, findings with **Observed / Why it matters / Exam lens**, and 2–3 suggested study topics.

## Configuration

Optional keys in `.claude/ccarf-audit-state.json` (per repo):

```json
{
  "report_dir": "/path/to/your/notes/ccarf/audits",
  "exam_guide_path": "/path/to/exam-guide.md"
}
```

- `report_dir` — send reports to your notes system (e.g. an Obsidian vault) instead of `.claude/ccarf-audit/reports/`.
- `exam_guide_path` — a local copy (PDF-to-markdown works well) of the official guide; the skill treats it as the source of truth for task-statement wording.

## Credits

- **Distractor-pattern library:** Abi Odedeyi (CodeFreeIQ) — the diagnostic core of the exam lenses, bundled under MIT with attribution ([LICENSE-THIRD-PARTY](LICENSE-THIRD-PARTY)). Her full study system (Explain → Feynman → Quiz → Notes with spaced repetition) is the companion to this plugin: this audits and explains; hers drills and retains.
- Not affiliated with or endorsed by Anthropic. CCAR-F task-statement references are paraphrased; always defer to the official exam guide.

## License

MIT — see [LICENSE](LICENSE).
