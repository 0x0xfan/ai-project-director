# Roadmap

Use this phase to convert requirements into staged, bounded tasks.

## Entry Conditions

- Harness exists.
- MVP features and acceptance criteria exist.

## Allowed Actions

- Split work into D0/D1/D2/D3/D4 phases.
- Create task cards.
- Define dependencies and parallelization rules.
- Decide which tasks need read-only review before implementation.
- Define the merge or acceptance path for each write task.

## Forbidden Actions

- Do not implement tasks.
- Do not create write threads for tasks with unclear contracts.
- Do not let multiple tasks own the same files without an explicit merge plan.
- Do not mark a task `READY` until allowed files, forbidden files, and verification are named.

## Stage Model

- D0: contract, schema, interface, data model, or documentation convergence.
- D1: first runnable vertical slice.
- D2: feature expansion.
- D3: UX, performance, content, reliability, or detail polish.
- D4: release, cleanup, final evidence, and archive.

## Task Card

```text
Task Card
- Task ID:
- Stage:
- Title:
- Objective:
- Inputs:
- Allowed Files:
- Forbidden Files:
- Dependencies:
- Acceptance Criteria:
- Verification:
- Suggested Thread Role:
- Worktree Needed:
- Goal Needed:
- Review Required:
- Status:
```

## Parallelization Rules

- Parallelize read-only discovery and review freely.
- Parallelize write tasks only when file ownership is disjoint or worktrees are available.
- Do D0 before implementation when contracts, schema, or data flow are unstable.
- Do not start D2 before the D1 vertical slice can be verified.

## Exit Gate

Roadmap is complete only when:

- The next batch of tasks is small and bounded.
- Each task has acceptance criteria.
- Each task has allowed and forbidden files.
- Dependencies are explicit.
- The director can explain why tasks are parallel or serial.
- Task statuses use `references/09-status-schemas.md`.

## Roadmap Handoff

```text
Roadmap Handoff
- Current Stage:
- Next Task Batch:
- Parallel Tasks:
- Serial Tasks:
- Blocked Tasks:
- Required Threads:
- Worktree Plan:
- Review Plan:
- Recommended Next Phase: Orchestration
```
