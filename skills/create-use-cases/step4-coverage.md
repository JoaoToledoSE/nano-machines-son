# Step 4 — Coverage and handoff

Check that every PRD requirement has a home, then hand the doc and its loose ends to the user.

---

## 1. Coverage

Fill the table: every PRD requirement, acceptance criterion and edge case, by its words and section, mapped to the cases that cover it, or to `out of scope — <reason, source>`.

## 2. Done check

Check each item of "Done when" in SKILL.md and report its state to the user.

## 3. Handoff

- Give the user the doc's path.
- In your message, not in the doc, list what the owners need to settle: each case marked `(diverges between PRD and brief)` and each `Not defined` question, with the case it sits in and its owner (the PRD owner for product behavior, the Tech Lead for engineering). They settle it in the PRD or the brief, and the doc then cites it there.
- Branch, commit, push, open a PR, or write to Jira or Confluence only on the user's explicit OK, following the repo's commit conventions.

---

## Notes

| Issue | Correct |
|---|---|
| The user answers a gap in chat | Not a source yet. Keep the line as it is and say the answer belongs in the PRD or the brief; cite it once it's there |
| A PRD requirement with no case and no out-of-scope reason | Add the case, or mark it out of scope with the source that says so |
