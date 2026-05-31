---
name: to-rfc
description: Turn the current conversation context and codebase understanding into an initiative RFC at docs/initiatives/<initiative_name>/RFC.md. Use when user wants to create an RFC from the current context, document a proposal, or capture tradeoffs and decision rationale.
---

# To RFC

Create `docs/initiatives/<initiative_name>/RFC.md` from the current conversation context and repository vocabulary.

Use `iterate-rfc` instead when an existing RFC needs to change after comments, new information, design drift, or switching to a different alternative.

Do **not** interview the user by default. Synthesize from what is already known. Ask only if the RFC would otherwise assert a materially ambiguous or risky decision.

## Workflow

1. **Understand the context**
   - Review the conversation for the proposal, motivation, constraints, decisions, alternatives, and explicitly excluded behavior.
   - Explore the repo enough to understand the domain, existing terminology, architecture vocabulary, and affected user or developer workflows.
   - Read relevant docs, specs, `docs/CONTEXT.md`, and ADRs in `docs/adr/` if they exist.
   - Use the project's own vocabulary throughout the RFC.

2. **Identify decision seams**
   - Identify the highest-level behavior, interface, or workflow seams affected by the proposal.
   - Prefer existing extension points, workflows, acceptance-test seams, E2E seams, or integration seams over new seams.
   - Keep the seam discussion focused on decisions and observable effects rather than implementation minutiae.
   - If the user explicitly asked for confirmation, briefly check that these seams match expectations before writing. Otherwise continue without interviewing.

3. **Write `docs/initiatives/<initiative_name>/RFC.md`**
   - Infer `<initiative_name>` from the proposal name when obvious; ask one clarifying question if it is ambiguous.
   - Create or update `docs/initiatives/<initiative_name>/RFC.md` unless the user specifies another path.
   - Focus on the proposal, context, goals, non-goals, tradeoffs, alternatives, risks, rollout, and decision rationale.
   - Include technical details only when they are necessary to explain the decision or tradeoff.
   - Do **not** publish to an issue tracker or apply issue labels.

4. **Report completion**
   - Summarize the RFC sections written.
   - Name the file updated.
   - Mention assumptions made because the current context was incomplete.

## RFC Template

```md
# RFC: <Proposal Name>

## Summary
Briefly state the proposed change and why it matters.

## Context
Describe the current situation, relevant background, and forces motivating the proposal.

## Goals
- List the outcomes this proposal is intended to achieve.

## Non-Goals
- List outcomes, use cases, or changes that are intentionally out of scope.

## Proposal
Describe the proposed approach at the level needed for stakeholders to understand the decision.

## Decision Drivers
- Capture the principles, constraints, product needs, technical forces, or business needs shaping the decision.

## Alternatives Considered
### <Alternative Name>
- Description: <what it is>
- Pros: <why it is attractive>
- Cons: <why it was not chosen or remains uncertain>

## Consequences and Tradeoffs
- Describe expected benefits, costs, limitations, operational effects, and future constraints.

## Validation Plan
- Describe how the team will know the proposal works.
- Prefer the highest-level existing behavioral or integration seam available.
- Good validation checks observable outcomes and decision assumptions, not incidental implementation details.

## Rollout and Migration
- Describe adoption, transition, compatibility, and rollback considerations if relevant.

## Open Questions
- List unresolved decisions or assumptions that need follow-up.
```

## Guidance

A good RFC:

- explains why a decision is needed now;
- records the tradeoffs, not just the preferred answer;
- distinguishes goals from non-goals;
- names risks and open questions honestly;
- uses repository terminology and respects existing ADRs;
- avoids volatile file paths, code snippets, and implementation detail unless needed to communicate a decision precisely.

## Guardrails

- Do not create GitHub issues, tickets, or tracker items unless the user separately asks for that.
- Do not apply triage labels.
- Do not invent unstated decisions; mark uncertain points as assumptions or open questions.
- Do not turn the RFC into a task list or implementation plan unless the user requests that format.
- Do not ask a sequence of interview questions unless the user explicitly requests an interview-style RFC process.
