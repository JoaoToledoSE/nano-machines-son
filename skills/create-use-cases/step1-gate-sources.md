# Step 1 — Gate on the sources and frame the doc

Confirm both sources exist, pin the versions read, settle the framing, and create the doc.

---

## 1. Find and read both sources

- **PRD:** a Confluence page. Read it with the Atlassian MCP's `getConfluencePage` (`contentFormat: "markdown"`). Record the page version and last-edited date the tool returns, and the PRD owner.
- **Intent Brief:** a file in one of two repos. Briefs live both in this repo's `aidlc-docs/` folder and in the `QuorumUS/aidlc-docs` repo, so a path starting with `aidlc-docs/` can mean either. Start from the path the user gives and don't invent one; if the brief isn't in one repo, look in the other before calling it missing. Fetch the repo that holds it and read the brief from the ref it lives on (`git show <ref>:<path>` in a clone, or `gh api` without one). Record the repo, the ref and the short SHA, and the Tech Lead named in the brief's header.

Stop when either is missing, and say why:
- No PRD: this skill doesn't cover an Intent without one (see When to use in SKILL.md).
- No brief: the brief comes first, and `/aidlc:intent-planning` creates it.

## 2. Settle the framing, one question at a time

Offer candidate answers drawn from the sources, so the user picks instead of typing what the PRD already says.

1. **Purpose of the doc:** a mock UI, the spec for Construction, or a QA basis. It sets how fine-grained the cases are.
2. **Actors,** and which of them this doc covers.
3. **Output path:** the module and the feature's package name (see Output location in SKILL.md).
4. **Glossary:** take each term from the PRD. Where the brief uses another word for the same thing, keep the PRD's and note the other.

## 3. Create the doc

Copy the template linked from SKILL.md to the output path. Fill the header: the sources with their versions, the purpose, the actors and the glossary. Leave Flows and Coverage as placeholders; Steps 3 and 4 fill them. Write the file now.

---

## Notes

| Issue | Correct |
|---|---|
| The brief exists only on an unmerged branch | Read it with `git show origin/<branch>:<path>`; record the branch and SHA, and mark it unmerged in Sources |
| A source changes during the run | Re-read it, update its version in Sources, and re-run Step 2 before finishing |
| The PRD links a Figma or other design | Read it for on-screen copy only, and cite it as a source next to the PRD |
