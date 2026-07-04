# Discovery

Use this phase when the project idea is still fuzzy. The goal is not to build; the goal is to make the project discussable and testable.

## Entry Conditions

- The user has a new idea, rough product concept, operational workflow, content system, automation, or Skill idea.
- Target users, first version, input/output, constraints, or success evidence are unclear.

## Allowed Actions

- Ask 4-7 high-leverage questions in one batch.
- Summarize answers into a project brief.
- Mark assumptions explicitly when the user asks Codex to choose defaults.
- Capture open questions instead of blocking indefinitely.

## Forbidden Actions

- Do not write implementation code.
- Do not create child threads, worktrees, or automations.
- Do not choose a heavy technical stack before the user flow and output are clear.
- Do not turn assumptions into confirmed requirements.

## Core Questions

Ask only questions that change the next phase:

- Who is the primary user or operator?
- What painful job should this project do?
- What is the smallest useful first version?
- What are the inputs and outputs?
- What must be out of scope for v1?
- What tools, platforms, accounts, files, APIs, or local constraints matter?
- What evidence proves v1 worked?

## Deliverables

Create or prepare:

```text
docs/project-brief.md
docs/open-questions.md
```

If no project folder exists yet, present the brief in chat and ask where to create the project.

## Exit Gate

Discovery is complete only when these are clear enough to write requirements:

- User/operator.
- Problem.
- MVP.
- Inputs and outputs.
- Non-goals.
- Success evidence.
- Known open questions.

## Discovery Handoff

```text
Discovery Handoff
- Project Name:
- User / Operator:
- Problem:
- MVP:
- Inputs:
- Outputs:
- Non-goals:
- Constraints:
- Success Evidence:
- Open Questions:
- Recommended Next Phase: Requirements
```
