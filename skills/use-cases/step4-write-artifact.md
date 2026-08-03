# Step 4 — Write the MD and publish the Confluence page

Exactly **two deliverables, always**: the canonical MD record (for engineers and future LLM sessions) and its Confluence page (for Product/CS/Support). MD first — it is the system of record; the page is its plain-language projection. Nothing else is produced: no HTML file, no published artifact, no third format.

---

## 0. The canonical MD record (write this first)

Write `app/<area>/docs/use-cases/<slug>.md` — the engineering trace. Optimize for signal density; future sessions and engineers will grep and cite it. Reference example (validated end-to-end): quorum-site `app/pac/authorized_contribution/docs/use-cases/historical-pledge-import-to-actions.md`.

Structure:

- **H1** `Use cases: <flow>`.
- **Header blockquote**: *What this is* (one paragraph; names the Confluence projection and the area STEERING/ADR); *Traced ref* (commit, date, merge status — updated if the ref merges later); *Companion artifacts* (the Confluence page URL + the note that captures live as its attachments).
- **Capture ledger**: one environment line (stack/ref, accounts used, dev-only chrome removed), then a table `# | Image | State | Status` where Status is the Step 3 honesty ledger (`CAPTURED` / `CAPTURED (forced: <method>)`), plus rows for `UNREACHABLE` states. Every filename in the ledger is a page-attachment name and **must resolve to an existing attachment**.
- **Architecture in one paragraph** (cite the area `STEERING.md` for depth).
- **Journey overview** (numbered).
- **One section per UC**: Who; preconditions table with **where each is enforced**; flow steps with `path — Symbol` citations (symbols, not line numbers — they drift); exceptions with verbatim backend messages **and** what the user actually sees. Operational/product facts the code can't show (e.g. "only the PS team runs this") go in a clearly-marked note with their source.
- **The Step 3 corrections section** (backend-vs-frontend mismatches) — the highest-value content.
- **Configuration reference table**; explicit **external/unverified** list.
- Placement rule (per AIDLC / PLAYBOOK): capability behavior docs are **co-located with the code** (`app/<area>/docs/use-cases/`). Never `aidlc-docs/areas/`. If the area has a `STEERING.md`, add a one-line pointer to this doc; do NOT create a steering trio just for this.

## 1. The Confluence page (exact format)

Create it in the team's Confluence space, titled **"<Feature> — How It Works"**. Reference example (validated by Product review): *Historical Pledge Import (Authorized Contributions) — How It Works* in the DEVTEAM space. Structure, in order:

1. **Status line**: a `PENDING VALIDATION` status chip · `Audience: <who>` · Jira ticket (inline card) · Epic (inline card) · PRD link.
2. **`What this feature does`** — 2–3 plain sentences, then the numbered outcomes ("each imported pledge is: 1. recorded in X, 2. turned into Y"). No meta commentary — no "newly built", no dates, no provenance.
3. **`Who's involved`** — the 2–3 parties with one-phrase roles, **operationally true** (who actually performs the flow — confirm with the feature owner — not whoever the gate theoretically permits).
4. **`Before <the flow> can happen`** — a checkbox task-list of preconditions in the reader's terms.
5. **`Step by step — the happy path`** — bold-numbered paragraph steps ("**1. Pick the upload type**. …"); system-performed steps carry an *(automatic)* marker; screenshots embedded at their steps.
6. **`Where <the results> show up`** — a bullet per surface, screenshots, then "worth knowing" bullets (each expressed in reader terms, e.g. "grids show the historical date; the payroll report shows the import date").
7. **`When it doesn't work — the failure cases`** — one intro line carrying the global guarantee ("failures are always per-row — one bad row never stops the rest"), then **`Case N — <title>`** (heading 3) per failure, each an ordered list with the happy path's anatomy: *what you see* (the message the user actually sees, quoted; screenshot when captured) → *what it means* → *what to do*. Not a lumped table, not prose blobs.
8. **`What this feature does NOT do`** — bullets, with follow-up ticket links where work is deferred.

Hard rules for the page body:

- **It is the least technical artifact — only what the audience acts on.** System guarantees are expressed inside the step or failure case they affect, in reader terms; a guarantee never gets its own callout panel. No code symbols, endpoints, flags, repo paths, PR numbers, or capture methodology anywhere on the page — provenance lives exclusively in the MD, which links the page (engineers find the page from the MD, not the reverse).
- Every image is an attachment embedded as **`mediaSingle` with `layout: "wide"`** (full-size inline; media-groups render as click-to-open thumbnails — wrong for a screenshot doc), captioned below in italics: *"Screenshot N — <state>"*.

## 2. Screenshot rules

