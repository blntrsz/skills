---
name: to-prd
description: Turn the current conversation context and codebase understanding into a business-focused initiative PRD at docs/initiatives/<initiative_name>/PRD.md. Use when user wants to create a PRD from the current context, capture product requirements, or convert discussion into BDD acceptance criteria.
---

# To PRD

Create `docs/initiatives/<initiative_name>/PRD.md` from the current conversation context and repository product vocabulary.

Do **not** interview the user by default. Synthesize from what is already known. Ask only if the PRD would otherwise assert a materially ambiguous or risky business requirement.

## Workflow

1. **Understand the context**
   - Review the conversation for the user's goal, desired outcomes, constraints, and explicitly excluded behavior.
   - Explore the repo enough to understand the product domain, user-facing concepts, and existing terminology.
   - Read relevant product docs, `docs/CONTEXT.md`, specs, and ADRs in `docs/adr/` if they exist.
   - Use the project's domain vocabulary in the PRD.

2. **Identify behavioral seams**
   - Identify the highest-level user-visible behaviors that should be validated.
   - Prefer existing acceptance-test, E2E, integration, or feature-spec seams over new seams.
   - Keep seam discussion business-facing: describe observable behavior, not implementation details.
   - If the user explicitly asked for confirmation, briefly check that these behavioral seams match expectations before writing. Otherwise continue without interviewing.

3. **Write `docs/initiatives/<initiative_name>/PRD.md`**
   - Infer `<initiative_name>` from the feature/product name when obvious; ask one clarifying question if it is ambiguous.
   - Create or update `docs/initiatives/<initiative_name>/PRD.md` unless the user specifies another path.
   - Focus on business requirements, user value, user-visible behavior, and acceptance criteria.
   - Do **not** include low-level technical requirements, file paths, code snippets, module names, database schemas, API contracts, or implementation plans.
   - Do **not** publish to an issue tracker or apply issue labels.

4. **Report completion**
   - Summarize the PRD sections written.
   - Name the file updated.
   - Mention assumptions made because the current context was incomplete.

## PRD Template

```md
# PRD: <Product/Feature Name>

## Problem Statement
Describe the problem from the user's or business actor's perspective.

## Solution
Describe the desired product outcome from the user's perspective.

## User Stories
1. As a <business actor>, I want <capability>, so that <business benefit>.

## BDD Acceptance Criteria

### Scenario: <User-visible behavior>
Given <business context>
When <user or system action occurs>
Then <observable business outcome occurs>
And <additional observable outcome, if needed>

## Business Rules and Decisions
- Capture business decisions, product rules, constraints, and policy choices.
- Avoid technical implementation requirements.

## Testing Approach
- Describe the user-visible behaviors that should be validated.
- Prefer the highest-level existing behavioral seam available in the product.
- Good tests verify outcomes users can observe, not implementation details.

## Out of Scope
- List behaviors, audiences, workflows, or outcomes that are explicitly excluded.

## Further Notes
- Include assumptions, open business questions, risks, or context that future readers need.
```

## BDD Guidance

Use `Given / When / Then` scenarios for acceptance criteria. Good scenarios describe business context and observable outcomes, use product vocabulary, are understandable by non-engineering stakeholders, and avoid implementation details such as classes, functions, endpoints, tables, or files.

```gherkin
Scenario: Create a PRD from existing conversation context
Given the conversation contains the user's product goal and constraints
When the to-prd workflow is used
Then docs/initiatives/<initiative_name>/PRD.md is written with business-focused requirements
And the PRD includes BDD acceptance criteria
And the PRD does not include an issue-tracker publishing step
```
