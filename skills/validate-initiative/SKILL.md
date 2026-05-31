---
name: validate-initiative
description: Validate an initiative for documentation drift across PRD, RFC, DESIGN, issues, context, ADRs, and implementation evidence. Use when checking doc drift before review, after implementation, after review fixes, or before merge.
---

# Validate Initiative

Validate that an initiative's documents still agree with each other and with the implementation evidence. This is a doc-drift gate, not a test runner, code review, or merge-readiness authority.

## Workflow

1. **Locate the initiative**
   - Infer `docs/initiatives/<initiative_name>/` from the user's request, branch name, changed files, or nearby docs. Ask one clarifying question if ambiguous.
   - Read `PRD.md`, `RFC.md`, `DESIGN.md`, issue files under `issues/`, `REVIEW.md` and `reviews/` if present, `VALIDATION.md` if present, plus relevant `docs/CONTEXT.md` and ADRs.

2. **Check document consistency**
   - PRD acceptance criteria should trace to design sections and issue acceptance criteria.
   - RFC decisions should trace to design choices; rejected alternatives should remain recorded if the direction changed.
   - Issues should have clear status, blockers, acceptance criteria, validation, and parent document links.
   - Open questions must be either resolved, explicitly accepted as remaining risk, or blocking.
   - Vocabulary should match `docs/CONTEXT.md`; new durable terms should be flagged for glossary updates.
   - New hard-to-reverse or surprising tradeoffs should be flagged for ADR consideration.

3. **Check implementation evidence for drift only**
   - Inspect the current diff, named branch/commit/PR, or issue implementation evidence enough to determine whether implemented behavior contradicts PRD/RFC/DESIGN/issues.
   - Do not review code quality; use `review` for that.
   - Do not determine whether accepted review findings were fixed; use review artifacts and follow-up review for that.
   - Flag drift where implementation changed the RFC/DESIGN/issue assumptions.

4. **Do not run verification commands**
   - Do not run tests, lint, typecheck, formatters, builds, package installs, or other verification commands.
   - If existing evidence is present in issue files, review docs, CI notes, or prior validation notes, record that evidence as context only.
   - If verification evidence is missing or stale, flag that as a follow-up for TDD/implementation or the project CI, not as a validation command to run here.

5. **Write `docs/initiatives/<initiative_name>/VALIDATION.md`**
   - Create or update the validation report unless the user asks for read-only output.
   - Keep it factual: doc consistency, implementation-evidence drift, blockers, and recommended document updates.
   - Do not silently rewrite PRD/RFC/DESIGN/issues; recommend exact updates unless the user asked this skill to patch them.

## VALIDATION.md Template

```md
# Validation: <Initiative Name>

## Scope
- Initiative: <path>
- Target: <diff, branch, commit, PR, or issue files inspected>

## Result
Pass | Blocked | Needs follow-up

## Document Consistency
- PRD → DESIGN → issues traceability: <summary>
- RFC decisions reflected in design: <summary>
- Open questions/blockers: <summary>
- Drift requiring doc updates: <summary>

## Existing Verification Evidence
- Evidence found in issues, review docs, CI notes, or prior validation notes: <summary>
- Missing or stale evidence that may matter: <summary>

## Review Artifact Context
- Review docs inspected for doc-drift context: <paths>
- Findings that appear to imply doc drift: <summary>

## Doc-Drift Readiness
- [ ] No blocking open questions remain in the docs
- [ ] PRD, RFC, DESIGN, and issues agree with each other
- [ ] Docs match implemented behavior or drift is explicitly accepted
- [ ] Required doc updates are listed as follow-ups

## Follow-ups
- <doc update, ADR, test, or issue needed>
```

## Guardrails

- Do not treat this as code review; use `review` for strict maintainability findings.
- Do not run verification commands; this skill checks documentation drift only.
- Do not make merge-readiness claims beyond doc-drift readiness.
- Do not invent missing decisions; mark them as blockers or follow-ups.
- Tiny fixes may use a lightweight doc-drift summary when the user intentionally skipped initiative docs.
