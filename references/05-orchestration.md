# Orchestration

Use this phase to run the project through child threads, worktrees, goals, and status files.

## Entry Conditions

- Roadmap handoff exists.
- Task cards exist.
- Project status files exist.

## Capability Recheck

Before creating or promising child threads, goals, worktrees, or automations, re-check that the tools are available in the current thread and environment. If availability changed since Intake, update the orchestration plan and use the fallback path instead of continuing on stale assumptions.

## Allowed Actions

- Create child threads when thread tools are available.
- Send targeted prompts to existing threads.
- Use worktrees for isolated write work when the project is a Git repo.
- Draft or start `/goal` for long-running task threads.
- Update `THREADS.md`, `CURRENT.md`, and `HANDOFF.md`.
- Use `references/09-status-schemas.md` for thread and task status.

## Forbidden Actions

- Do not create child threads without a task card.
- Do not create multiple write threads in the same non-Git folder.
- Do not let child threads choose their own scope.
- Do not accept implementation-thread completion as final.
- Do not create recursive uncontrolled thread spawning.
- Do not promise background progress when thread, goal, automation, or worktree tools are unavailable.

## Thread Prompt Contract

Every child thread prompt must include:

```text
- Role:
- Task ID:
- Objective:
- Project path:
- Files to read:
- Files allowed to edit:
- Files forbidden to edit:
- Acceptance criteria:
- Verification required:
- Output report format:
- Stop/block conditions:
```

## Thread Roles

- `read-only-review`: may inspect and report; must not edit files.
- `implementation`: may edit only allowed files; must produce evidence.
- `adversarial-review`: must challenge a candidate result; should not repair unless explicitly assigned.
- `repair`: may fix only failed criteria from a review.

## Worktree Policy

- Prefer worktree for isolated coding tasks in Git repositories.
- Keep one implementation thread per worktree.
- Add `.worktreeinclude` for required ignored local files such as `.env.local`, only after confirming they are needed.
- If worktree tools are not exposed, either create a thread in the current project with strict file ownership or provide the user with a manual worktree plan.

## Tool Fallbacks

- If thread creation is unavailable, produce child-thread prompts for the user to paste manually and keep orchestration in the current thread.
- If automation creation is unavailable, draft the heartbeat prompt and ask the user to create it.
- If `/goal` is unavailable, create a bounded task contract and tell the user what persistence will be lost.
- If worktree creation is unavailable, use disjoint file ownership or serialize write tasks.

## Goal Policy

Use `/goal` or a goal lifecycle tool when:

- The task may require multiple attempts.
- Verification will determine the next step.
- The task should continue across turns.

Do not use `/goal` for vague requirements or missing acceptance criteria.

## Status Files

Record active coordination in:

```text
THREADS.md
CURRENT.md
HANDOFF.md
```

Minimum thread registry row:

```text
| Task ID | Thread ID | Role | Scope | Status | Last Evidence | Next Action |
```

## Exit Gate

Orchestration handoff is ready when:

- Every active child thread has a task ID and role.
- No thread has unbounded scope.
- Completed candidate work is queued for adversarial review.
- Blocked threads have clear next input needed.
- Status files reflect the real thread state, including unknown or unreadable threads.

## Orchestration Handoff

```text
Orchestration Handoff
- Active Threads:
- Completed Candidate Work:
- Threads Needing Correction:
- Threads Needing Review:
- Blockers:
- Status Files Updated:
- Next Heartbeat Action:
```
