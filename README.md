# ai-project-director

`ai-project-director` is a Codex Skill for running local AI-assisted projects as a staged, reviewable workflow.

It is designed for people who do not want to start with code immediately. The Skill first clarifies the project, turns the idea into requirements, creates durable project files, plans the roadmap, then manages execution through child threads, goals, heartbeat automation, and adversarial review gates.

## What It Solves

When working with AI on a local project, the common failure mode is not that the model cannot write code. The failure mode is that the work has no operating system:

- requirements live only in chat history
- AI starts implementing before the goal is clear
- multiple threads overwrite or confuse each other
- completed work is accepted without evidence
- automation keeps running after the user wanted to pause
- a new thread cannot resume because state was never written to files

This Skill turns that into a controlled lifecycle:

```text
Intake
-> Discovery
-> Requirements
-> Harness
-> Roadmap
-> Orchestration
-> Adversarial Review
-> Heartbeat
-> Pause / Resume / Handoff
```

## When To Use

Use this Skill when you want Codex to help with:

- starting a new software, content, operations, automation, or Skill project
- clarifying a vague idea before implementation
- discussing requirements and MVP scope
- creating project state files for future AI threads
- splitting work into staged task cards
- running local projects with multiple Codex threads
- using Git worktrees or goals for isolated execution
- setting up heartbeat-style project advancement
- reviewing completed work with independent adversarial checks
- pausing and resuming a long-running project safely

Do not use it for a single small task. For that, use a bounded task workflow such as `contract-first-loop`.

## Core Rules

The Skill is built around hard phase gates:

```text
No discovery handoff -> do not write requirements.
No requirements handoff -> do not build a roadmap.
No harness/status files -> do not start multi-thread execution.
No acceptance criteria -> do not start implementation or /goal.
No independent adversarial review -> do not accept important implementation work as complete.
No Git/worktree isolation -> do not run multiple write threads in the same project directory.
User pause/stop instructions override all automation and active work.
```

## Workflow

### 0. Intake

Classify the request:

- new project
- existing project
- resumed project
- single task
- project audit

The Skill checks project path, existing files, Git status, state files, and tool availability before choosing the next phase.

### 1. Discovery

Turns a rough idea into a project brief.

Typical outputs:

```text
docs/project-brief.md
docs/open-questions.md
```

### 2. Requirements

Converts the brief into verifiable requirements, MVP scope, non-goals, and acceptance criteria.

Typical outputs:

```text
docs/requirements.md
feature_list.json
evaluator-rubric.md
```

For non-code projects, `TASKS.md` can replace `feature_list.json`.

### 3. Harness

Creates or updates durable project files so future Codex threads can resume without relying on chat memory.

Typical outputs:

```text
AGENTS.md
CURRENT.md
HANDOFF.md
DECISIONS.md
THREADS.md
TASKS.md
docs/commands.md
docs/architecture.md
```

### 4. Roadmap

Splits work into staged tasks:

```text
D0: contract, schema, interface, data model, or documentation convergence
D1: first runnable vertical slice
D2: feature expansion
D3: UX, performance, content, reliability, or detail polish
D4: release, cleanup, final evidence, and archive
```

Each task gets allowed files, forbidden files, acceptance criteria, verification, dependencies, and review requirements.

### 5. Orchestration

Manages execution through child threads, worktrees, goals, and state files.

Thread roles:

```text
main-control
read-only-review
implementation
adversarial-review
repair
heartbeat
```

For Git repositories, implementation work should prefer isolated worktrees. For non-Git projects, multiple write threads should not run in the same directory.

### 6. Adversarial Review

Implementation completion is only candidate completion.

Important work must pass a separate review that looks for:

- failed acceptance criteria
- missing evidence
- forbidden file edits
- stale assumptions
- old project direction leaking back in
- unverifiable claims
- regression risk

Verdicts:

```text
ACCEPTED
NEEDS_REPAIR
BLOCKED
NEEDS_USER_DECISION
INVALID_REVIEW
```

### 7. Heartbeat

Creates or drafts heartbeat automation for recurring project management.

Heartbeat decisions:

```text
DONT_NOTIFY
NOTIFY
ASK
PAUSE
STOP
```

The heartbeat should read project state, inspect child thread status, route completed work to review, update status files, and avoid notifying the user when nothing important changed.

### 8. Pause, Resume, Handoff

Manual pause always wins.

When the user pauses, the Skill should stop creating new threads, pause heartbeat automation when possible, tell active child threads to stop, and mark `CURRENT.md` / `HANDOFF.md` as paused.

When resuming, it must rebuild state from files, not memory.

## Installation

Copy this folder into your Codex skills directory.

On Windows:

```powershell
Copy-Item -Recurse . "$env:USERPROFILE\.codex\skills\ai-project-director"
```

On macOS or Linux:

```bash
cp -R . ~/.codex/skills/ai-project-director
```

Then restart or reload Codex skills if needed.

## Usage

Start a new project:

```text
Use $ai-project-director to launch a new project.
Project idea: I want to build a local tool that turns product research notes into short video scripts.
Start from discovery. Do not write code yet.
```

Resume an existing local project:

```text
Use $ai-project-director to resume this project.
Read CURRENT.md, HANDOFF.md, AGENTS.md, and THREADS.md first.
Tell me the current phase, blockers, and next safest action.
```

Set up multi-thread execution:

```text
Use $ai-project-director to create a roadmap and decide which tasks can run in parallel.
Only start child threads after task cards, allowed files, forbidden files, and acceptance criteria are defined.
```

Run a review gate:

```text
Use $ai-project-director to adversarially review the candidate work from task D1.
Do not accept the implementation thread's self-report without evidence.
```

## Repository Structure

```text
ai-project-director/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── 00-intake-routing.md
    ├── 01-discovery.md
    ├── 02-requirements.md
    ├── 03-harness.md
    ├── 04-roadmap.md
    ├── 05-orchestration.md
    ├── 06-adversarial-review.md
    ├── 07-heartbeat.md
    ├── 08-pause-resume.md
    ├── 09-status-schemas.md
    └── 10-quality-audit.md
```

## Relationship To Other Workflows

This Skill is a top-level project director.

It can use narrower workflows when appropriate:

- project initialization patterns for durable files
- task contract loops for single bounded tasks
- `/goal` for long-running tasks with clear completion criteria
- child threads and worktrees for isolated execution
- adversarial review threads for quality gates

## Safety Notes

- Do not run unattended automation without clear pause and stop conditions.
- Do not allow multiple write threads in a non-Git project directory.
- Do not delete generated artifacts without listing candidate paths and protecting source material.
- Do not treat hidden or ignored local files as safe to copy unless the user confirms.
- Do not accept key work as complete without independent review and evidence.

## Status

Initial public version. The Skill has passed local validation with `quick_validate.py` and includes an internal quality audit checklist.
