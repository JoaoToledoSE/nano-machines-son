---
name: weekly-reflection
description: Write Joao's end-of-week personal reflection — a warm, journal-style review of the work week (wins, key decisions, challenges, time & energy, what carries into next week) gathered from Google Calendar, Slack, Gmail, GitHub, and Jira, saved as a one-page PDF. Use whenever the user asks for a weekly reflection, weekly summary, end-of-week review, "wrap up my week", "how did my week go", or when a scheduled Friday weekly-reflection task fires. This is a personal processing tool for Joao, NOT a status report for management.
---

# Weekly Reflection

Write Joao Toledo's end-of-week personal reflection. This is for Joao himself — it helps him process the week, recognize accomplishments, and set up the next week. It is NOT a status report for management, so optimize for honesty and usefulness to Joao, not for looking good.

## Week scope

Compute the date range first: Monday of the current week through today. On macOS, `MONDAY=$(date -v-mon +%Y-%m-%d)` gives this week's Monday when run Friday. The reflection normally runs Friday afternoon, but if run on another day, still cover Monday through today and use today's date in the filenames.

## Data gathering

Gather from every source below, in parallel where the tools allow. **If a source is unavailable** (connector not set up, auth expired, command fails), don't abort — gather what you can and add a short "Sources missing this week: …" footnote at the end of the reflection so Joao knows the picture is partial.

### 1. Google Calendar (connector)

Load the calendar tools with ToolSearch (search "calendar events"). Pull all events Monday–today for joao.toledo@quorum.us. Note which meetings happened, which were skipped/declined, and how time was distributed: deep work vs. meetings vs. 1:1s.

### 2. Slack (connector)

Load Slack tools with ToolSearch (search "slack"). Scan Joao's key channels and DMs for the week. Look for: decisions made, problems solved, discussions led, help given to others, blockers encountered.

Search `from:<@U050KHW12VB>` (Joao's user ID) sorted by timestamp. Results come 20 per page and Joao's message volume is high, so don't page deep from Friday backwards — run one search per day-window instead (e.g. `after:` / `before:` pairs covering Mon–Tue, Wed, Thu–Fri) to guarantee the whole week is covered.

### 3. Gmail (connector)

Load Gmail tools with ToolSearch (search "gmail" or "email search"). Pull notable threads from the week, filtering out automated noise (Sentry, Haystack, Jira, Datadog, GitHub notifications). Focus on strategic discussions, cross-team coordination, and anything Joao drove forward.

### 4. GitHub (`gh` CLI — no connector needed)

```bash
gh search prs --author=@me --created=">=$MONDAY" --json number,title,repository,url,state
gh search prs --reviewed-by=@me --updated=">=$MONDAY" --json number,title,repository,url,author
```

Don't pass `--state` — it only accepts open|closed and omitting it returns all states including merged. Drop PRs from the reviewed list that Joao also authored (his handle is JoaoToledoSE). Capture PR numbers and titles — the Wins section wants specifics.

### 5. Jira (Atlassian MCP)

Load `searchJiraIssuesUsingJql` with ToolSearch. Query:

```
(assignee = currentUser() OR reporter = currentUser()) AND updated >= startOfWeek()
```

Don't filter by project — Joao's home project is QPE (PAC team / Cobalt Squad) but his week often spans COR, DATA, and others.

### 6. Last week's reflection (continuity)

Read the most recent `.md` in `~/Documents/weekly-reflections/`. Check its "Carrying Into Next Week" list and note follow-through (done / moved forward / still open) in this week's reflection. This is what turns consecutive reflections into a running journal instead of isolated snapshots. If the directory is empty, this is the first run — skip silently.

## Output

### Structure — use exactly these five sections

**This Week's Wins** — What went well. Concrete accomplishments — shipped features, unblocked teams, good conversations, problems solved, progress on key projects. Be specific with names, PR numbers, ticket keys, project names. Celebrate the work.

**Key Decisions & Discussions** — Important decisions Joao was part of or drove. Strategic conversations, architecture choices, team direction. Brief context on each.

**Challenges & Friction** — What was hard, what didn't go as planned, blockers encountered. Not a complaint list — frame as learning opportunities or things to address.

**Time & Energy** — A brief reflection on how the week's time was spent. Was there enough deep work? Too many meetings? Any patterns worth noting?

**Carrying Into Next Week** — Open threads, upcoming deadlines, things to follow up on. 3–5 items max.

### Tone and length

Warm and reflective — a thoughtful journal entry, not a corporate report. Use Joao's actual context (project names, people, ticket keys) so it feels personal and useful. Roughly one page of content total.

### Files

1. Write the markdown to `~/Documents/weekly-reflections/YYYY-MM-DD-weekly-reflection.md` (today's date). Always write the `.md` — it's the source next week's run reads for continuity. Create the directory if it doesn't exist.
2. Render the same content as a PDF at `~/Documents/weekly-reflections/YYYY-MM-DD-weekly-reflection.pdf` using the `anthropic-skills:pdf` skill.
3. End the chat turn with a short TLDR of the week and the paths to both files.

## Setup requirements

- **Google Calendar, Gmail, and Slack connectors** — added via the Claude desktop app's connector settings. Until they're connected, runs degrade to GitHub + Jira with a missing-sources footnote.
- **`gh` CLI** authenticated (already true on this machine).
- **Atlassian (Jira) MCP** connected (already true in this environment).
