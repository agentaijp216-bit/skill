---
name: how
description: Use when asked how a subsystem, feature, flow, or package works; for code walkthroughs before changes; and for ownership, placement, or layering questions. Focuses on architecture, runtime flow, and a useful onboarding mental model.
disable-model-invocation: true
---

# How

Explore the current repository and explain how the requested part works. Give enough detail for someone to build a reliable mental model and make a change safely, without annotating every line of code.

This is adapted from Lauren Tan's pstack `how` skill for Codex. Work directly in the current task; do not depend on Cursor commands, model-routing rules, or unavailable subagent tools.

## Workflow

1. **Set the scope.** If the request is ambiguous, state the part of the system you will trace and proceed with the best supported interpretation.
2. **Find the entry point.** Read the relevant docs, tests, routes, commands, and source files. Use repository search to follow references and callers.
3. **Trace the behavior.** Follow the main data and control flow across files or services. Note important boundaries, state changes, persistence, external calls, and error paths.
4. **Check evidence.** Distinguish what the code proves from what is inferred. Use existing tests or docs as evidence; do not run tests unless the user asks.
5. **Explain the design.** Describe the system from the user's question outward. Include only details needed to understand behavior, ownership, or placement.

## Output format

Use the sections that help answer the question; omit sections that do not apply:

- **Overview** — short answer and scope.
- **Key concepts** — the few important components or terms.
- **How it works** — step-by-step flow, including relevant data and state changes.
- **Where things live** — important files and their responsibilities.
- **Gotchas** — constraints, edge cases, or uncertainties supported by evidence.

Link repository files with paths and line numbers when available. Clearly label inferences and mention when the repository does not establish an answer.
