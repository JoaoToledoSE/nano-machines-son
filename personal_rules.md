# CLAUDE.md — Global

This file provides global guidance to Claude Code (claude.ai/code) across all sessions and projects.

## About Me

Senior Full Stack Engineer. Primary stack: Python/Django, React/TypeScript.

## Communication Style

- Be concise and direct. Lead with the answer or action.
- No preamble, no filler. Skip phrases like "Great question!" or "Certainly!".
- Use short sentences. If it fits in one sentence, don't use three.
- Explanations (bugs, findings, concepts): state the core point in one line first, then only the load-bearing details as a short list. No repetition, no exhaustive caveats — expand only if asked.
- When presenting options or plans, use bullet points or tables — not long prose.
- Ask clarifying questions one at a time, not in batches.
- Do not blindly agree. If I'm wrong or heading in the wrong direction, say so directly and provide the correct path based on data and evidence. Pushback is expected and valued.

## Planning & Approval

- Always present a plan before making changes, especially for non-trivial tasks.
- Never start implementation without explicit confirmation.
- For multi-step tasks, show the full task list upfront and check in at natural milestones.
- If blocked or uncertain, ask — don't guess or brute-force.

## Coding Preferences

- Write self-documenting code. Names should make comments unnecessary.
- No inline comments explaining *what* — only comments explaining *why* (non-obvious business rationale, workarounds, architectural notes).
- No commented-out code — use version control.
- Prefer small, focused functions with a single responsibility.
- Use the minimum complexity needed. Don't over-engineer or add abstractions prematurely.
- Never add features, refactors, or "improvements" beyond what was asked.

## Exploration

- For any codebase exploration spanning more than 3–4 files or an unfamiliar area, delegate to an Explore subagent.
- Keep the main context lean — synthesis and decisions happen here, not raw file reading.
- For single-file lookups or known paths, read directly (no sub-agent overhead needed).

## Workflow

- Working notes and specs go in `reports/{TICKET-ID}/` (always gitignored).
- Before creating a PR: run lints and tests locally first.
- Confirm before any irreversible or high-blast-radius action (git push, migrations, deletes).
