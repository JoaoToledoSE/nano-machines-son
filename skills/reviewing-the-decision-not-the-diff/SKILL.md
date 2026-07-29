---
name: reviewing-the-decision-not-the-diff
description: Use when reviewing a pull request, branch, or diff — especially one whose description or comments assert why the change is correct, one that writes to data or contracts another repo or team owns, or one where the review has produced a list of findings with no way to rank them.
---

# Reviewing the Decision, Not the Diff

## Overview

Most findings in a diff are consequences of one structural decision. Find it and the list collapses — which is why a root-first review is also a short one.

**Core principle:** a wrong rationale outlives the bug it describes. The bug is discoverable by anyone who looks; a confident comment stops the next person looking.

## When to Use

- The diff or its description explains **why** it is correct ("matches X", "by construction", "safe because")
- It changes something two systems must agree on — basis, unit, timezone, encoding, identity, wire format
- It writes to data or a contract another repo, service, or team owns
- You have findings and can't rank them

**Not for:** dependency bumps, mechanical renames, formatting.

**Pairs with:** superpowers:requesting-code-review dispatches the review; this skill is the pass the reviewer runs.

## The Pass

1. **Name the decision.** One sentence: *this diff chose ___ over ___.* If the change crosses an ownership boundary, write one line per system: what it does with the shared value and what unit/basis/assumption it encodes. Contract bugs are invisible from either side alone.
2. **List the correctness and safety claims** in the diff and its description — skip motivation and context; mark each verified / refuted / not-checked. Claims citing code outside the diff are the highest-yield thing you will check.
3. **Read the path, not the symbol.** A grep proves the cited mechanism exists *somewhere* in that file, which reads as confirmation and is not. Follow the call path and cite the file:line actually on it — mechanisms are often per-operation.
4. **Name the mutation.** Per claimed guarantee, write the one-line change that breaks it and cite the assertion that catches it, or state that none does. Passing tests are not coverage.
5. **Sort by dissolution.** Per finding: does fixing the decision make this disappear? If yes it's a consequence. If several roots survive, report each. If nothing dissolves, the findings are independent — say so and report them flat.
6. **Cost it.** Estimate the order of magnitude of data touched or calls affected — a nit at 10 rows is a finding at 10 million. Read the nearest downstream consumer of a changed contract to bound blast radius.

## Reporting

Keep whatever slots the harness or template gives you. Change the order and nesting inside them:

```
Root — decision sentence, the side-by-side (boundary-crossing diffs only), what you'd change   ← this gets the words
  ├─ consequence, one line          ← they are one fix, not four
  └─ consequence, one line
Independent findings
Verified / couldn't verify
Minors
Verdict — one line
```

Length is a signal: a long review means you did not find the root.

## Common Mistakes

| Mistake | Why it fails |
|---|---|
| Rationale error filed as Minor ("it's just a comment") | It's the durable artifact — the code gets read through it |
| Reviewing only the changed hunks | The decision is visible against the unchanged code |
| Forcing one root onto independent findings | Author fixes the fake root; the real findings vanish |
