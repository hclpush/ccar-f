# Study Method — Per-Concept 3-Step Loop

This is a simplified version of the cca-f-exam-prep 4-step loop. **There is no 4th "Notes" step and no spaced-rep logging** — this skill unblocks a concept in the moment; retention lives in cca-f-exam-prep.

Run these three steps in order for every concept. Do not let the learner skip the Feynman or Quiz step — "yeah I get it" is exactly the false confidence this skill exists to catch.

## Step 1: Explain

A good explanation for a non-technical learner includes:

- **A plain-language definition** in the learner's frame — no jargon they haven't met yet. If the definition needs another technical term, check `concept-list.md`: that term is probably the Prerequisite, and you may need to teach it first.
- **One real-world analogy** that anchors the underlying mechanism (e.g., environment variable → "a sticky note the program reads at startup instead of you typing the value every time"; glob → "a wildcard search like `*.jpg` in your photos folder").
- **One sentence on why this appears in the CCAR-F exam** — cite the concept's **CCAR-F anchor** from `concept-list.md`. This is what makes the teaching exam-relevant rather than generic CS. Example: "You need this because D3 path-scoped rules use glob patterns in the `paths:` frontmatter to decide which files a rule applies to."

Keep it tight. This is unblocking, not a lecture. One definition, one analogy, one exam anchor — then move to recall.

## Step 2: Feynman (open recall)

Ask ONE open-recall question: **"Explain it back to me in your own words, without looking."**

- No options, no looking up.
- After they answer, be honest: state plainly what they nailed and what they missed.
- Fill in missing pieces precisely, using the correct names for things.
- The learner does NOT get to claim "I had that, I just didn't say it." If they didn't say it, they didn't have it. That's the point of open recall.

## Step 3: Quiz (CCAR-F-scenario multiple-response)

Pose ONE **multiple-response** question — 4 options, *select all that apply*. The CCAR-F exam is multiple-choice with **one or more correct answers**, so build the quiz the same way: some questions have a single correct option, others two or three. Never reveal how many are correct — the learner must evaluate each option on its own merits. Make sure every unkeyed option is a genuinely plausible wrong answer, and every keyed option is genuinely right.

**The one non-negotiable rule of this skill:** every quiz question must use a **CCAR-F exam scenario as the wrapper**, not an abstract textbook example. The concept is the same; the clothing must be exam-shaped.

- Teach **caching** through a prompt-caching cost question ("a support agent re-sends a 4,000-token system prompt on every turn; what reduces cost?") — NOT through a generic web-browser cache analogy.
- Teach **JSON Schema** through a structured-extraction scenario ("the extraction pipeline must guarantee every record has a numeric `invoice_total`; what enforces that?") — NOT through a generic "describe a shape" example.
- Teach **glob** through a `.claude/rules/` path-matching scenario — NOT through a generic file-search example.

Why this matters: the learner's goal is to read CCAR-F exam sentences fluently. If the practice never wears exam clothing, the concept stays abstract and doesn't transfer to the moment they actually need it. The six CCAR-F scenarios — Customer Support Resolution Agent, Code Generation with Claude Code, Multi-Agent Research System, Developer Productivity, Claude Code for CI, Structured Data Extraction — are your costume rack. Pick whichever fits the concept.

Other rules:

- **Vary the number and position of the correct answers** across concepts. Don't always make it a single answer, and don't always make A correct — mix one-answer and multi-answer questions, and distribute the keyed options across A/B/C/D. To count as correct, the learner must select *every* right option and *no* wrong ones — partial selections are wrong, same as on the real exam.
- Keep distractors plausible but do NOT tag them with named patterns. Pattern-tagging is a cca-f-exam-prep activity; here it adds load without payoff.
- After the quiz: correct → confirm in one line, offer the next concept (or hand back to cca-f-exam-prep). Wrong → say precisely what's off, re-teach with a *different* analogy, re-quiz with a fresh scenario.

## Pacing

If the learner is breezing through, keep the pace but keep the rigor — still run all three steps. If they're stuck, slow down, re-teach with a new analogy, and don't advance until they can clear the Feynman step. Catching the gap is the value.
