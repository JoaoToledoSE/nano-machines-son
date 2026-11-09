# Step 2 — Map where the PRD and the brief disagree

Compare the two sources before writing any case, so each disagreement is flagged in the case it touches instead of being settled by whichever source you read last.

---

## 1. Compare

Walk every brief section that settles, answers or comments on behavior (decisions, failure modes, non-goals, open questions, FAQ, and any findings on PRD items) against every PRD section that states or leaves open a behavior (requirements, edge cases, MVP cut, open items, decision log). Note three kinds:

- **Contradiction:** they describe the same behavior differently.
- **Brief decision not in the PRD:** the brief settles something the PRD doesn't mention or leaves open.
- **Open in one, answered in the other.**

The brief points to the PRD instead of restating it, so silence in the brief is agreement by reference, not a divergence.

## 2. Keep the map for Step 3

Write each divergence, with both sides' words and sections, to a working note in `<repo-root>/reports/create-use-cases-<feature_package>-$(date +%F)/` (`mkdir -p` it first), not to the doc. The path is repo-root-relative because `.gitignore` ignores only `/reports/`; a `reports/` folder under any other directory can get committed. The doc has no divergence section: Step 3 flags each one in every case it touches. A divergence that touches no case stays out of the doc; the PRD and the brief are where their owners reconcile it.

## 3. Flag, don't resolve

- Don't edit either source, and don't apply "the PRD wins" on your own: a brief decision can be newer than the PRD. Their owners reconcile them.
- A case a divergence touches gives both sides with their tags and adds `(diverges between PRD and brief)`. Cases it doesn't touch go ahead normally.

---

## Notes

| Issue | Correct |
|---|---|
| The brief is silent on a PRD requirement | Not a divergence; the brief points to the PRD |
| A PRD open item is answered by a brief decision | A divergence of the third kind; flag it in the cases that depend on it |
