# {Feature} — use cases

> **Status:** expected behavior. The feature is not built yet. After it ships, `trace-flow-behavior` verifies these flows against the running app, and the area's STEERING.md becomes the source of truth.
>
> **Sources:** the [PRD]({url}) (version {n}, edited {date}) is canonical · the [Intent Brief]({path}) ({repo}, {ref} @ {sha}) · today's behavior of {the flow it extends, if any}: {STEERING section or code} at {ref} @ {sha}
>
> **Purpose:** {mock UI | spec for Construction | QA basis}
>
> **Actors:** {actor} (in scope) · {actor} (out of scope)
>
> **How to read the tags:** `PRD § …` is a PRD requirement or decision. `brief § …` is an Intent Brief decision, failure mode or answered question. `proposal — from …` is a draft by Claude, with what it was inferred from; it is not a requirement until its owner writes it into the PRD or the brief. `mock proposal` is a UI detail for a mock only. `today — …` is how a flow this feature extends behaves now; it is context, not a requirement. `diverges between PRD and brief` marks a case where the two sources disagree; both sides are given. `Not defined in the PRD or the brief` marks a case neither source settles, with the question to ask.

## Glossary

- **{Term}:** {meaning} ({source}). {The brief's other word for it, if any.}

## Flows

### {Flow name: actor and goal}

- **Journey overview:**
  1. {stage}
- **Happy path:**
  1. {step} ({source})
- **Other cases:**
  - **{Case}** → {what happens} → {what the actor sees or does next} ({source})
  - **{Case}** → {the PRD's version} (PRD § {section}) · {the brief's version} (brief § {section}) (diverges between PRD and brief)
  - **{Case}** → Not defined in the PRD or the brief: {the question}
- **Outcome surfaces:** {where the result becomes visible or usable} ({source})
- **Negative space:** {where it deliberately does not appear} ({source})

## Coverage

| PRD requirement | Covered by | Notes |
|---|---|---|
| {words} (PRD § {section}) | {flow: case} | {out of scope: reason, source} |
