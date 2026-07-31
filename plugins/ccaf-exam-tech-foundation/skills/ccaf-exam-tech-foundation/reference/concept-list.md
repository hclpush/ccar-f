# CCA-F Foundation Concepts — 32 concepts in 7 categories

The foundational computer-science concepts a non-technical learner needs to read CCA-F exam material fluently. Each is taught only as far as the exam needs it.

Each concept has:
- **CCA-F anchor** — the specific exam domain/topic/scenario where the concept appears.
- **Prerequisite** — what to understand first (— means none; it's an entry point).

Domains (with official exam weights): **D1** Agentic Architecture & Orchestration (27%) · **D2** Tool Design & MCP Integration (18%) · **D3** Claude Code Configuration & Workflows (20%) · **D4** Prompt Engineering & Structured Output (20%) · **D5** Context Management & Reliability (15%).
Scenarios (exam draws 4 of these 6 at random): Customer Support Resolution Agent · Code Generation with Claude Code · Multi-Agent Research System · Developer Productivity with Claude · Claude Code for Continuous Integration · Structured Data Extraction.

> Domains, weights, and scenario names verified against the **official Anthropic CCA-F Certification Exam Guide** (v0.1) on 2026-07-01 — the authoritative source. Get the current PDF from Anthropic's certification page; direct link at time of writing: https://everpath-course-content.s3-accelerate.amazonaws.com/instructor%2F8lsy243ftffjjy1cx9lm3o2bw%2Fpublic%2F1773274827%2FClaude+Certified+Architect+%E2%80%93+Foundations+Certification+Exam+Guide.pdf . Claude-specific flags/paths cross-checked against docs.claude.com. Re-verify against the latest guide version before relying on any Claude-specific anchor.

> Scope note: Claude-specific concepts (prompt/system prompt, context window, tokens, streaming, CLI usage) are covered by the main cca-f-exam-prep material, not this foundation layer.

---

## Category 1 — Programming basics

1. **Loop**
   - CCA-F anchor: D1 agent loop — the model acts, observes, repeats until a stop condition.
   - Prerequisite: —

2. **Function**
   - CCA-F anchor: D2 tool design — a tool is fundamentally a function the model can call.
   - Prerequisite: —

3. **Decorator**
   - CCA-F anchor: D2 — the SDK pattern that registers a function as a callable tool.
   - Prerequisite: Function

4. **Class**
   - CCA-F anchor: D2 — SDK objects (clients, tool definitions) are instances of classes.
   - Prerequisite: Function

5. **Variable & data types (string, integer, boolean, null)**
   - CCA-F anchor: D4 structured output — schema fields declare types; a distractor often swaps a number for a string.
   - Prerequisite: —

6. **Exception / error handling**
   - CCA-F anchor: D5 reliability — retries, fallbacks, handling a failed tool call gracefully.
   - Prerequisite: Function

7. **Callback**
   - CCA-F anchor: D1/D2 — passing a function to be invoked later (e.g. tool execution hooks).
   - Prerequisite: Function

8. **Wrapper**
   - CCA-F anchor: D2 — an MCP tool typically wraps an existing API or function, exposing a simpler, model-friendly interface around it.
   - Prerequisite: Function

9. **Package / module**
   - CCA-F anchor: D2/D3 — SDKs and MCP servers are installed as packages (`pip install anthropic`, `npm install`); code is organized into importable modules.
   - Prerequisite: Function

10. **TypeScript**
    - CCA-F anchor: D2 — the Claude Agent SDK ships in TypeScript and Python; typed tool definitions catch shape mistakes before runtime.
    - Prerequisite: Variable & data types

---

## Category 2 — Data formats

11. **JSON**
    - CCA-F anchor: D4 structured output; Structured Data Extraction — model responses as JSON objects.
    - Prerequisite: Variable & data types

12. **JSON Schema**
    - CCA-F anchor: D4 output validation; D3 Claude Code `--output-format json --json-schema '<schema>'` in CI (the two flags are used together) — enforcing the shape of extracted data.
    - Prerequisite: JSON

13. **Enum**
    - CCA-F anchor: D4 — constraining a field to a fixed set of allowed values (e.g. a category label).
    - Prerequisite: JSON Schema

14. **Nullable type**
    - CCA-F anchor: D4 — a field that may be a value or null; common in extraction schemas for missing data.
    - Prerequisite: Variable & data types, JSON Schema

15. **YAML (config files)**
    - CCA-F anchor: D3 — skill/agent/rule frontmatter and config files are written in YAML.
    - Prerequisite: JSON

16. **Pydantic**
    - CCA-F anchor: D4 structured output — the Python library that defines a schema as a class and validates model output against it; the SDK's structured-output path.
    - Prerequisite: Class, JSON Schema

---

## Category 3 — APIs & networking

17. **REST API**
    - CCA-F anchor: D1/D2 — the Claude API and MCP-exposed services follow REST conventions.
    - Prerequisite: —

18. **API key / secrets via environment variables**
    - CCA-F anchor: D2/D3 — env-var expansion in `.mcp.json` and passing secrets to CI headless runs via env vars rather than hardcoding (Task 2.4 MCP config; Task 3.6 CI). *Note: API authentication/billing itself is out of scope — the in-scope skill is env-var secret handling in config, not auth flows.*
    - Prerequisite: REST API, Environment variable

19. **Endpoint**
    - CCA-F anchor: D2 — the specific URL an API or MCP server exposes for a capability.
    - Prerequisite: REST API

---

## Category 4 — CLI & environment

20. **Environment variable**
    - CCA-F anchor: D3 — CI/CD headless mode, API keys passed via env vars rather than hardcoded.
    - Prerequisite: —

21. **stdin / stdout**
    - CCA-F anchor: D3 — Claude Code in CI reads a prompt from stdin and prints results to stdout non-interactively.
    - Prerequisite: —

---

## Category 5 — Infrastructure

22. **Docker**
    - CCA-F anchor: D3 — reproducible environments for running Claude Code in CI.
    - Prerequisite: —

23. **Container**
    - CCA-F anchor: D3 — the isolated unit Docker runs; where a CI agent executes.
    - Prerequisite: Docker

24. **CI/CD pipeline**
    - CCA-F anchor: D3 — Claude Code for Continuous Integration; automated runs triggered on commits/PRs.
    - Prerequisite: Container

25. **Kubernetes**
    - CCA-F anchor: D3 — orchestrates many containers at scale; where production CI/agent workloads commonly run.
    - Prerequisite: Container

26. **Terraform**
    - CCA-F anchor: D3 — infrastructure-as-code; declaring CI/agent infrastructure in config files so environments are reproducible.
    - Prerequisite: CI/CD pipeline

---

## Category 6 — Software architecture concepts

27. **Caching**
    - CCA-F anchor: D5/D4 — prompt caching reuses a stable prefix to cut cost and latency.
    - Prerequisite: —

28. **Microservices**
    - CCA-F anchor: D1/D2 — an architecture of small, independently deployed services; MCP servers expose capabilities as separate services an agent composes.
    - Prerequisite: REST API

29. **Exponential backoff**
    - CCA-F anchor: D5 reliability — retrying a failed call with an increasing wait between attempts, so transient failures recover without hammering the service.
    - Prerequisite: Exception / error handling

---

## Category 7 — Dev workflow

30. **Git basics (repo, branch, commit)**
    - CCA-F anchor: D3 — Code Generation with Claude Code; agents operate on branches and record changes as commits.
    - Prerequisite: —

31. **Pull request (PR)**
    - CCA-F anchor: D3 — the review gate: an agent proposes changes on a branch and opens a PR for human review before merge.
    - Prerequisite: Git basics (repo, branch, commit)

32. **Glob pattern (file path matching like `**/*.ts`)**
    - CCA-F anchor: D3 — `.claude/rules/` path-scoped rules use globs in `paths:` frontmatter.
    - Prerequisite: —
