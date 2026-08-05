# ccarf-exam-tech-foundation

Teaches the ~32 foundational computer-science concepts a non-technical learner needs to read CCAR-F exam material fluently — JSON, JSON Schema, glob patterns, environment variables, caching, Docker, Kubernetes, and more. Each concept is anchored to the specific exam topic where it appears, and taught only as far as the exam needs it.

It runs a 3-step loop per concept:

1. **Explain** — plain-language definition + real-world analogy + one sentence on why it appears in the CCAR-F exam.
2. **Feynman** — one open-recall question.
3. **Quiz** — one multiple-response question (*select all that apply*, one or more correct — matching the real CCAR-F format) wrapped in a CCAR-F-style scenario (never an abstract textbook example).

This is a **prerequisite layer** beneath the `cca-f-exam-prep` study system — meant to unblock a single concept in the moment, then hand the learner back to exam prep. No spaced-rep logging, no distractor-pattern tagging (those belong to exam prep).

```
skills/ccarf-exam-tech-foundation/
├── SKILL.md
└── reference/
    ├── concept-list.md   # 32 concepts × 7 categories; CCAR-F anchor + prerequisite each
    └── study-method.md   # the 3-step Explain → Feynman → Quiz loop
```

## Install

As part of the ccar-f marketplace (recommended — see the [repo README](../../README.md)):

```
/plugin marketplace add hclpush/ccar-f
/plugin install ccarf-exam-tech-foundation@ccar-f
```

Manual alternative — copy the skill folder into your Claude Code skills directory:

```bash
cp -R skills/ccarf-exam-tech-foundation ~/.claude/skills/
```

## Contributing

The exam and Claude Code both change. Before editing any concept anchor:

- **Verify Claude-specific claims** (CLI flags, config-file paths, MCP behavior) against docs.claude.com — never assert them from memory.
- **Verify exam structure** (domains, weights, scenarios, in/out-of-scope topics) against the *current* official exam guide, not third-party prep sites.
- **Note the source and date** of any change so the next contributor knows what was checked.

One concept — **Caching** — is anchored to a topic the guide lists as *out of scope* (prompt caching). It's kept intentionally as background (teach the concept, flag "not tested"). Don't remove it as an error. Claude-specific concepts (tokens, context window, prompt/system prompt, streaming, CLI usage) were moved out of this foundation layer — they belong to the main `cca-f-exam-prep` material.
