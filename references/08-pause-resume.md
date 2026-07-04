# Pause, Resume, And Handoff

Use this phase to make long-running project management controllable.

## Pause Entry Conditions

- The user says pause, stop, hold, wait, do not continue, or equivalent.
- The project reaches a safety boundary.
- Required user decision is missing.

## Pause Actions

Do immediately:

- Stop creating new threads.
- Pause or update heartbeat automation when tools are available.
- Send stop instructions to active child threads when thread tools are available.
- Mark `CURRENT.md` and `HANDOFF.md` as paused.
- Record the reason and what must happen before resume.
- Mark active thread statuses using `references/09-status-schemas.md`.

## Pause State

```text
Project State: PAUSED
- Paused At:
- Paused By:
- Reason:
- Active Threads Notified:
- Heartbeat Status:
- Safe Resume Condition:
- Next Action On Resume:
```

## Resume Entry Conditions

- The user explicitly says resume or continue.
- The pause reason has been resolved.

## Resume Actions

- Read `CURRENT.md`, `HANDOFF.md`, `THREADS.md`, and `DECISIONS.md`.
- Inspect active thread statuses before sending follow-ups.
- Rebuild the next-step plan from files, not memory.
- Ask before restarting heartbeat if the previous pause was manual.
- Do not assume old child threads are still safe to continue.
- Re-check whether project direction, tools, or user priorities changed during the pause.

## Final Handoff

When closing a project or major stage, write:

```text
FINAL_REPORT.md
```

or update `HANDOFF.md` with:

```text
Final Handoff
- Completed Work:
- Accepted Artifacts:
- Verification Evidence:
- Remaining Risks:
- Archived Threads:
- Superseded Decisions:
- Suggested Next Project:
```

## Exit Gate

Pause/resume is complete when the project state matches reality and a new thread can continue without reading old chat history.
