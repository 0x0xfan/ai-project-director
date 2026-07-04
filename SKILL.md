---
name: ai-project-director
description: "Full lifecycle project director for local AI-assisted projects. Use when the user wants to start, clarify, plan, scaffold, manage, resume, or continuously advance a local project using Codex, including non-technical users who need project discovery, PRD/requirements discussion, project harness setup, task roadmap, multi-thread or worktree orchestration, heartbeat automations, adversarial review gates, pause/resume, and handoff discipline. Trigger for new project, project director, project workflow, project clarification, requirements discussion, local project orchestration, multi-thread project, heartbeat, subthread, worktree, or automatic project advancement."
---

# AI Project Director

## Purpose

Run a complete local project workflow from vague idea to clarified requirements, durable project files, task roadmap, controlled execution, independent review, and resumable handoff.

This skill is a stage-gated director, not a single-task executor. It should keep the project state visible in files and prevent premature implementation, runaway multithreading, and self-certified completion.

Use the user's current language for questions, summaries, project documents, and final responses unless the target repository uses a different convention.

## Core Rule

Every phase transition requires a handoff packet. Do not move to implementation, orchestration, or automation until the previous phase has produced its required artifacts and explicit acceptance criteria.

Hard gates:

- No discovery handoff -> do not write requirements.
- No requirements handoff -> do not build a roadmap.
- No harness/status files -> do not start multi-thread execution.
- No acceptance criteria -> do not start implementation or `/goal`.
- No independent adversarial review -> do not accept important implementation work as complete.
- No Git/worktree isolation -> do not run multiple write threads in the same project directory.
- User pause/stop instructions override all automation and active work.

## Workflow Router

First identify the current situation:

- Unclear whether this is new, existing, resumed, or single-task work -> read `references/00-intake-routing.md`.
- New vague project idea -> read `references/01-discovery.md`.
- Existing project with unclear requirements -> read `references/02-requirements.md`.
- Sparse project without durable state files -> read `references/03-harness.md`.
- Requirements exist but no task plan -> read `references/04-roadmap.md`.
- Project needs child threads, worktrees, goals, or heartbeat -> read `references/05-orchestration.md`.
- A task claims completion or needs review -> read `references/06-adversarial-review.md`.
- User wants recurring project advancement -> read `references/07-heartbeat.md`.
- User pauses, resumes, archives, or asks for handoff -> read `references/08-pause-resume.md`.
- Project needs consistent task/thread/status files -> read `references/09-status-schemas.md`.
- The skill or project workflow itself needs audit -> read `references/10-quality-audit.md`.

For mixed requests, start with the earliest missing phase. Example: if the user asks for multi-thread implementation but requirements are vague, do Discovery and Requirements first.

## Phase Map

### Phase 0: Intake

Goal: classify the request as new project, existing project, resumed project, or single task.

Read `references/00-intake-routing.md`.

Do:

- Identify project path, current files, Git status, and available instructions.
- Decide which phase is the first valid phase.
- Ask only the minimum clarifying questions needed to choose the phase.

Do not:

- Write code.
- Create child threads.
- Create automations.

Exit with an Intake Summary and next phase.

### Phase 1: Discovery

Goal: turn a vague idea into a project brief.

Use when target user, MVP, output, constraints, or success evidence are unclear.

Read `references/01-discovery.md`.

Exit only with a Discovery Handoff.

### Phase 2: Requirements

Goal: turn the project brief into verifiable requirements and first end-to-end flow.

Read `references/02-requirements.md`.

Exit only when at least one MVP feature has acceptance criteria and non-goals.

### Phase 3: Harness

Goal: make the project resumable by future threads.

Read `references/03-harness.md`.

Create or update durable files conservatively. Prefer existing project conventions. Do not overwrite user work.

Exit only when a fresh thread can answer: what is this project, what is current, what is next, how to run, how to verify, and what not to touch.

### Phase 4: Roadmap

Goal: split requirements into staged work and task cards.

Read `references/04-roadmap.md`.

Use D0/D1/D2/D3 staging:

- D0: contract, schema, interfaces, or documentation convergence.
- D1: first runnable vertical slice.
- D2: feature expansion.
- D3: UX/detail/performance polish.
- D4: release, cleanup, and final evidence.

Exit only when tasks have dependencies, allowed files, forbidden actions, and verification.

### Phase 5: Orchestration

Goal: manage execution through child threads, worktrees, goals, and heartbeat loops.

Read `references/05-orchestration.md`.

The main-control thread should coordinate rather than personally doing long implementation. It may make small state/document fixes when needed.

### Phase 6: Review Gate

Goal: challenge completed work before accepting it.

Read `references/06-adversarial-review.md`.

Implementation completion is only candidate completion. Important work must pass independent adversarial review before the director accepts it.

### Phase 7: Heartbeat

Goal: keep project advancement alive on a schedule without losing control.

Read `references/07-heartbeat.md`.

Heartbeat prompts must be durable, narrow, and status-aware. They should decide between `DONT_NOTIFY`, `NOTIFY`, `ASK`, `PAUSE`, or `STOP`.

### Phase 8: Pause, Resume, Handoff

Goal: make automation controllable and project state recoverable.

Read `references/08-pause-resume.md`.

When paused, stop scheduling new work and instruct active child threads to stop. When resumed, read state files before acting.

## Universal Handoff Packet

Every phase handoff must include:

```text
Handoff Packet
- From Phase:
- To Phase:
- Current Decision:
- Accepted Artifacts:
- Rejected or Superseded Artifacts:
- Open Questions:
- Scope Lock:
- Allowed Next Actions:
- Forbidden Actions:
- Verification Required:
- Active Threads:
- Blockers:
- User Decision Needed:
```

Write the packet into the appropriate project state file when the project has a harness. Use `CURRENT.md` for live status and `HANDOFF.md` for next-session continuity.

When task, thread, phase, or review status needs to be written, use the schemas in `references/09-status-schemas.md` rather than inventing new labels.

## Thread Roles

Use these roles consistently:

- `main-control`: project director, state owner, dispatcher, reviewer of reports.
- `read-only-review`: explores risk and options without editing files.
- `implementation`: performs one scoped task.
- `adversarial-review`: attacks a candidate result and looks for counterexamples.
- `repair`: fixes specific failed criteria from review.
- `heartbeat`: wakes the main-control thread and performs the next minimal management action.

## Relationship To Other Skills

- Use project initialization patterns from `harness-project-init` when creating durable repo files.
- Use `contract-first-loop` for one concrete task contract or when drafting a persistent `/goal`.
- This skill owns the full project lifecycle and decides when those narrower workflows are appropriate.

## Self-Audit

After creating or substantially updating this skill, audit it with `references/10-quality-audit.md` and run the standard skill validator. Treat validation success as necessary but not sufficient; the workflow must also pass adversarial checks for phase leakage, unsafe automation, missing handoffs, and ambiguous completion.

## Final Response

Keep user-facing updates concise:

- Current phase.
- What was created or changed.
- Gate status.
- Next recommended phase or action.
- Questions only when the next phase cannot proceed safely without them.
