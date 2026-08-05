# Step 2 — Trace the Behavior (Backend)

Establish, with citations, what actually happens at each step of the flow: entry points, gates, outcomes, exceptions, async follow-ups.

---

## 1. Fan out parallel Explore agents

One agent per dimension, dispatched in a single message. Typical split (adapt to the flow):

- **Main flow** — the happy path end-to-end, including any handoff to external systems and the return path (sync jobs, webhooks, replicas).
- **Auth/entry variants** — how each actor variant reaches the flow (login modes, SSO, deep links).
- **Gating** — permissions, feature flags, eligibility/state fields: where each is stored, where each is *enforced*, and what a blocked user gets.

Set search breadth to **very thorough**. Seed each prompt with the directories from Step 1's steering read.

## 2. What to demand from each agent

Every agent prompt must ask for:

- **Ordered journey steps** with `file:symbol` citations.
- **Preconditions/gates** for each step: org settings, feature flags, model fields, config records — and *where each is enforced* (frontend hint vs server-side check are different facts).
- **Exact backend messages/status codes** for failures (verbatim strings).
- **A gate's enforcing code and its human-readable description are separate facts — verify both, independently.** A Slack/email/error message describing a rule (e.g. "staff or the uploader") is a paraphrase, not the rule; trace the actual condition (the `if`, the `.filter()`, the decorator) and name the exact field it checks. A method's name is not proof either — a manager method called `for_user()` can filter by organization, not user id. If the message and the code disagree, that disagreement IS a finding — surface it, don't silently adopt the friendlier wording.
- **Async follow-ups**: sync tasks, their cadence, idempotency.
- **An explicit "could NOT confirm from this repo" list** — external-system behavior gets marked, never inferred.

## 3. Consolidate

Write `step2-behavior.md` in the report directory: the merged journey (numbered), a preconditions table per variant, the exceptions list with verbatim backend messages, and the external/unconfirmed list. Do this **before** starting Step 3.

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Assuming a backend error string is user-visible | It's a *backend* fact until Step 3 confirms the frontend renders it (e.g. PAC login's 401 "No PAC profile found" is swallowed by a generic "Login Unsuccessful" modal) |
| Treating login/auth as the permission gate | Auth often only checks identity/existence; the capability gate (eligibility, permission) is usually enforced later, at page render or submit |
| Trusting a message string or method name as the gate | Both are human paraphrases of the code, not the code — trace the actual `if`/`.filter()`/decorator and name the field it checks; a mismatch (Slack text says "staff or uploader," the queryset actually filters by organization) is a finding, not a detail to smooth over |
| One giant agent | Split by dimension — three focused agents return deeper, checkable reports than one broad one |
| Losing findings to compaction | Consolidate to disk before Step 3; agent transcripts are not a substitute for the merged summary |
