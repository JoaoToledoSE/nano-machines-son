---
name: use-cases
description: Produce a plain-language use-case document for a product flow — step-by-step use cases with preconditions, exceptions, and UI mockups rebuilt from the real frontend code — published as an HTML artifact the whole team (PM/CS/Support) can read. Use when asked for "use cases", a "user journey", or team-readable documentation of how a feature behaves.
argument-hint: <flow description> [variants to cover] [assumptions]
---

# Use Cases

`$FLOW` is the product flow to document from `$ARGUMENTS` (e.g. "supporter donates by credit card on a PAC Action Center"). `$ARGUMENTS` may also name variants to cover (e.g. "SSO and non-SSO login, eligibility") and assumptions to take as given (e.g. "assume the donation form is configured").

## When to use

Someone needs to understand how a flow actually works today — end-to-end, with what the user sees on screen — in language a non-engineer can read. The output is a published artifact: use cases in journey order, each with preconditions, numbered steps, exceptions, and mockups faithful to the real UI.

## Prerequisites

- Working tree on an up-to-date `master` (or explicitly note which ref is being documented — a stale branch documents behavior that isn't in production).
- Exploration is delegated to parallel Explore agents; keep the main context for synthesis.

## Output location

Working notes: `reports/use-cases-<slug>-<YYYY-MM-DD>/` (gitignored). **Final outputs are two committed files, co-located with the code area** (per AIDLC: capability behavior docs follow the code — never `aidlc-docs/areas/`, which is for Intent working docs):

```
app/<area>/docs/use-cases/<slug>.md     # canonical record — engineering trace with path — Symbol citations
app/<area>/docs/use-cases/<slug>.html   # artifact source — plain-language + mockups, republishes to the same URL
```

The published artifact is a human-facing **projection** of the HTML, not the system of record. If the area has a `STEERING.md`, add a pointer to the `.md` there.

---

## Step 1 — Scope the flow

Read `step1-scope.md` (in this skill's directory) and follow it for `$FLOW`.

---

## Step 2 — Trace the behavior (backend)

Read `step2-trace-behavior.md` (in this skill's directory) and follow it for `$FLOW`.

---

## Step 3 — Trace the real UI (frontend)

> Runs in parallel with nothing — it depends on Step 2's map of which surfaces exist. Its findings routinely **correct** Step 2 assumptions about what users actually see.

Read `step3-trace-ui.md` (in this skill's directory) and follow it for `$FLOW`.

---

## Step 4 — Write and publish the artifact

Read `step4-write-artifact.md` (in this skill's directory) and follow it for `$FLOW`.
