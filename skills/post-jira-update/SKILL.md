---
name: post-jira-update
description: Draft a high-level, non-technical status update for a Jira ticket, show it to the user for validation, and post it as a comment only after explicit approval. Use whenever the user asks to "post an update", "write a status update", "comment on the ticket", "share progress with stakeholders", or wants a summary of technical work that CS, AM, Product, Support, or leadership can read — even if they don't say "Jira" explicitly but a ticket is in context. Also use for /post-jira-update <TICKET-KEY>.
---

# Post Jira Update

Turn technical findings into a stakeholder-readable Jira comment. The audience is
everyone on the ticket who is *not* an engineer — CS, AMs, Product, Support,
leadership. They need to understand what's happening and what happens next,
without reading code paths or jargon.

## Workflow

1. **Resolve the ticket.** Use the key from the arguments, or the ticket already
   under discussion in the conversation. If neither exists, ask. Never guess a
   ticket key — posting to the wrong ticket is worse than asking.

2. **Gather what's known.** Prefer facts already established in the conversation
   (a triage, an investigation, a fix that shipped). If the conversation lacks
   substance for an update, fetch the ticket (`getJiraIssue`, fields including
   `comment`) and summarize its current state honestly — don't invent progress.

3. **Draft the update** using the structure below.

4. **Show the draft and stop.** Present it in chat, ask what to change, and
   explicitly offer to adjust anything that commits the team (dates, sprints,
   "low-risk" claims). **Never post without the user's explicit approval — this
   gate is the point of the skill.** "Looks good", "approved", "post it" count;
   silence or a topic change does not.

5. **Post on approval** via `addCommentToJiraIssue` with
   `contentFormat: "markdown"` and `responseContentFormat: "markdown"`
   (cloudId: the site hostname, e.g. `yourcompany.atlassian.net`). If the
   user asked for edits, apply them and show the revised draft again before
   posting.

6. **Confirm with the link.** After posting, give the user the ticket's browse
   URL so they can verify the rendered comment.

## Update structure

Adapt headings to the situation (triage update, progress update, fix shipped,
blocked) — but keep this shape: lead with a dated header, then answer the
questions a non-engineer actually has, in this order.

```markdown
**{Type of update} — {Month D, YYYY}**

{1-2 sentence summary: what we did and the headline conclusion.}

**What's happening today:** {The user-visible symptom or current state, in the
words a customer would use.}

**Why this happens:** {Plain-language cause. Name prior tickets/releases by key
if they matter to the story, but explain them — "a security fix shipped earlier
this year (ABC-123) correctly stopped X" — never bare references.}

**What this delivers / what we're doing:** {Numbered list of concrete outcomes,
phrased as what people will experience, not what code will change.}

**What this is:** {Classification a PM cares about: planned enhancement vs. new
bug vs. regression; risk level; whether groundwork exists.}

**Next step:** {One line. Only commit to timing the user has confirmed.}
```

## Writing rules

- **No code identifiers.** No file paths, function names, branch names, HTTP
  status codes, or stack terminology. "A blank access-denied page", not
  "a bodyless 403 response in views.py".
- **Explain every ticket reference.** A key like QI-33937 means nothing to CS —
  always pair it with a plain description of what it was.
- **Say what the reader gains.** Frame deliverables from the affected person's
  perspective: "the user will see a friendly page explaining they're not
  eligible", "support gets an automatic alert".
- **Honest classification.** If it's not a bug, say so plainly — stakeholders
  make different decisions for "planned enhancement" vs. "regression".
- **Don't overcommit.** Timing promises ("this sprint", dates) go in only if the
  user confirmed them; flag them explicitly when presenting the draft so the
  user consciously approves the commitment.
- **Length:** aim for something readable in under a minute — roughly 150-250
  words. If the draft runs long, cut detail from "why" before cutting "what's
  happening" or "next step".
