---
name: create-use-cases
description: Writes the expected-behavior use cases of a feature that is not built yet, from its PRD and Intent Brief, into a use-cases.md in the feature's app folder. Every line names its source, and a case where the PRD and the brief disagree is flagged in place. Use whenever someone asks for use cases, casos de uso, user journeys or expected behavior for a feature still in planning, before code exists, even if they don't name this skill. To document how a flow already behaves in the app, use trace-flow-behavior instead.
argument-hint: <feature> [PRD URL] [Intent Brief path]
---

# Create Use Cases

`$FEATURE` is the feature named in `$ARGUMENTS`; the PRD URL and the Intent Brief path may follow it.

## When to use

- The feature has a PRD and an Intent Brief, and what it adds isn't built yet. It may extend a flow that already runs; the addition is the unbuilt part.
- **Not** for documenting how a flow behaves today: `trace-flow-behavior` does that.
- **Not** for an Intent with no PRD. There the brief carries the requirements, so there is nothing to compare it against. Say so and stop.

The doc holds use cases only. Divergences, decisions and open questions are tracked in the PRD and the brief by their owners; this doc flags them in the case they touch and points there.

## Standing rules — hold for the whole run

These apply to every line you write, in every step. A use-case doc gets read as the spec, so anything it states without a source becomes a requirement nobody decided.

1. **Every line names its source**, as a tag at the end of the step or case:
   - `(PRD § <section>)`: a PRD requirement or decision.
   - `(brief § <section>)`: an Intent Brief decision, failure mode or answered question.
   - `(proposal — from <basis>)`: your draft, with what it was inferred from. It stays a proposal until its owner writes it into the PRD or the brief.
   - `(mock proposal)`: a UI detail for a mock, not a requirement.
   - `(today — <where read>)`: how a flow this feature extends behaves now, read from the area's STEERING.md End-to-end Flows or from the code. It is context for a case, never a requirement.

   Two markers go in the case itself:
   - `(diverges between PRD and brief)`: the sources say different things about this case. Give both sides with their tags and don't pick one.
   - `Not defined in the PRD or the brief: <the question>`: neither source settles this case. Write the concrete question instead of filling the gap with a guess.
2. **Only the PRD and the brief settle behavior.** A decision counts once its owner records it in one of them; then cite it there. An answer in chat, a meeting or a ticket comment leaves the line a proposal until it lands in the PRD or the brief. Never name a person as the source of a behavior: the doc would credit them with a choice they may not have made, and reviewers stop questioning a line that carries a name.
3. **An inference from a technical constraint is a proposal.** When an integration's shape suggests a behavior (a webhook that fires once per payment suggests one notification per payment), tag it `(proposal — from <the constraint>)`. An API's shape is not a product decision.
4. **The PRD is canonical and Product-owned.** Where product behavior is unsettled, the case says so and the question goes to the PRD owner; this doc must not become a side channel around the PRD. Don't edit the PRD or the brief: the brief belongs to its Tech Lead.
5. **Write into the final file from the first draft.** Never keep the draft in chat or scratch to paste elsewhere later; a handoff to another session or a ticket passes the file path. Pasted text loses its provenance, and the next reader credits it to whoever pasted it.
6. **Behavior, not screens.** Write actor, goal, outcome and variations. Screen layout appears only as `(mock proposal)`, because layout in a use-case doc reads as a requirement and freezes the design before Design sees it.
7. **Spell out every reference.** Name PRD items and brief questions by their words, never "Q10" or "item 25" alone: numbering changes between versions, and a reader can't resolve it.
8. **Language:** talk to the user in their language; write the doc in English.

## Output location

`app/<module>/<feature_package>/use-cases.md`: the folder named like the feature's Python package, with underscores, since `app/` subfolders are packages. Create the folder if no code exists yet. If the package name isn't settled, ask the Tech Lead; if the code lands elsewhere later, the doc moves with it.

The doc is the run's persistence: write each section as soon as it is drafted, so a compaction or an interruption loses nothing.

Template: [assets/use-cases-template.md](assets/use-cases-template.md).

---

## Step 1 — Gate on the sources and frame the doc

Read [step1-gate-sources.md](step1-gate-sources.md) and follow it for `$FEATURE`.

---

## Step 2 — Map where the PRD and the brief disagree

Read [step2-divergences.md](step2-divergences.md) and follow it.

---

## Step 3 — Draft the flows

Read [step3-draft-flows.md](step3-draft-flows.md) and follow it.

---

## Step 4 — Coverage and handoff

Read [step4-coverage.md](step4-coverage.md) and follow it.

---

## Done when

- Every step and case has a source tag, a divergence marker or a `Not defined` line.
- Every divergence between the PRD and the brief that touches a case is flagged in that case, with both sides.
- Every PRD requirement maps to a case or to "out of scope", with the reason.
- The user has the list of divergences and `Not defined` questions to take to the PRD owner and the Tech Lead.
- Nothing was committed, pushed or posted without the user's explicit OK.

## After the build

Run `trace-flow-behavior` on each flow. In its step 1, start the case inventory from this doc's cases and add every case the trace finds that this doc misses. The area's STEERING.md End-to-end Flows section then becomes the source of truth; add a line at the top of this doc pointing there.
