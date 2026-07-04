# Adversarial Review Gate

Use this phase whenever a child thread claims completion or a significant artifact is ready to accept.

## Entry Conditions

- Candidate work exists.
- Acceptance criteria and verification surface exist.
- The implementation thread has produced a report, diff, artifact, or evidence.

## Allowed Actions

- Open an independent review thread when thread tools are available.
- Review diff, files, generated artifacts, commands, screenshots, logs, and reports.
- Search for counterexamples, missing edge cases, old assumptions, and evidence gaps.
- Return a strict verdict.
- If independent review tools are unavailable, run an adversarial review in the current thread only under the same-thread constraints below.

## Same-Thread Review Constraints

When `Review Independence` is `same-thread`, `ACCEPTED` requires all of:

- At least 3 distinct counterexample attempts recorded in `Counterexamples`, even if all are rejected.
- At least 1 reproducible external evidence item inspected, such as a diff, log file, screenshot, test output, command output, or generated artifact.
- A note explaining why independent review was not possible.

If any requirement is unmet, the verdict must be `NEEDS_REPAIR`, `NEEDS_USER_DECISION`, or `INVALID_REVIEW`; never `ACCEPTED`.

## Forbidden Actions

- Do not let the implementation thread self-certify important work.
- Do not accept "looks good" without evidence.
- Do not repair during review unless the review task explicitly includes repair.
- Do not broaden the task to make it pass.
- Do not treat an unverifiable criterion as passed.

## Review Checklist

Check:

- Does the result satisfy every acceptance criterion?
- Was verification actually run or inspected?
- Are there old terms, superseded decisions, or stale mainline assumptions?
- Did the work touch forbidden files?
- Are there boundary cases or failure paths missing?
- Did the implementation silently guess data, schema, or business decisions?
- Is the evidence reproducible by another thread?
- Did it preserve unrelated user changes?

## Verdicts

Use only:

```text
ACCEPTED
NEEDS_REPAIR
BLOCKED
NEEDS_USER_DECISION
INVALID_REVIEW
```

Use `INVALID_REVIEW` when the review itself did not inspect evidence, exceeded scope, or repaired instead of reviewing.

## Review Report

```text
Adversarial Review Report
- Task ID:
- Reviewed Thread:
- Review Independence: independent-thread | same-thread | manual
- Verdict:
- Evidence Reviewed:
- Passed Criteria:
- Failed Criteria:
- Unverifiable Criteria:
- Counterexamples:
- Forbidden Scope Check:
- Regression Risk:
- Required Repair:
- User Decision Needed:
```

## Exit Gate

- `ACCEPTED` may advance to merge, next stage, or final handoff.
- `NEEDS_REPAIR` must create a repair task scoped to failed criteria only.
- `BLOCKED` must name the missing input, tool, environment, or contradiction.
- `NEEDS_USER_DECISION` must ask a concrete question and stop expansion.
