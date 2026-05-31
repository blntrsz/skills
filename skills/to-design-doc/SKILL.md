---
name: to-design-doc
description: Generate an initiative DESIGN at docs/initiatives/<initiative_name>/DESIGN.md from existing PRD.md and RFC.md documents plus codebase context. Use when user wants to convert product requirements and an RFC into an actionable design document.
---

# To Design Doc

Create `docs/initiatives/<initiative_name>/DESIGN.md` from the initiative `PRD.md`, `RFC.md`, and repository context. The PRD owns user requirements, the RFC owns the proposed change with tradeoffs and alternatives, and the DESIGN owns how to implement that direction.

Do **not** interview the user by default. Synthesize from the source documents and codebase. Ask only if the design would otherwise depend on a materially ambiguous or risky decision not covered by the PRD or RFC.

## Workflow

1. **Read source documents**
   - Locate and read `docs/initiatives/<initiative_name>/PRD.md` and `docs/initiatives/<initiative_name>/RFC.md` unless the user provides different paths. Infer `<initiative_name>` when obvious; ask if ambiguous.
   - Treat the PRD as the source of product requirements, user-visible behavior, and acceptance criteria.
   - Treat the RFC as the source of proposal rationale, constraints, tradeoffs, alternatives, and decisions.
   - Preserve open questions from either document instead of inventing answers.

2. **Explore implementation context**
   - Explore the repo enough to understand existing architecture, domain vocabulary, test seams, extension points, and relevant conventions.
   - Read nearby docs, `docs/CONTEXT.md`, ADRs in `docs/adr/`, specs, and examples if they exist.
   - Prefer existing patterns and seams over new abstractions.

3. **Write `docs/initiatives/<initiative_name>/DESIGN.md`**
   - Create or update `docs/initiatives/<initiative_name>/DESIGN.md` unless the user specifies another path.
   - Translate requirements and RFC decisions into an implementation-oriented design.
   - Include enough technical detail for an implementation agent to proceed, while avoiding unnecessary code dumps.
   - Keep product behavior traceable back to PRD acceptance criteria and RFC decisions.

4. **Report completion**
   - Summarize the major design areas covered.
   - Name the file updated.
   - Mention assumptions, unresolved questions, or source-document gaps.

## DESIGN.md Template

```md
# Design: <Feature or Proposal Name>

## Overview
Summarize the design and the PRD/RFC goals it satisfies.

## Source Documents
- PRD: <path or title>
- RFC: <path or title>

## Requirements Traceability
- Map key PRD requirements or BDD scenarios to the design areas that satisfy them.
- Map key RFC decisions to the design choices that implement them.

## Current System Context
Describe the relevant existing architecture, workflows, domain concepts, and constraints.

## Proposed Design
Describe the target design, responsibilities, boundaries, and user-visible behavior.

## Interfaces and Contracts
Describe affected public interfaces, commands, events, data shapes, configuration, or integration contracts.
Avoid volatile low-level detail unless needed for implementation clarity.

## Data and State
Describe important state, lifecycle, persistence, validation, and migration concerns.

## Error Handling and Edge Cases
Describe failure modes, recovery behavior, empty states, permissions, and degraded experiences.

## Testing Strategy
Describe how to validate the design using the highest-level existing seams available.
Include unit, integration, acceptance, or E2E coverage only where each adds distinct value.

## Rollout and Migration
Describe migration, compatibility, release, observability, and rollback considerations.

## Risks and Tradeoffs
List important risks, mitigations, and tradeoffs inherited from the RFC or discovered in the codebase.

## Open Questions
List unresolved questions without inventing answers.
```

## Guidance

A good design doc:

- is traceable to the PRD and RFC;
- explains how the system should change and why;
- uses repository vocabulary and respects existing ADRs;
- prefers existing architecture, seams, and conventions;
- gives implementers enough direction without becoming a full code listing;
- separates confirmed decisions from assumptions and open questions.

## Guardrails

- Do not create GitHub issues, tickets, or tracker items unless the user separately asks for that.
- Do not apply triage labels.
- Do not ignore PRD or RFC conflicts; call them out as open questions or risks.
- Do not invent product requirements or RFC decisions.
- Do not include large code snippets; use concise shapes or pseudocode only when they clarify a decision.
- Do not ask a sequence of interview questions unless the user explicitly requests an interview-style design process.
