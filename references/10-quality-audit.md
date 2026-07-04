# Quality Audit

Use this file to audit the project workflow or this skill itself after creation or substantial changes.

## Audit Targets

Check the workflow against these failure modes:

- Phase leakage: later-phase work starts before earlier handoff exists.
- Hidden assumptions: user decisions are silently converted into requirements.
- Missing evidence: completion is claimed without verification.
- Self-certification: implementation thread accepts its own work.
- Unbounded automation: heartbeat or goal can keep expanding scope.
- Unsafe parallelism: multiple write threads can touch the same files.
- Tool hallucination: workflow assumes unavailable thread, automation, worktree, or goal tools.
- Local data risk: cleanup, secrets, ignored files, or user assets are not protected.
- Non-code mismatch: workflow assumes software project when user has operations/content/business project.
- Resume failure: a fresh thread cannot continue from files alone.

## Audit Procedure

1. Read `SKILL.md`.
2. Read only the references relevant to the requested audit.
3. Build a phase-by-phase checklist.
4. Try to break each gate with a realistic bad scenario.
5. Patch the smallest instruction that prevents the failure.
6. Run the skill validator.

## Minimum Acceptance Criteria

The skill or workflow passes only if:

- Trigger description says when to use it.
- Each phase has entry conditions, allowed actions, forbidden actions, deliverables, and exit gate.
- Status labels are consistent.
- Automation has pause and stop rules.
- Multi-thread execution has isolation rules.
- Review gate cannot be satisfied by the same implementation context alone.
- Non-code projects can still use the workflow.
- Tool-unavailable fallbacks exist.

## Audit Report

```text
Quality Audit Report
- Target:
- Files Reviewed:
- Verdict: ACCEPTED | NEEDS_REPAIR | BLOCKED
- Critical Findings:
- Important Findings:
- Minor Findings:
- Repairs Applied:
- Validator Result:
- Remaining Risk:
```
