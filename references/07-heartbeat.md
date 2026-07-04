# Heartbeat

Use heartbeat automation to keep a project moving while preserving control.

## Entry Conditions

- The project has a harness.
- The main-control thread has a clear role.
- Active tasks or threads exist.
- The user wants recurring follow-up or background advancement.

## Allowed Actions

- Create or update a thread automation when automation tools are available.
- Draft a durable heartbeat prompt when tools are not available.
- Tune cadence by project phase.
- Record automation ID and cadence in `CURRENT.md` and `HANDOFF.md`.
- Read `references/09-status-schemas.md` when updating task or thread state.

## Forbidden Actions

- Do not create heartbeat before project state files exist.
- Do not use heartbeat to bypass missing user decisions.
- Do not let heartbeat perform broad file edits without a task card.
- Do not notify the user on every wakeup unless something changed.
- Do not restart after a manual pause unless the user explicitly resumes.

## Cadence

- Fast active push: 5-10 minutes.
- Normal active development: 20-45 minutes.
- Maintenance: daily or weekly.

Short cadence is useful when child threads finish quickly. Long cadence is better for low-risk monitoring.

## Heartbeat Prompt Shape

```text
This is the main-control heartbeat for <project>.
On wake:
1. Read CURRENT.md, HANDOFF.md, AGENTS.md, THREADS.md, and latest child-thread statuses.
2. Check for drift, blocked work, unverified completion, and stale state.
3. If a child thread completed, read its report and route to adversarial review.
4. Take the next smallest management action.
5. Update CURRENT.md and HANDOFF.md if state changes.
6. Return one decision: DONT_NOTIFY, NOTIFY, ASK, PAUSE, or STOP.
Do not expand scope. Do not edit protected files. Do not create new write work without a task card.
```

## Decisions

- `DONT_NOTIFY`: no meaningful state change.
- `NOTIFY`: state changed but no user decision is needed.
- `ASK`: user decision is required.
- `PAUSE`: user requested stop or a safety boundary requires pause.
- `STOP`: project is complete, invalid, or cannot continue.

## Exit Gate

Heartbeat is healthy when:

- It reads project state before acting.
- It records meaningful state changes.
- It does not spam the user.
- It does not create unbounded work.
- It can be paused safely.
- It reports tool limitations instead of pretending unattended work is available.