- Every image on the page is a **real capture** from Step 3's curated set — never a reconstruction, never retouched. A state with no capture gets a text panel ("runs in <external system>") — never an invented visual.
- A forced state's caption carries the `(forced)` marker — the page never presents a forced state as organic.
- Captures show local seed data; nothing resembling a real client may appear (re-seed and re-capture, never retouch).
- **Captures are page attachments ONLY — never committed to the repo** (screenshots rot with the UI; binaries persist in monorepo history forever). Every MD-ledger row must resolve to an existing attachment; a capture trimmed from the page body still gets attached (un-embedded) so the ledger never points at nothing.
- **Token bootstrap (first-time users).** The attachment upload needs the user's Atlassian API token. Look for it before publishing images — env var or shell rc (commonly `ATLASSIAN_API_TOKEN` in `~/.zshrc`; check the *name* only, never print values). **If none exists, stop and guide the user through creating one** — do not dead-end, and do not ask them to paste the token into the chat:
  1. Open <https://id.atlassian.com/manage-profile/security/api-tokens> → **Create API token** → name it (e.g. `claude-confluence`) → copy it.
  2. Have the user add it to their own shell rc themselves: `export ATLASSIAN_API_TOKEN=<paste>` in `~/.zshrc` — they edit the file; the token value never passes through the conversation.
  3. Resume: read it into a shell variable from the rc at use time (`TOKEN=$(grep ... ~/.zshrc | cut -d= -f2-)`), never echo it, never write it anywhere else.
- **Attachment + embed recipe (proven).** The Atlassian MCP cannot create attachments, and data-URIs can't carry real screenshots through tool arguments (base64 ≈ 1 token/char — measured). Instead: (1) upload each image via the Confluence REST API with the token above (read into a shell variable, never print it): `curl -u "<email>:$TOKEN" -H "X-Atlassian-Token: nocheck" -F "file=@<img>.png" https://<site>/wiki/rest/api/content/<pageId>/child/attachment`, capturing `extensions.fileId` from each response; (2) embed each image as `{"type":"mediaSingle","attrs":{"layout":"wide"},"content":[{"type":"media","attrs":{"type":"file","id":"<fileId>","collection":"contentId-<pageId>"}}]}` — via a REST ADF transform (GET `api/v2/pages/<id>?body-format=atlas_doc_format`, splice, PUT version+1), since the MCP's html dialect only writes media-groups; (3) verify by re-fetching the ADF and counting mediaSingle nodes.

## 3. Plain-language rules (the page, always)

- No code symbols, endpoints, model names, or feature flags in the body. Translate: token → "secure one-time pass (60 seconds, single use)"; replication + cron → "syncs back automatically within minutes".
- Keep verbatim **user-facing** copy in quotes — the one place exactness beats plainness.
- Surface the Step 3 corrections where the reader hits them ("the screen shows a generic message; the real cause is X — check X first") — that's what makes the page useful to Support.

## 4. Publish and iterate

- Create once (`createConfluencePage`, html dialect is fine for the text skeleton); **every subsequent change targets the same pageId** — surgical edits via the REST ADF transform with a version message per change. Never mint a second page.
- The `PENDING VALIDATION` chip stays until the feature owner validates; then flip it and link the page from the Jira ticket and epic.
- The MD ships through the normal PR flow (link the Confluence page in the PR body); the page is updated in place as review feedback lands — and the MD is updated in the same pass when a page correction reveals a fact the trace lacked (e.g. an operationally-true actor).

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Drafting the whole doc in context, writing at the end | Write the MD incrementally; a compaction mid-draft loses everything |
| Creating a new page for a revision | Same pageId, version messages — one page, updated in place |
| Treating the page as the record | The MD is canonical; the page is the plain-language projection |
| Putting the doc in `aidlc-docs/areas/` | Behavior docs co-locate with the code (`app/<area>/docs/use-cases/`) |
| Burying the behavior corrections | They're the headline — in the failure cases and in the chat summary |
| Synthesizing a visual for an uncaptured state | Text panel with the UNREACHABLE reason — images are real captures only |
| Real-client-like data visible in a capture | Re-seed/rename and re-capture — never retouch, never ship it |
| Committing capture PNGs to the repo | Page attachments only; the MD ledger pins names + provenance |
| Trying to push image bytes through MCP tool arguments | base64 ≈ 1 token/char — physically impossible; REST-attach, embed by fileId |
| Publishing with 📸 placeholder slots | The page isn't done until every image is attached + embedded (mediaSingle wide) |
| Engineering framing on the page (provenance panels, guarantee callouts, theoretical actors) | Provenance → MD only; guarantees → inline in their step/case; actors → operationally true, confirmed with the owner |
