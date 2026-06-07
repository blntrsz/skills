---
name: iterate-spec
description: Iterates existing PRD, TRD, and BDD specs from feedback, changed decisions, open questions, or doc drift by running a focused grill-with-docs clarification loop, then regenerating the specs with to-spec. Use when the user mentions /iterate-spec, asks to improve or update specs/docs from feedback, or provides spec-review decisions, review comments, annotations, PR comments, critique, or doc-drift notes about existing PRD/TRD/BDD specs.
---

# Iterate Spec

## Quick start

Given current specs plus feedback, changed decisions, open questions, or doc drift, resolve the inputs through a focused grilling session, then update the specs and directly related docs.

This is a spec-feedback iteration skill, not an initial spec-review generator. If no feedback source exists, ask for one or ask whether the user wants `/spec-review` or another review pass before iterating.

Example triggers:
- “/iterate-spec docs/specs/checkout.md with this feedback…”
- “Iterate the PRD/TRD from these review comments.”
- “Use the annotations to improve the feature files.”
- “Apply this spec-review decision log.”
- “Fix this doc drift between the BDD and PRD.”

## Workflow

1. Identify inputs
   - Find the current spec files: PRD, TRD, BDD feature files, or the document named by the user.
   - Find the feedback source: pasted notes, `/spec-review` decision logs, changed decisions, open questions, doc-drift notes, review comments, annotations, PR comments, issue comments, or the current conversation.
   - Do not treat the absence of feedback as implicit approval or as a request to invent feedback.
   - If either source is ambiguous, ask one clarifying question before proceeding.

2. Read the existing context
   - Read the target specs and feedback in full.
   - Inspect relevant `docs/CONTEXT.md`, `docs/CONTEXT-MAP.md`, and ADRs when present.
   - Explore the codebase instead of asking when the answer is discoverable from code or docs.

3. Normalize feedback
   - Group duplicates and near-duplicates.
   - Separate actionable issues from praise, taste comments, and out-of-scope requests.
   - Treat doc drift as feedback: identify which document is stale, which source should win, and what must change to make PRD/TRD/BDD/domain docs consistent.
   - Produce a short working list of feedback themes, contradictions, missing decisions, doc-drift fixes, and spec-quality gaps.

4. Reiterate the plan with grill-with-docs
   - Load and follow [grill-with-docs](../grill-with-docs/SKILL.md).
   - Ask one question at a time and include your recommended answer.
   - Focus on unresolved feedback, changed decisions, open questions, doc drift, and spec-quality gaps; do not re-litigate settled decisions unless feedback contradicts them.
   - For each actionable theme, drive to one of: accepted, rejected with rationale, deferred with owner/condition, or answered by existing docs/code.
   - Update `CONTEXT.md` and offer ADRs exactly as `grill-with-docs` instructs when terminology or durable decisions crystallise.
   - Continue until every actionable feedback theme is resolved and the user agrees the revised plan is stable.

5. Regenerate specs with to-spec
   - Load and follow [to-spec](../to-spec/SKILL.md).
   - Infer the spec category from the target spec files by default: BDD-only → Small feature; TRD-only → Small technical task; PRD+BDD → Standard feature; PRD+TRD+BDD → Full initiative.
   - Ask the `to-spec` category question only when the target spec set is ambiguous, missing, or feedback explicitly requests adding or removing PRD/TRD/BDD outputs.
   - Synthesize from the resolved plan; do not start a second broad interview.
   - Update existing spec files in place when possible, preserving useful structure and deleting obsolete text.
   - Update directly related docs when the feedback is doc drift, but do not broaden into general documentation cleanup.
   - Keep BDD feature files as the source of truth for behavior.

## Quality gates

Before finishing, verify:

- A real feedback source was identified, or the user explicitly chose to continue after a separate review/source clarification.
- Every actionable feedback theme, changed decision, and doc-drift item is reflected in the specs/docs, explicitly rejected, or explicitly deferred.
- The specs do not contradict `CONTEXT.md`, ADRs, observed code, or each other.
- Terminology is consistent with the project glossary.
- PRDs stay concise and under 100 lines when generated.
- BDD scenarios contain concrete acceptance criteria and no implementation-only details.
- TRDs include the implementation decisions needed to code safely.

## Output

Report:

- Feedback sources and spec files inspected.
- Key decisions made during the grill session.
- Files changed.
- Traceability: feedback theme / changed decision / doc-drift item → decision → spec/doc location or reason not changed.
- Remaining open questions, if any.
