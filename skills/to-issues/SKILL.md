---
name: to-issues
description: Break PRD/TRD/BDD specs, plans, or approved source context into independently grabbable vertical tracer-bullet issues. Use when the user asks to slice work into issues; iterate until the breakdown is approved before publishing or implementation.
---

Break a plan or spec into independently-grabbable issues using vertical slices (tracer bullets). Each issue is a thin vertical slice that cuts through all relevant integration layers end-to-end, not a horizontal slice of one layer.

Each slice delivers a narrow but complete path through every relevant layer (for example schema, API, UI, tests). A completed slice is demoable or verifiable on its own. Prefer many thin slices over a few thick ones.

## Workflow

1. Read the source material
   - Read the named plan, PRD, TRD, BDD feature files, issue, or conversation context in full.
   - When BDD exists, treat feature scenarios as the behavioral contract and map slices back to them.
   - Inspect relevant `docs/CONTEXT.md`, ADRs, and nearby code when needed to understand boundaries or dependencies.
   - If the source material is missing, contradictory, or too vague to slice, ask one clarifying question before producing issues.

2. Slice vertically
   - Each issue should be independently implementable by one agent/developer.
   - Each issue should be independently reviewable and demonstrable.
   - Avoid horizontal tasks such as “add schema,” “build UI,” “write tests,” or “wire API” unless they are explicitly non-user-facing technical work and still verifiable on their own.
   - Prefer dependency-light tracer bullets that prove one end-to-end behavior before broadening coverage.

3. Present the proposed breakdown as a numbered list. For each slice, show:
   - **Title**: short descriptive name.
   - **Goal / observable behavior**: what users, callers, or operators can see when the slice is complete.
   - **Spec references**: PRD/TRD sections, BDD feature/scenario names, bug report, or source notes this slice covers.
   - **Acceptance criteria**: concrete checks that make the slice done.
   - **Validation plan**: expected test, command, manual flow, or verification evidence for the slice.
   - **Touched layers**: schema/data, domain logic, API, UI, CLI, tests, docs, migrations, etc. Include only relevant layers.
   - **Blocked by**: which other slices, if any, must complete first.
   - **Non-goals**: nearby behavior intentionally excluded from this slice.
   - **Risks / open questions**: anything that needs confirmation before or during implementation.
   - **User stories covered**: which user stories this addresses, if the source material has them.

4. Ask the user:

   - Does the granularity feel right? (too coarse / too fine)
   - Are the dependency relationships correct?
   - Should any slices be merged or split further?
   - Are any acceptance criteria or non-goals wrong?

5. Iterate until the user approves the breakdown.

6. When the user approves the breakdown, publish the issues into the user's issue tracker. When an issue tracker is not specified, create markdown files for each issue in `docs/issues/*.md`.

## Quality gates

Before publishing, verify:

- Every issue is a vertical, demoable or verifiable slice unless it is explicitly a technical-only task.
- BDD scenarios and PRD/TRD requirements from the source material are covered or intentionally deferred.
- Each issue has acceptance criteria and a validation plan.
- Dependencies are minimal and explicit.
- No issue requires speculative behavior outside the approved spec.
