---
name: spec-review
description: Runs an AI-assisted human review of existing PRD/TRD/BDD specs by mapping decision areas, challenging one decision at a time, and producing a decision log. Use when specs should be reviewed before slicing, when user asks to review specs interactively, or when design decisions need to be accepted, changed, or left open for later discussion.
---

# Spec Review

Review existing specs through an interactive decision interview. This is review-only: do not rewrite specs. Produce a decision log for `/iterate-spec`.

## Quick start
1. Read PRD, TRD, BDD, `CONTEXT.md`/`CONTEXT-MAP.md`, ADRs, and relevant code/docs when needed.
2. Give a high-level map of decision areas and recommended review order.
3. Drill into one decision at a time.
4. Stop after each decision and ask the user to choose: keep current, choose another option, or leave open.
5. Finish with kept decisions, changed decisions, and open questions.

## Workflow

### 1. Identify inputs
- Find target PRD, TRD, and BDD feature files from the request or repo conventions.
- If target specs are ambiguous, ask one clarifying question.
- Read domain docs and ADRs so terminology and durable decisions match project language.
- If a factual question can be answered from code or docs, inspect those instead of asking.

### 2. Build the review map
Before drilling in, summarize:
- product/behavior goal
- technical approach
- main BDD behaviors
- glossary/ADR constraints
- decision areas worth reviewing
- recommended review order, prioritizing behavior, implementation safety, and issue slicing

Look especially for:
- implicit decisions hidden in prose
- PRD ↔ BDD or TRD ↔ BDD mismatches
- vague or conflicting terminology
- unresolved edge cases that affect implementation
- data/state/API/contract decisions
- rollout, migration, compatibility, or observability choices when relevant
- gaps that would make `/to-issues` or `/tdd` guess

### 3. Interview one decision at a time
For each decision, present:
- **Decision area**: short name.
- **Current decision**: what the specs appear to say now, with file references.
- **Why it matters**: downstream behavior, implementation, slicing, or review risk.
- **Options**:
  - **A. Keep current decision** — explain consequences.
  - **B/C/etc. Other possible decisions** — realistic alternatives with trade-offs.
  - **Open question** — leave for later discussion; say whether it blocks slicing or implementation.
- **Recommendation**: your recommended option and why.

Ask which option the user chooses. Ask one decision at a time and wait.

### 4. Record each answer
Classify each response as:
- **Keep current**: no spec feedback needed unless wording should be clarified.
- **Change**: create feedback for `/iterate-spec` with old decision, chosen decision, affected specs, and rationale.
- **Open question**: record the question, options, owner/context if known, and whether it blocks `/to-issues` or `/tdd`.

If a chosen option conflicts with `CONTEXT.md`, ADRs, or observed code, call out the conflict immediately and ask whether to revisit it or mark it as feedback for `/iterate-spec`.

## Output
```md
## Spec Review Decision Log

### Specs and context inspected
- ...

### Kept Current Decisions
- Decision — rationale / references.

### Changed Decisions / Feedback for iterate-spec
1. Current spec says ...
   Chosen decision: ...
   Update: PRD/TRD/BDD/doc drift as needed.
   Rationale: ...

### Open Questions
1. Question ...
   Options discussed: ...
   Blocks: slicing / implementation / neither.

### Recommended next step
Run `/iterate-spec` with changed decisions and resolved open questions, or proceed to `/to-issues` if there are no blocking changes/questions.
```

## Quality gates
- High-risk or ambiguous decision areas were reviewed, intentionally skipped, or left open.
- Changed decisions are concrete enough for `/iterate-spec` to apply.
- Open questions say whether they block `/to-issues` or `/tdd`.
- No specs were edited during review.
- Terminology conflicts with `CONTEXT.md`, ADRs, or code were surfaced.
