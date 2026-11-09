# Step 3 — Draft the flows

Inventory every case the sources imply, then write each flow into the doc.

---

## 1. Inventory

- One flow per actor goal. A variant that changes the actor or the surfaces is its own flow; a variant that only branches a step is an Other-cases entry. One home per variant.
- For each flow, list the happy path and every other case the sources imply: declines, failures of external systems, empty states, permission denials, duplicates, lost connectivity, limits and reversals. Mine the PRD's requirements and open items and the brief's failure modes and FAQ.
- If the feature extends a flow that already runs, read how that flow behaves today before listing its cases: the area's STEERING.md End-to-end Flows section when it covers the flow, otherwise the code. Wherever today's behavior meets a new requirement in a way neither source mentions, such as a side effect, a stored value the change would overwrite or delete, or a check that runs in only one place, add a case tagged `(today — <where read>)`, with a `Not defined` line where the sources don't say what the feature does there, and pin the ref you read in the header's Sources. State today's behavior only as far as you traced it, error paths included; a reading you didn't trace end to end says `unconfirmed`. Record only the meeting points; documenting the existing flow is `trace-flow-behavior`'s job.
- A flow ends where the user sees the result, not where the triggering action returns. List its outcome surfaces now.
- Write the inventory into the doc as the Flows skeleton before drafting any step. Every inventory case must end up in a flow, as a case or as a `Not defined` line; a case that silently disappears is a defect.

## 2. Draft each flow

Use the template's fields: Journey overview, Happy path, Other cases, Outcome surfaces and Negative space. They are the field names of the STEERING.md End-to-end Flows section, so `trace-flow-behavior` can verify this doc after the build.

- Tag every step and case (standing rule 1 in SKILL.md).
- Where a divergence from Step 2 touches a case, give both sides with their tags and add `(diverges between PRD and brief)`.
- Quote on-screen copy only when the PRD or a linked design gives it. Otherwise describe what the message says, without inventing wording.
- Cite no code for the new behavior; it doesn't exist yet. A `today` tag points at the STEERING flow or the symbol today's behavior was read from.
- Where the sources are silent, write `Not defined in the PRD or the brief: <the question>` in the case. Don't fill the gap with a plausible guess.
- Leave out Corrections and Mirror; `trace-flow-behavior` adds them after the build.

## 3. Write as you go

Append each flow to the doc as soon as it is drafted, before starting the next.

---

## Notes

| Issue | Correct |
|---|---|
| A screen layout written as a step | Rewrite it as behavior; keep the layout as `(mock proposal)` only if the doc's purpose is a mock |
| A case with no source and no gap note | Tag it `(proposal — from <basis>)` or mark it `Not defined in the PRD or the brief` |
| Today's behavior written as a requirement ("unchanged", "as today") | Keep it as context under its `today` tag. What the feature must do comes from the PRD or the brief, so a carry-over they don't state is a `Not defined` line |
