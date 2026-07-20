# Step 4 — Write and Publish the Artifact

Compose **two deliverables** from the consolidated Step 2/3 findings: the canonical MD record (for LLMs + engineers) and the HTML artifact (for humans). MD first — it's the system of record; the HTML is its plain-language projection.

---

## 0. The canonical MD record (write this first)

Write `app/<area>/docs/use-cases/<slug>.md` — the engineering trace. This is what future Claude sessions and engineers will grep and cite; optimize for signal density, not presentation:

- Header block: what the doc is, the artifact URL (so any session can republish in place), scope + traced ref/date, assumptions.
- Architecture in one paragraph; journey overview (numbered).
- One section per UC: preconditions (flags, org settings, model fields — with **where each is enforced**), flow steps with `path — Symbol` citations (symbols, not line numbers — they drift), exceptions with verbatim backend messages **and** what the user actually sees.
- The Step 3 corrections section (backend-vs-frontend mismatches) — the highest-value content.
- Configuration reference table; explicit external/unverified list.
- Placement rule (per AIDLC / PLAYBOOK): capability behavior docs are **co-located with the code** (`app/<area>/docs/use-cases/`). Never `aidlc-docs/areas/` (reserved for Intent working docs / the single master-index file). If the area has a `STEERING.md`, add a one-line pointer to this doc; do NOT create a steering trio just for this.

---

## 1. Document structure (HTML)

Write `app/<area>/docs/use-cases/<slug>.html` (same directory as the MD; put the artifact URL in an HTML comment at the top). Load the `artifact-design` skill before writing (required by the Artifact tool). Structure, in order:

1. **Header**: actor-first title ("How a supporter donates by credit card on…"), one-line scope, date.
2. **"Who's involved"**: the 2–3 parties with one-phrase roles, plus the single most load-bearing fact of the flow (e.g. "Quorum never sees the card number").
3. **Theme callout**: screens use the product's default theme (name the default hex values); orgs customize branding — "layout and flow exact, branding default".
4. **Use cases** (the core), in journey order — entry/sign-in variants first, gates second, the headline flow last. Each UC:
   - `UC-n — <actor-first title>` + a category chip (Sign-in / Eligibility / Donation…)
   - **Who**: one line.
   - **Before this can happen**: checkbox-style precondition list (plain language; assumptions from Step 1 appear here as single lines).
   - **Step by step**: numbered; steps executed by an external system get a visual "external" marker.
   - **What the user sees**: mockup screens with numbered captions (see §2).
   - **When it doesn't work**: exceptions with the message the user *actually* sees.
   - Callouts for the non-obvious facts (e.g. "signing in ≠ allowed to donate").
5. **Admin checklist table**: what has to be configured, where, in plain language.
6. **Footer scope note**: what is external/unverified; where the engineering-level trace lives.

Diagrams (mermaid) are **optional** — only include if the user asks; the use cases + mockups are the deliverable.

## 2. Mockup fidelity rules

- Screens are wireframe-scale reproductions of the **real widgets** from `step3-ui.md`: real layout, real default theme colors, real copy. Generic placeholder branding ("Example PAC") for org-specific content — never a real client's name/logo.
- Wrap each screen in minimal browser chrome (dots + URL pill) with a plausible generic URL.
- Hardcode the product's default theme values in the mockup CSS; keep screens light in both doc themes (they're depictions, like screenshots). The doc chrome itself must support light + dark.
- Reproduce **state**: per-UC screens show the conditional rendering exactly (sections that vanish, nav items stripped, error modal over dimmed page).
- Every string in a mockup must be traceable to `step3-ui.md`. If it can't be, don't render it.
- External embedded content: seamless region + caption "run by <external system> — exact look controlled by them".

## 3. Plain-language rules (unless Step 1 said engineering audience)

- No code symbols, endpoints, model names, or feature flags in the body. Translate: token → "secure one-time pass (60 seconds, single use)"; replication + cron → "syncs back automatically within minutes"; mirror table → "the PAC's member list".
- Keep verbatim **user-facing** copy in quotes — that's the one place exactness beats plainness.
- Surface the Step 3 corrections prominently (callouts): "the user sees X even though the real reason is Y" is what makes the doc useful to Support.

## 4. Publish

- Publish with the Artifact tool from the `app/<area>/docs/use-cases/<slug>.html` path. **Revisions must republish the same file path** (same session) or pass the artifact `url` (future sessions) — a new path mints a new URL.
- Keep `<title>` and favicon stable across republishes.
- Both files are permanent docs (not `reports/` residue) and belong in the repo. Claude never commits them itself — finish by telling the user they're ready to commit (typically alongside the area's docs in the same PR).
- In chat: link the artifact, lead with what changed, and call out the Step 3 corrections explicitly.

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Drafting the whole doc in context, writing at the end | Write MD and HTML incrementally; a compaction mid-draft loses everything |
| Republishing under a new filename | Same file path = same URL; new path = new artifact |
| Treating the artifact as the record | The MD is canonical; the artifact/HTML is the human-facing projection |
| Putting the doc in `aidlc-docs/areas/` | That tree is for Intent working docs; behavior docs co-locate with the code (`app/<area>/docs/use-cases/`) |
| Burying the behavior corrections | They're the headline — put them in callouts and in the chat summary |
| Real org branding in mockups | Always generic ("Example PAC", "Example Corp") — mockups must not impersonate a client |
