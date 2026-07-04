# Requirements

Use this phase to convert the discovery brief into verifiable requirements and a first end-to-end flow.

## Entry Conditions

- Discovery handoff exists, or the user already provided equivalent detail.
- The project goal is clear enough to define acceptance criteria.

## Allowed Actions

- Write user flows, features, non-goals, and acceptance criteria.
- Define the first vertical slice.
- Define failure states, edge cases, and manual review points.
- Separate confirmed requirements from assumptions.
- Adapt verification to the project type: commands and tests for code, sample outputs for content, dashboards or metrics for operations, and rubric-based review for strategy work.

## Forbidden Actions

- Do not implement features.
- Do not open implementation threads.
- Do not create a large roadmap before the first vertical slice is testable.
- Do not hide unresolved decisions inside vague requirement language.

## Requirement Shape

Each feature should include:

```text
Feature
- ID:
- Title:
- User / Operator Need:
- Input:
- Output:
- Scope:
- Non-goals:
- Acceptance Criteria:
- Verification:
- Dependencies:
- Risks:
- Status:
```

## Deliverables

Create or update:

```text
docs/requirements.md
feature_list.json
evaluator-rubric.md
```

For non-code projects, `feature_list.json` may be replaced by `TASKS.md` if that matches the project better.

## Exit Gate

Requirements are complete only when:

- At least one MVP feature is defined.
- The first end-to-end path is explicit.
- Each MVP feature has observable acceptance criteria.
- Non-goals are explicit.
- Verification evidence is named.
- The user can tell what will not be built or decided in v1.

## Requirements Handoff

```text
Requirements Handoff
- MVP Features:
- First End-to-End Flow:
- Acceptance Criteria:
- Verification Methods:
- Non-goals:
- Risks:
- Assumptions:
- User Decisions Needed:
- Recommended Next Phase: Harness
```
