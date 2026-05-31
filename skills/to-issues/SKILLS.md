---
name: to-issues
description: Break initiative DESIGN.md, PRD.md, and RFC.md into local independently-grabbable issue files under docs/initiatives/<initiative_name>/issues/. Use when user wants to convert a design, PRD, RFC, plan, or spec into local implementation issues.
---

# To Issues

Break `docs/initiatives/<initiative_name>/DESIGN.md` into independently-grabbable local issues using vertical slices (tracer bullets), with the initiative `PRD.md` and `RFC.md` as source context.

Do **not** publish to an issue tracker. Do **not** apply labels. Create local markdown issue files only.

## Workflow

1. **Gather context**
   - Locate and read `docs/initiatives/<initiative_name>/DESIGN.md`, `docs/initiatives/<initiative_name>/PRD.md`, and `docs/initiatives/<initiative_name>/RFC.md` unless the user provides different paths. Infer `<initiative_name>` when obvious; ask if ambiguous.
   - Treat `DESIGN.md` as the primary implementation breakdown source.
   - Use `PRD.md` for product requirements, user stories, and acceptance criteria.
   - Use `RFC.md` for decision rationale, constraints, alternatives, and tradeoffs.
   - Preserve unresolved questions instead of inventing answers.

2. **Explore the codebase**
   - Explore enough to understand the current architecture, product vocabulary from `docs/CONTEXT.md`, existing seams, and relevant ADRs in `docs/adr/`.
   - Issue titles and descriptions should use the project's domain vocabulary.
   - Prefer existing architecture and test seams when describing validation.

3. **Draft vertical slices**
   - Break the work into tracer-bullet issues.
   - Each issue should deliver a narrow but complete end-to-end path through the affected product or system behavior.
   - A completed slice must be demoable, testable, or verifiable on its own.
   - Prefer many thin slices over a few thick slices.
   - Slices may be `HITL` or `AFK`: `HITL` requires human interaction, review, or a decision; `AFK` can be implemented and merged without human interaction. Prefer `AFK` where possible.

4. **Review the breakdown**
   - Present the proposed breakdown as a numbered list before writing files.
   - For each slice, show title, type, blockers, and covered PRD stories or BDD scenarios.
   - Ask whether the granularity, dependencies, and HITL/AFK classifications are correct.
   - Iterate until the user approves, unless the user explicitly asks to skip review.

5. **Create local issues**
   - Create one markdown file per approved slice under `docs/initiatives/<initiative_name>/issues/` unless the user specifies another directory.
   - Use dependency order in filenames so blockers come first.
   - Use stable, readable filenames such as `0001-short-kebab-title.md`.
   - Reference local blocker filenames in each issue.
   - Do not create, close, or modify remote tracker issues.

## Local Issue Template

```md
# <Issue Title>

## Type
AFK | HITL

## Parent Documents
- PRD: <path>
- RFC: <path>
- Design: <path>

## What to build
A concise description of this vertical slice. Describe end-to-end behavior, not a layer-by-layer task list.

Avoid stale details such as exact file paths or large code snippets. Include a concise type shape, state machine, or pseudocode only if it captures an important design decision more clearly than prose.

## User Stories / Scenarios Covered
- <PRD user story or BDD scenario>

## Acceptance Criteria
- [ ] <observable criterion>
- [ ] <observable criterion>
- [ ] <observable criterion>

## Validation
- Describe the highest-level existing seam that should verify this slice.
- Mention lower-level tests only where they add distinct confidence.

## Blocked By
- None - can start immediately

## Notes
- Include relevant constraints, assumptions, risks, or open questions.
```

## Guardrails

- Do not publish to GitHub Issues or any other issue tracker unless the user separately asks for that.
- Do not apply labels, triage states, milestones, or assignees.
- Do not create horizontal layer-only issues such as "add schema" or "build UI" unless that is the smallest independently verifiable slice.
- Do not modify parent documents unless the user asks.
- Do not invent missing PRD, RFC, or design decisions; record gaps as notes or HITL issues.
