# Step 4 — Write and Publish the Artifact

Compose **two deliverables** from the consolidated Step 2/3 findings: the canonical MD record (for LLMs + engineers) and the HTML artifact (for humans). MD first — it's the system of record; the HTML is its plain-language projection.

---

## 0. The canonical MD record (write this first)

Write `app/<area>/docs/use-cases/<slug>.md` — the engineering trace. This is what future Claude sessions and engineers will grep and cite; optimize for signal density, not presentation:

- Header block: what the doc is, the artifact URL (so any session can republish in place), scope + traced ref/date, assumptions.
- **Capture ledger** (from Step 3): state → image file → `CAPTURED` / `CAPTURED (forced: <method>)` / `UNREACHABLE — <why>`. Screenshots live in `app/<area>/docs/use-cases/<slug>/` and are referenced with relative paths so they render on GitHub.
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
   - **What the user sees**: real screenshots with numbered captions (see §2).
   - **When it doesn't work**: enumerated failure cases given the same weight and anatomy as the happy path — one numbered/titled case per failure, each consisting of: what you see (the message the user *actually* sees, screenshot when captured) → what it means → what to do. Not a lumped table, not prose blobs.
   - Callouts for the non-obvious facts (e.g. "signing in ≠ allowed to donate").
5. **Admin checklist table**: what has to be configured, where, in plain language.
6. **Footer scope note**: what is external/unverified; where the engineering-level trace lives.

Diagrams (mermaid) are **optional** — only include if the user asks; the use cases + mockups are the deliverable.

## 2. Screenshot rules

- Every image in the deliverables is a **real capture** from Step 3's committed set (`app/<area>/docs/use-cases/<slug>/NN-<state>.png`) — never a reconstruction, never retouched. If a state has no capture, it gets a text panel ("runs in <external system> — not capturable locally" or the ledger's UNREACHABLE reason), not an invented visual.
- Caption each screenshot with its state; a forced state's caption carries the `(forced)` marker — the doc never presents a forced state as organic.
- Captures show local seed data; the doc notes that orgs customize branding/data ("layout and flow exact, data is seeded sample data"). Nothing resembling a real client may appear.
- The MD references the PNGs by relative path (renders on GitHub). The published projection carries the same files: base64-embedded for an HTML artifact (the CSP blocks remote images), attachments for a Confluence-page deviation.
- **A Confluence projection is not done until the captures are ON the page.** Filename-labeled placeholder slots are an intermediate editing state, never the deliverable — the page ships with the images attached and embedded at their journey positions.
- **Confluence attachment recipe (proven).** The Atlassian MCP cannot create attachments, and data-URIs can't carry real screenshots through tool arguments (base64 ≈ 1 token/char — measured). Instead: (1) upload each image via the Confluence REST API with the user's existing API token (commonly `ATLASSIAN_API_TOKEN` in their shell rc — read it into a shell variable, never print it): `curl -u "<email>:$TOKEN" -H "X-Atlassian-Token: nocheck" -F "file=@<img>.png" https://<site>/wiki/rest/api/content/<pageId>/child/attachment`, capturing `extensions.fileId` from each JSON response; (2) embed each image where it belongs — **as a `mediaSingle` node (ADF, `layout: "wide"`), not `mediaGroup`**: groups render as click-to-open thumbnails, singles render full-size inline, which is what a screenshot doc needs. The MCP's html dialect only writes media-groups, so either write the body via `contentFormat: "adf"`, or publish html media-groups first and convert them with a small REST ADF transform (GET `api/v2/pages/<id>?body-format=atlas_doc_format`, splice each `mediaGroup` into per-media `{"type":"mediaSingle","attrs":{"layout":"wide"},"content":[media]}`, PUT with version+1); (3) verify by re-fetching the page (ADF) and counting mediaSingle nodes. If the user has no API token anywhere, report the page as **incomplete/blocked** naming the missing images — never report a placeholder page as published-done.
- Keep screenshots light-theme depictions; the doc chrome itself must support light + dark.

## 3. Plain-language rules (unless Step 1 said engineering audience)

- No code symbols, endpoints, model names, or feature flags in the body. Translate: token → "secure one-time pass (60 seconds, single use)"; replication + cron → "syncs back automatically within minutes"; mirror table → "the PAC's member list".
- Keep verbatim **user-facing** copy in quotes — that's the one place exactness beats plainness.
- Surface the Step 3 corrections prominently (callouts): "the user sees X even though the real reason is Y" is what makes the doc useful to Support.
- **A Confluence projection is the least technical artifact — its body is what the audience acts on.** The page consists of: the journey steps with screenshots, what the user sees at each one, what to do when something fails, and where things become visible afterwards. System guarantees are expressed inside the step or failure entry they affect, in the reader's terms ("re-uploading the same file is safe", "one bad row doesn't stop the rest") — a guarantee never gets its own callout panel. Provenance — repo paths, PR numbers, capture methodology, the trace-doc pointer — appears **nowhere on the page**: it lives in the in-repo MD (header block + capture ledger), and the MD links the page. Engineers find the page from the MD, not the reverse.

## 4. Publish

- Publish with the Artifact tool from the `app/<area>/docs/use-cases/<slug>.html` path. **Revisions must republish the same file path** (same session) or pass the artifact `url` (future sessions) — a new path mints a new URL.
- Keep `<title>` and favicon stable across republishes.
- Both files are meant to be **committed** (they're permanent docs, not `reports/` residue) — but committing is the user's call; leave them untracked and say so.
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
| Synthesizing a visual for an uncaptured state | Text panel with the UNREACHABLE reason — images are real captures only |
| Publishing a Confluence projection with 📸 placeholder slots | The page isn't done until images are attached + embedded — REST-attach with the user's API token, then MCP-embed by fileId (see §2 recipe) |
| Trying to push image bytes through MCP tool arguments | base64 ≈ 1 token/char — physically impossible for real screenshots; attach via REST, embed by fileId |
| Real-client-like data visible in a capture | Re-seed/rename and re-capture — never retouch the image, never ship it |
| Hot-linking screenshots from an external host | Commit them next to the doc; embed base64 (artifact) or attach (Confluence) |
