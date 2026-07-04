# Harness

Use this phase to make the project durable and restartable. The repository or folder must become the system of record, not the chat history.

## Entry Conditions

- Requirements handoff exists.
- A target project folder is known.

## Allowed Actions

- Inspect existing files before creating or overwriting anything.
- Create missing project state files.
- Record startup, verification, scope, and current status.
- Recommend Git when appropriate and safe; ask before running `git init` in an existing non-Git folder.
- Add `.worktreeinclude` only when ignored local files are required in Codex-managed worktrees.

## Forbidden Actions

- Do not overwrite existing project guidance blindly.
- Do not delete user files.
- Do not implement business features.
- Do not create multiple write threads before the harness exists.

## Standard Files

Prefer this minimal set:

```text
AGENTS.md
CURRENT.md
HANDOFF.md
DECISIONS.md
THREADS.md
TASKS.md
docs/project-brief.md
docs/open-questions.md
docs/requirements.md
docs/commands.md
docs/architecture.md
evaluator-rubric.md
```

If the existing project already uses equivalent files, preserve that convention and map the roles.

Use `references/09-status-schemas.md` for status labels and suggested file sections.

## Git And Local Safety

- If the folder is a Git repo, record branch, dirty state, and whether worktrees can be used.
- If the folder is not a Git repo, recommend `git init` before multi-thread write work, but do not run it without user confirmation.
- Without Git/worktree isolation, allow parallel read-only review but avoid parallel write threads.
- Never treat local-only projects as disposable; require explicit delete/cleanup scope.

## Cleanup Safety

If the user asks to clean old artifacts:

- List candidate paths and sizes first.
- Protect source material, user assets, local secrets, examples, evidence, and legacy reference folders unless explicitly included.
- Prefer deleting generated outputs only when they are ignored or clearly reproducible.
- Record cleanup scope and result in `CURRENT.md` or `HANDOFF.md`.

## Exit Gate

Harness is complete only when a fresh thread can answer:

- What is this project?
- What is the current main direction?
- What is in progress?
- What is the next action?
- How do I run it?
- How do I verify it?
- What must not be touched?

## Harness Handoff

```text
Harness Handoff
- Project Path:
- Project Mainline:
- Git Status:
- Key State Files:
- Startup Commands:
- Verification Commands:
- Scope Locks:
- Forbidden Areas:
- Current WIP:
- Recommended Next Phase: Roadmap
```
