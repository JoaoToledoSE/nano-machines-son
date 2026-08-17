# Step 3 — Trace and Capture the Real UI

Build a state inventory from the frontend code, then drive the locally running app through every state with Playwright and screenshot it. **Deliverable images are only ever real captures** — there is no mockup path. This step also corrects Step 2's assumptions about what users actually see.

---

## 1. State inventory (light code trace)

Fan out one or two Explore agents (search breadth: medium — this is capture planning, not visual extraction). Demand:

- **Every screen/state in the flow**, including conditional renders (sections that appear/disappear per choice, per permission, per flag), error/empty states, and every outcome surface where the flow's result appears (Step 1 names them).
- **The gate for each state**: permission, feature flag, org setting, data precondition — and where it's enforced.
- **Entry routes/URLs** and the interaction that reaches each state from the previous one.
- **Exact user-facing strings** for states you may not manage to capture, and for reconciling backend messages against what the UI renders.

Do NOT ask for DOM structure or CSS — screenshots carry the visuals now.

## 2. Capture plan

Before opening a browser, write the plan as a table in `step3-capture-plan.md`:

| State | How to reach | Forcing required | Account / org |
|---|---|---|---|

**Reconcile the plan 1:1 against the §1 inventory before opening a browser**: every state and consumption surface the inventory names gets exactly one plan row — a capture intent or an explicit `UNREACHABLE` with its reason. An inventory state with no plan row is a defect of the plan.

"Forcing" is anything that makes a state reachable locally: enabling a feature flag, seeding data, editing DB rows, running a pipeline against seeded inputs, mocking a remote response. **A seeded-account deficit is forcing work, not unreachability** — a missing permission, flag, or data row/value on a local account is local-DB state: grant it, flip it, seed it. `UNREACHABLE` is reserved for states that live in an external system or genuinely cannot be produced locally, and its reason names the forcing that was attempted (or why none exists). All forcing is local-DB only, and every forcing method gets recorded (it appears in the doc's ledger).

## 3. Capture session (Playwright, main context)

### 3a. Capture environment — verify before you drive

The browser must be pointed at **the code ref being documented, backed by a local DB**. Do not assume the running app is either. Before logging in:

- **Identify what the app ports actually serve.** Check the listening containers/processes (`docker ps` + port mappings, `lsof -iTCP:<port>`) and the checkout/branch they mount. A backend connected to a **production or remote DB** on the app port (e.g. a `backend_prod` container) is a **hard stop**: never log in, never force states against it — get a local-DB backend serving instead.
- **Serve the documented ref.** If the ref lives in a git worktree whose stack binds no host ports (worktree isolation), the practical route is: stop the containers holding the app ports, temporarily point the main checkout at the documented commit (`git switch --detach <sha>` when the branch is held by a worktree), start the project's lean local stack, and run pending migrations. Record what you changed and **restore it after the session** (original branch, original containers).
- **Start the async workers the flow needs** (the project's Celery/worker task) — a lean stack queues background work but nothing consumes it.

### 3b. Playwright + login

- Drive with the Playwright MCP tools (`browser_navigate`, `browser_snapshot`, `browser_type`, `browser_click`, `browser_take_screenshot`, `browser_network_requests` for ground truth on saves).
- **Log in with the project's local seeded credentials** — the project's app-driving skill documents them (e.g. quorum-site's `verify-frontend`: seeded accounts, which have 2FA, the superuser caveat, the password-manager dialog gotcha). Generic flow: navigate to the login page, fill username/password from the seeded accounts, submit with Enter on the password field, and confirm the login POST fired before assuming success.
- Permission-gated states need a **non-superuser** account — superusers bypass the very gates you're documenting.

### 3c. Session rules
- Fix the viewport before the first shot (e.g. 1280×800, device-pixel-ratio 1) and keep it for the whole session — mixed sizes read as sloppy in the doc. Default (light) theme unless the flow is theme-specific.
- Drive each planned state and screenshot it to the scratch/session directory first; **curate** the finals as `NN-<state>.png` (numbered in journey order, kebab-case state names) — they become Confluence page attachments in Step 4, never repo files. Not every shot lands in the doc.
- **The happy path's capture sequence ends at the outcome surfaces** — the user seeing/using the result — not at the confirmation of the triggering action; number the captures through to them.
- Transient UI (toasts, auto-dismissing banners): screenshot immediately, and treat the network request (2xx save) as ground truth if the toast is gone.
- **Data hygiene**: captures use local seed data only. Nothing resembling a real client's name, logo, or production data may appear — re-seed or rename before capturing, never retouch the image.

## 4. Per-screen honesty ledger (required)

Every state from the capture plan gets exactly one line:

- **CAPTURED** — reached organically with a normal account.
- **CAPTURED (forced: <method>)** — real screenshot of a state forced locally (flag flipped, row seeded/edited, remote mocked). The method is part of the record.
- **UNREACHABLE — <why>** — external system, non-visual surface (no screen to capture; the why names how the result was verified instead), or locally unproducible; the reason names the forcing attempted (or why none exists). The doc gets a text description naming which class — external, unverified, or non-visual — never external/unverified for an owned, verified surface. **Never a synthesized visual.**

One surface never stands in for another unless both render the **same fact**: a marker/badge/status on surface A is not evidence of the values or content surface B shows. Each planned state gets its own line for the fact it shows — "already evidenced elsewhere" is not a ledger status.

## 5. Reconcile against Step 2 — this is where corrections happen

For every backend message and step in `step2-behavior.md`, ask: *does the live app actually render this?* You are now verifying against the running product, not inferring from code — trigger the failure where feasible and screenshot what actually appears. Record every mismatch (generic modal swallowing a specific refusal, sections that vanish, either/or rendering) — these are the most valuable findings in the doc.

Write `step3-ui.md` in the report directory: the capture ledger, per-screen notes (what the capture shows, any strings the UI renders differently than the backend sends), and the corrections list.

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Rebuilding a screen as HTML/SVG because capture is inconvenient | There is no mockup path. Force the state or mark it UNREACHABLE with a text description |
| Backend string assumed user-visible | Trigger it in the app; screenshot what actually renders (often a generic modal) |
| Capturing with whatever data is lying around | Seeded/generic data only; real-client-like data means re-seed, not retouch |
| Mixed viewport sizes across shots | Fix viewport + DPR 1 at session start, keep for every capture |
| A forced state presented as organic | The ledger line carries `(forced: <method>)` and the doc says so |
| Superuser account hides a permission-gated state | Capture permission-dependent states with a non-superuser account (see the project's app-driving skill) |
| External embedded content (iframe) | Capture the page as rendered; caption the embedded region as external — its internals are theirs |
| Inventory state with no capture-plan row | Reconcile 1:1 before the browser opens — every inventory state/surface gets a row: capture intent or explicit UNREACHABLE + reason |
| UNREACHABLE declared on a seeded-account deficit | A missing permission/flag/data value on a local account is forceable local-DB state — grant/flip/seed it; the ledger names the forcing attempted |
| Skipping a surface because another "shows the same thing" | Valid only when both render the same fact — a status marker on one surface is not the values/content another would show |
| Capture sequence ends at the "operation succeeded" screen | The happy path ends where the user sees the result — capture the outcome surfaces |
