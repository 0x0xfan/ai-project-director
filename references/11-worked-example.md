# Worked Example

Use this only when a user or agent needs a concrete example of how the workflow should feel in practice.

## User Request

```text
I want to build a local tool that turns product research notes into short video scripts.
I am not sure what files or features are needed yet.
```

## Intake

```text
Intake Summary
- Route: NEW_PROJECT
- Project Name: product-script-tool
- Project Path: N/A until user chooses one
- Existing Files: N/A
- Git Status: N/A
- Existing State Files: N/A
- User Goal: convert research notes into reusable short-video scripts
- Immediate Constraint: requirements and acceptance criteria are unclear
- Recommended First Phase: Discovery
```

Do not scaffold or code yet. Ask only for the minimum missing routing facts, such as target user, desired first output, source material type, and where the local project should live.

## Discovery Handoff Shape

```text
Handoff Packet
- From Phase: Discovery
- To Phase: Requirements
- Current Decision: Build a local workflow tool for turning product research notes into short video scripts.
- Accepted Artifacts:
  - Target user: the project owner and future AI agents.
  - MVP: paste or load one product research note and produce one script draft.
  - Output: Chinese short-video script with hook, body, CTA, and material notes.
  - Non-goals: no account login, no cloud publishing, no batch generation in MVP.
- Rejected or Superseded Artifacts: N/A
- Open Questions:
  - Which script style profile should MVP use first?
  - Should inputs be Markdown files, pasted text, or folders?
- Scope Lock: one input note to one script draft.
- Allowed Next Actions: write requirements and acceptance criteria.
- Forbidden Actions: do not build UI, batch pipeline, or automation yet.
- Verification Required: user can inspect one generated script against the agreed structure.
- Active Threads: N/A
- Blockers: input format not chosen
- User Decision Needed: choose first input format
```

## Requirements Handoff Shape

```text
Handoff Packet
- From Phase: Requirements
- To Phase: Harness
- Current Decision: MVP uses Markdown input files and outputs Markdown script drafts.
- Accepted Artifacts:
  - Feature: parse one Markdown research note.
  - Feature: generate one script draft with hook, body, CTA, and material notes.
  - Acceptance: output includes all required sections and preserves key product facts.
  - Acceptance: no fabricated price, platform rule, or unavailable material claim.
- Rejected or Superseded Artifacts: pasted-text-only input
- Open Questions: exact folder path
- Scope Lock: local Markdown in, local Markdown out.
- Allowed Next Actions: create project harness files after user confirms path.
- Forbidden Actions: do not implement generator logic until harness exists.
- Verification Required: sample note -> sample script output can be manually checked.
- Active Threads: N/A
- Blockers: project path not confirmed
- User Decision Needed: confirm project path
```

## Roadmap Example

```text
Roadmap Stage D0: document input/output contract and script rubric.
Roadmap Stage D1: create first runnable note-to-script vertical slice.
Roadmap Stage D2: add style profiles and folder batch input.
Roadmap Stage D3: polish output quality and error handling.
Roadmap Stage D4: final verification evidence and handoff cleanup.
```

The example is intentionally small. A real project may stop earlier, route to `SINGLE_TASK`, or expand into orchestration only after task cards, allowed files, forbidden files, and verification are explicit.
