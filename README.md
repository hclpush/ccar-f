# cca-f

Claude Code plugins for studying the **CCA-F exam (Claude Certified Architect – Foundations)** — built for people preparing the certification while shipping real projects.

> **Disclaimer — unofficial.** These are community study aids. They are **not affiliated with, endorsed by, or produced by Anthropic**. "CCA-F," "Claude Certified Architect," and "Claude" are referenced for study purposes only. Anthropic's official exam guide is the source of truth.

## The idea

Studying a certification in one tab while coding in another wastes the best study material you have: your own project. These plugins connect the two —

- your codebase becomes worked examples of what the exam tests,
- the concepts you're missing get taught exactly where the exam uses them,
- and shaky topics get handed to whatever active-recall routine you use.

## Plugins

| Plugin | What it does | Docs |
|---|---|---|
| **ccaf-practice-audit** | Audits your project against the practices the exam tests; every finding explained as study material (task statement, scenario framing, named distractor patterns). Ships an **opt-in** Stop hook that re-audits automatically on significant repo change. | [README](plugins/ccaf-practice-audit/README.md) |
| **ccaf-exam-tech-foundation** | The prerequisite layer: teaches the ~38 foundational CS concepts (JSON, glob, env vars, caching, MCP…) a non-technical learner needs, each anchored to its exam topic, via a 3-step Explain → Feynman → Quiz loop. | [README](plugins/ccaf-exam-tech-foundation/README.md) |
| **ccaf-mock-exam-analysis** | The backward loop: turns a completed mock exam into a targeted study plan — per-miss domain + root-cause classification, distractor-pattern tagging, trend comparison across mocks, and a days-until-exam drill plan. | [README](plugins/ccaf-mock-exam-analysis/README.md) |

Install only what you want — plugins are independent.

> ⚠️ **ccaf-practice-audit installs a Stop hook** (silent until you opt a repo in). Read [its README](plugins/ccaf-practice-audit/README.md) — especially the "what the hook does" and security-posture sections — before installing.

## Install

```
/plugin marketplace add hclpush/cca-f
/plugin install ccaf-practice-audit@cca-f
/plugin install ccaf-exam-tech-foundation@cca-f
/plugin install ccaf-mock-exam-analysis@cca-f
```

Each plugin's README also documents a manual, no-plugin-system install.

## Source of truth

Task statements, exam domains, weights, and scenario names are grounded in the **official Anthropic CCA-F Certification Exam Guide**. This repo does **not** redistribute the guide — download the current version from Anthropic's certification page.

Direct PDF link at time of writing (may rotate — prefer the certification page):
<https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2F8lsy243ftffjjy1cx9lm3o2bw%2Fpublic%2F1773274827%2FClaude+Certified+Architect+%E2%80%93+Foundations+Certification+Exam+Guide.pdf>

Claude-specific facts (CLI flags, config paths) are cross-checked against <https://docs.claude.com>.

## Repo layout

```
cca-f/
├── .claude-plugin/marketplace.json    # lists every plugin below
├── .githooks/                         # pre-commit guard for mirrored files
├── plugins/
│   ├── ccaf-practice-audit/           # audit skill + Stop hook + change detector
│   ├── ccaf-exam-tech-foundation/     # concept-teaching skill
│   └── ccaf-mock-exam-analysis/       # mock-exam gap analysis + drill planning
├── LICENSE
└── README.md
```

Plugins are self-contained by design (installing one copies only its folder), so shared ideas — e.g. the distractor-pattern library — live inside the plugin that uses them rather than in a common folder.

## Development

Self-containment means a few files are deliberately duplicated across plugins and must stay byte-identical — the distractor-pattern library above all, since both skills cite its pattern numbers. A symlink would break standalone installs, so a pre-commit hook enforces it instead.

Enable it once per clone:

```bash
git config core.hooksPath .githooks
```

Then edit whichever copy you like. On commit, the hook propagates your change to the siblings and stages them. If two copies were edited differently, it aborts and asks you to resolve. Groups are declared in [.githooks/mirror-groups.txt](.githooks/mirror-groups.txt) — add a block there to mirror a new file.

Note that files which merely look duplicated are *not* mirrored: each plugin's `LICENSE-THIRD-PARTY` names its own plugin's path and is meant to differ.

## Credits

- **Distractor-pattern library** (bundled in ccaf-practice-audit and ccaf-mock-exam-analysis — plugins are self-contained, so each carries its own copy): Abi Odedeyi (CodeFreeIQ), MIT with attribution — see [LICENSE-THIRD-PARTY](plugins/ccaf-practice-audit/LICENSE-THIRD-PARTY). Her full study system (Explain → Feynman → Quiz → Notes with spaced repetition) is the companion to these plugins.
- CCA-F task-statement references are paraphrased; always defer to the official exam guide.

## License

MIT — see [LICENSE](LICENSE). Third-party content: [LICENSE-THIRD-PARTY](plugins/ccaf-practice-audit/LICENSE-THIRD-PARTY).
