---
name: use-cases
description: Produce use-case documentation for a product flow — always exactly two deliverables: an in-repo technical MD (the engineering trace) and a Confluence page (plain-language, with real UI screenshots captured from the locally running app via Playwright) the whole team (PM/CS/Support) can read. Use when asked for "use cases", a "user journey", or team-readable documentation of how a feature behaves.
argument-hint: <flow description> [variants to cover] [assumptions]
---

# Use Cases

`$FLOW` is the product flow to document from `$ARGUMENTS` (e.g. "supporter donates by credit card on a PAC Action Center"). `$ARGUMENTS` may also name variants to cover (e.g. "SSO and non-SSO login, eligibility") and assumptions to take as given (e.g. "assume the donation form is configured").

## When to use

Someone needs to understand how a flow actually works today — end-to-end, with what the user sees on screen — in language a non-engineer can read. The outputs are **exactly two, always**: an in-repo technical MD (the system of record) and a Confluence page in the team space (journey steps, real screenshots, enumerated failure cases). No other format is produced.

## Prerequisites

- Working tree on an up-to-date `master` (or explicitly note which ref is being documented — a stale branch documents behavior that isn't in production).
- **The flow's app must be runnable locally — screen captures are mandatory; there is no mockup path.** Visuals in the deliverables are real Playwright screenshots of the running app, never reconstructions. If the project defines an app-driving skill (stack check, login, feature flags — e.g. quorum-site's `verify-frontend`), follow its procedures instead of improvising.
- Exploration is delegated to parallel Explore agents; keep the main context for synthesis. The capture session itself runs in the main context (browser state is interactive).

## Output location

Working notes: `reports/use-cases-<slug>-<YYYY-MM-DD>/` (gitignored). **Final outputs are exactly two — one committed file plus one Confluence page:**

```
app/<area>/docs/use-cases/<slug>.md            # canonical record — engineering trace with path — Symbol citations (committed, co-located with the code per AIDLC; never aidlc-docs/areas/)
Confluence page "<Feature> — How It Works"     # plain-language projection in the team space; all captures live as its attachments (never committed to the repo)
```

The page is the human-facing **projection** of the MD; the MD is the system of record and links the page. If the area has a `STEERING.md`, add a pointer to the `.md` there.

---

## Step 1 — Scope the flow

Read `.claude/skills/use-cases/step1-scope.md` and follow it for `$FLOW`.

---

## Step 2 — Trace the behavior (backend)

Read `.claude/skills/use-cases/step2-trace-behavior.md` and follow it for `$FLOW`.

---

## Step 3 — Trace and capture the real UI

> Runs in parallel with nothing — it depends on Step 2's map of which surfaces exist. Its findings routinely **correct** Step 2 assumptions about what users actually see.

Read `.claude/skills/use-cases/step3-trace-ui.md` and follow it for `$FLOW`.

---

## Step 4 — Write the MD and publish the Confluence page

Read `.claude/skills/use-cases/step4-write-artifact.md` and follow it for `$FLOW`.
