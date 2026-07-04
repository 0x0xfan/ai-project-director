# Status Schemas

Use these schemas to keep project, task, thread, review, and heartbeat state consistent across sessions.

## Phase Status

Use only:

```text
NOT_STARTED
IN_PROGRESS
WAITING_USER
BLOCKED
COMPLETED
SUPERSEDED
PAUSED
```

## Task Status

Use only:

```text
BACKLOG
READY
ACTIVE
CANDIDATE_DONE
IN_REVIEW
NEEDS_REPAIR
BLOCKED
ACCEPTED
SUPERSEDED
PAUSED
```

`CANDIDATE_DONE` means an implementation thread claims completion. It is not accepted until adversarial review passes.

## Thread Status

Use only:

```text
PLANNED
CREATED
ACTIVE
WAITING
CANDIDATE_DONE
REVIEWING
NEEDS_CORRECTION
BLOCKED
STOPPED
ARCHIVED
UNKNOWN
```

If a thread cannot be read, mark it `UNKNOWN` and do not assume completion.

## Review Status

Use only:

```text
ACCEPTED
NEEDS_REPAIR
BLOCKED
NEEDS_USER_DECISION
INVALID_REVIEW
```

Use `INVALID_REVIEW` when the review did not inspect evidence, exceeded its scope, or repaired instead of reviewing.

## THREADS.md Suggested Table

```text
| Task ID | Thread ID | Role | Scope | Allowed Files | Forbidden Files | Status | Last Evidence | Next Action |
```

## TASKS.md Suggested Block

```text
## <Task ID> - <Title>

- Status:
- Stage:
- Owner Thread:
- Objective:
- Allowed Files:
- Forbidden Files:
- Dependencies:
- Acceptance Criteria:
- Verification:
- Review Required:
- Last Evidence:
- Next Action:
```

## File Responsibility Boundaries

- `DECISIONS.md`: irreversible or expensive-to-reverse direction changes, with reason, superseded assumptions, affected files, verification needed, and revisit trigger. Append only; do not rewrite history.
- `CURRENT.md`: live snapshot of this moment, including current phase, WIP, active threads, blockers, last verified evidence, next smallest action, and user decisions needed. Update whenever state changes.
- `HANDOFF.md`: first file to read on resume. Extract the practical resume bridge from `CURRENT.md`: read first, do next, do not do, active threads to check, required verification, and safe resume condition.

## CURRENT.md Suggested Top Section

```text
# Current Project State

- Project State:
- Current Phase:
- Active Mainline:
- Current WIP:
- Active Threads:
- Last Verified Evidence:
- Blockers:
- Next Smallest Action:
- User Decision Needed:
```

## HANDOFF.md Suggested Top Section

```text
# Handoff

- Last Updated:
- Resume From:
- Read First:
- Do Next:
- Do Not Do:
- Active Threads To Check:
- Required Verification:
- Safe Resume Condition:
```

## Decision Hygiene

Record direction changes in `DECISIONS.md`:

```text
## YYYY-MM-DD - <Decision>

- Decision:
- Reason:
- Supersedes:
- Affected Files:
- Verification Needed:
- Revisit Trigger:
```
