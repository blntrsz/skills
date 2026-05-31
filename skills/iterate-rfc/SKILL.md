---
name: iterate-rfc
description: Revise an existing initiative RFC after comments, new information, design changes, or switching to an alternative solution. Use when RFC feedback arrives, assumptions change, tradeoffs need revisiting, or the chosen proposal should move to a different alternative.
---

# Iterate RFC

Update an existing `docs/initiatives/<initiative_name>/RFC.md` when feedback or new evidence changes the proposal, rationale, risks, alternatives, or validation plan.

## Workflow

1. **Gather inputs**
   - Locate the RFC from the user's request, branch, or `docs/initiatives/<initiative_name>/RFC.md`. Ask if ambiguous.
   - Read the RFC, the feedback/comments/new information, and any linked PRD, DESIGN, issues, ADRs, or `docs/CONTEXT.md` that affect the decision.
   - If comments reference code behavior, inspect the code instead of guessing.

2. **Classify the change**
   - Clarification: wording or evidence changed, but the proposal stays the same.
   - Scope change: goals, non-goals, affected workflows, or constraints changed.
   - Assumption change: new facts invalidate part of the current rationale.
   - Alternative switch: a previously rejected option is now preferred, or a new option wins.
   - Design drift: implementation/design work exposed a better shape than the RFC described.

3. **Resolve only what is supported**
   - Do not invent decisions. Ask one focused question when the winning direction, business impact, or tradeoff is materially ambiguous.
   - Preserve useful rejected alternatives and explain why they lost.
   - If switching alternatives, move the old proposal into `Alternatives Considered` or `Consequences and Tradeoffs`, then update `Proposal`, `Decision Drivers`, `Validation Plan`, and `Rollout and Migration`.

4. **Patch the RFC**
   - Update the smallest set of sections needed to make the RFC truthful.
   - Add `## Revision History` if absent once the RFC has been materially changed after review.
   - Record what changed, why, and what evidence or comments caused the change.
   - Keep PRD-facing requirements in the PRD and implementation steps in DESIGN/issues; the RFC should remain a decision/tradeoff document.

5. **Flag downstream impact**
   - If the RFC change invalidates DESIGN or issues, name the exact files/sections that need regeneration or edits.
   - Recommend `to-design-doc`, `to-issues`, or `validate-initiative` when appropriate.
   - Offer ADR creation only for decisions that are hard to reverse, surprising without context, and a real tradeoff.

## Revision History Format

```md
## Revision History

- YYYY-MM-DD — <short description of change>. Reason: <feedback, evidence, or decision source>.
```

## Report Completion

Summarize:
- RFC sections changed.
- The decision direction before and after.
- New or resolved open questions.
- Downstream docs or issues that now need updates.

## Guardrails

- Do not turn the RFC into a task list.
- Do not erase rejected alternatives that explain the current decision.
- Do not silently update PRD, DESIGN, issues, or ADRs unless the user explicitly asks; report downstream impact instead.
- Do not ask a sequence of interview questions unless the user requested an interview-style RFC iteration.
