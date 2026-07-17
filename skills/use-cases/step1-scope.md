# Step 1 — Scope the Flow

Pin down exactly what is being documented before any exploration.

---

## 1. Extract from `$ARGUMENTS`

- **The flow**: one sentence, actor-first ("a supporter donates by credit card on a PAC Action Center").
- **Variants to cover**: each becomes its own use case (e.g. SSO vs non-SSO login, eligible vs blocked).
- **Assumptions**: things to take as given, excluded from the use cases (e.g. "the donation form is already configured"). These still appear as one-line preconditions, just not as steps.
- **Audience**: default is the whole team (plain language). If the user asked for an engineering-level doc, keep code symbols; otherwise they are banned from the final output (Step 4).

If the flow or variants are ambiguous, ask **one** clarifying question now — not during synthesis.

## 2. Orient in the codebase

- Check the ref: `git status` — if not on up-to-date `master`, tell the user which ref is being documented.
- Read the area's `STEERING.md` (and `CLAUDE.md`/`ADR.md` if present) for every app the flow touches. The steering doc usually names the exact subdirectories, external systems, and gating fields — this makes the Step 2/3 agent prompts dramatically better.
- Note which parts of the flow live **outside this repo** (external systems, hosted iframes, third-party IdPs). These will be marked "external — illustrative/unverified" throughout; never present their internals as fact.

## 3. Set up persistence

```bash
mkdir -p reports/use-cases-<slug>-$(date +%F)
```

Write `step1-scope.md` in that directory: flow, actors, variants, assumptions, areas/apps involved, external systems. Every later step appends its findings to this directory before moving on (context-reset safety).

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Jumping straight to grep | Read the area `STEERING.md` first — it names the modules and the external boundaries |
| Treating a variant as a footnote | Each named variant (SSO/non-SSO, eligible/blocked) is its own use case with its own preconditions |
| Documenting a feature branch | Use cases describe production behavior — document master, or state the ref explicitly |
