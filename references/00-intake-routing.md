# Intake Routing

Use this phase before choosing a workflow. The goal is to identify what kind of request this is and avoid starting the wrong machinery.

## Classify The Request

Choose exactly one primary route:

- `NEW_PROJECT`: user has an idea but no stable project folder or requirements.
- `EXISTING_PROJECT`: project folder exists but state, requirements, or next action are unclear.
- `RESUME_PROJECT`: project has state files, previous threads, or paused automation.
- `SINGLE_TASK`: user wants one bounded task, not a full project workflow.
- `PROJECT_AUDIT`: user wants to inspect, review, or improve a project workflow.

If multiple routes apply, choose the earliest missing lifecycle phase. Example: an existing folder with no requirements goes to Requirements or Harness before Orchestration.

## Required Intake Facts

Collect only what changes routing:

```text
Intake Summary
- Route:
- Project Name:
- Project Path:
- Existing Files:
- Git Status:
- Existing State Files:
- User Goal:
- Immediate Constraint:
- Recommended First Phase:
```

## Tool And Environment Checks

Before promising orchestration, check what is actually available:

- Thread tools: can create/read/send messages to threads?
- Automation tools: can create/update heartbeat automation?
- Git/worktree: is the project a Git repo and safe for worktrees?
- Local commands: are startup and verification commands known?

If a tool is unavailable, provide a fallback plan instead of pretending the workflow can run automatically.

## Safety Defaults

- Ask before initializing Git in an existing non-Git folder.
- Ask before deleting or archiving files.
- Ask before enabling unattended automation.
- For non-Git projects, allow parallel read-only review but keep write work single-threaded.
- Treat hidden, ignored, or local-secret files as sensitive unless the user says otherwise.

## Exit Gate

Intake is complete when the next phase is justified and the user has provided or approved the target project path.

## Intake Handoff

```text
Intake Handoff
- Route:
- Project Path:
- Existing State:
- Missing State:
- Tool Availability:
- Safety Constraints:
- First Valid Phase:
- User Confirmation Needed:
```
