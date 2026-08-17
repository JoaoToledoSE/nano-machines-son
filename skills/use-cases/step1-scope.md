# Step 1 — Scope the Flow

Pin down exactly what is being documented before any exploration.

---

## 1. Extract from `$ARGUMENTS`

- **The flow**: one sentence, actor-first ("a supporter donates by credit card on a PAC Action Center"). A flow ends where the user **sees the result**, not where the triggering action returns.
- **Outcome surfaces**: part of the flow's definition, not downstream extras — enumerate now every surface where the flow's result becomes visible or usable to a user. The product's own UI copy and the PRD's promise language are the checklist ("each X is recorded in A and appears in B" puts A and B in scope).
- **Variants to cover**: each becomes its own use case (e.g. SSO vs non-SSO login, eligible vs blocked).
- **Assumptions**: things to take as given, excluded from the use cases (e.g. "the donation form is already configured"). These still appear as one-line preconditions, just not as steps.
- **Audience**: both are always served, no mode to choose — the in-repo MD carries the engineering trace (code symbols, citations); the Confluence page is plain-language for the whole team (code symbols banned there, Step 4 §3).

If the flow or variants are ambiguous, ask **one** clarifying question now — not during synthesis.

## 2. Orient in the codebase

- Check the ref: resolve the default branch at runtime (`gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`), fetch, and compare `HEAD` against it — a bare `git status` names the current branch but establishes neither the default branch nor its freshness. If not on the up-to-date default branch, tell the user which ref is being documented.
- Read the area's `STEERING.md` (and `CLAUDE.md`/`ADR.md` if present) for every app the flow touches. The steering doc usually names the exact subdirectories, external systems, and gating fields — this makes the Step 2/3 agent prompts dramatically better.
- Note which parts of the flow live **outside this repo** (external systems, hosted iframes, third-party IdPs). These will be marked "external — illustrative/unverified" throughout; never present their internals as fact.

## 3. Set up persistence

```bash
mkdir -p reports/use-cases-<slug>-$(date +%F)
```

Write `step1-scope.md` in that directory: flow, actors, outcome surfaces, variants, assumptions, areas/apps involved, external systems. Every later step appends its findings to this directory before moving on (context-reset safety).

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Jumping straight to grep | Read the area `STEERING.md` first — it names the modules and the external boundaries |
| Treating a variant as a footnote | Each named variant (SSO/non-SSO, eligible/blocked) is its own use case with its own preconditions |
| Documenting a feature branch | Use cases describe production behavior — document the default branch, or state the ref explicitly |
| Scoping the flow to end at the triggering action | The flow ends where the user sees the result — list the outcome surfaces now, with UI copy / PRD promises as the checklist |
