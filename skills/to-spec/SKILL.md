---
name: to-spec
description: Turn a resolved or clarified plan into PRD, TRD, and/or BDD feature specs. Use when the user asks to capture agreed context as specs; use iterate-spec for feedback on existing specs.
license: MIT
---

This skill takes the current conversation context and codebase understanding and produces the requested spec document(s). It captures resolved context; it is not a second broad interview.

Use `PRD.md` and `TRD.md` templates from this skill directory when generating those documents.

## Before writing

1. Gather the resolved context
   - Use the current conversation, prior clarification notes, named plan/spec files, and relevant codebase understanding.
   - Read relevant `docs/CONTEXT.md`, `docs/CONTEXT-MAP.md`, and ADRs when present so terminology and decisions match the project language.
   - If a fact is discoverable from code or docs, inspect it instead of asking the user.

2. Choose the spec category
   - Ask one question: "Based on our conversation, I'd recommend (1) Small feature, (2) Small technical task, (3) Standard feature, or (4) Full initiative. Which do you prefer?" — picking the category that best fits the context.
   - If the conversation is ambiguous, briefly explain your reasoning in one sentence alongside the recommendation.

3. Handle gaps without reopening planning
   - After the user answers, do NOT continue interviewing the user broadly — synthesize what you already know.
   - If an unresolved ambiguity would make the spec misleading, ask one focused clarification question or recommend returning to `/grill-with-docs` before writing.
   - Do not invent requirements, decisions, user stories, or edge cases that were not resolved in the source context.

## Spec categories

| # | Category | PRD | TRD | BDD | When |
|---|---|---|---|---|---|
| 1 | Small feature | — | — | ✓ | User-facing change, well-understood scope, BDD captures it fully |
| 2 | Small technical task | — | ✓ | — | Infra, refactoring, tooling, CI, internal APIs — no user-facing behavior to spec |
| 3 | Standard feature | ✓ | — | ✓ | User-facing feature needing a PRD for context, BDD carries the detail |
| 4 | Full initiative | ✓ | ✓ | ✓ | Major effort needing proposal, design rationale, and implementation detail before coding |

## BDD is the source of truth for behavior

Captures actual capabilities and acceptance criteria in `features/<domain>/*.feature` files using Gherkin, organized by domain. If a domain grows unwieldy, slice it further: `features/<domain>/<subdomain>/*.feature`. These should be written so they can later become executable tests when the implementation exists.

BDD scenario quality:

- Use concrete `Given / When / Then` acceptance behavior, not implementation steps.
- Keep each scenario focused on one observable capability or rule.
- Cover the critical happy path and resolved edge cases; do not add speculative edge cases.
- Use terminology from `CONTEXT.md` when a glossary exists.
- Avoid UI, database, API, or internal implementation details unless they are the behavior under specification.

## Document roles

- **PRD** (under 100 lines): What we want to build and why. Concise introduction — not the source of truth for detailed behavior.
- **TRD** (Technical Requirements Document): Proposes and designs the implementation — covers background, proposal, goals, design, data model, flows, testing, rollout, alternatives, open questions. Use whichever sections are relevant; not every section is mandatory.
- **BDD** (`features/<domain>/*.feature`): Canonical behavioral spec. The definitive answer for what the system does.

## Quality gates

Before finishing, verify:

- The generated specs use project glossary terms consistently and do not contradict ADRs or observed code.
- PRDs stay under 100 lines and avoid detailed behavior that belongs in BDD.
- BDD scenarios contain concrete acceptance criteria and no implementation-only details.
- TRDs include the implementation decisions needed to code safely, plus non-goals and open questions when relevant.
- Any remaining open question is explicit and not silently filled in by assumption.

## Output

Report:

- Spec category chosen and why.
- Source context files/docs inspected.
- Files created or updated.
- Remaining open questions or assumptions, if any.
