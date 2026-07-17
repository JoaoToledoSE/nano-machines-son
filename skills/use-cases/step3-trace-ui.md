# Step 3 — Trace the Real UI (Frontend)

Extract enough visual truth from the frontend code to rebuild each screen as a faithful mockup — and to correct Step 2's assumptions about what users actually see.

---

## 1. Fan out parallel Explore agents

One agent per UI surface group, dispatched together. Typical split:

- **The flow's main widget/page** (e.g. the form component): DOM structure, sections, conditional rendering per state.
- **Chrome + entry surfaces**: login/registration widgets, header/nav, footer, blocked/error pages, and the **theme system**.

Set search breadth to **very thorough**. State explicitly in the prompt: *"I care about exact visual/DOM structure and styling, not data flow."*

## 2. What to demand from each agent

- **A structural DOM sketch** (nested element list with component/class names).
- **Verbatim styled-component/stylus CSS** for key elements: colors, radii, borders, shadows, font sizes, layout.
- **Theme source + DEFAULT values**: where colors/fonts come from (org design model, ThemeProvider, CSS vars) and the exact default hex values when an org doesn't customize. Mockups use the defaults; the doc notes orgs customize.
- **Exact user-facing strings** (i18n keys + English values): titles, labels, placeholders, button text, error copy, empty states.
- **How errors surface**: modal vs toast vs inline banner vs field error — and *which message* each failure actually shows.
- **Conditional rendering per state**: what appears/disappears when the user makes each choice.
- An explicit unconfirmed list (unresolvable CSS vars, runtime-injected values).

## 3. Reconcile against Step 2 — this is where corrections happen

For every backend message and every step in `step2-behavior.md`, ask: *does the frontend actually render this?* Record every mismatch — these are the most valuable findings in the doc (support-ticket shape, UX surprises). Examples of the class:

- A specific backend refusal replaced by a generic error modal.
- A selection that swaps out entire form sections (fields the doc would otherwise show don't exist in that state).
- Either/or rendering where a composite was assumed (SSO button *replaces* username/password).

Write `step3-ui.md` in the report directory: per-screen spec (structure, key CSS values, exact strings) + the corrections list.

---

## Gotchas / Notes

| Issue | Correct |
|---|---|
| Mockup shows a backend string the UI never renders | Only strings confirmed in frontend code/i18n go into mockups |
| Assuming frontend constants are the theme defaults | JS fallback constants often differ from the real model defaults — take defaults from the settings/design *model* |
| Drawing a composite screen | Respect conditional rendering: mock each state as it actually renders |
| Embedded external content (iframe) | Render it as the seamless region it is, caption it as external, and mark its internals illustrative |
