---
name: review
description: Run an extremely strict code-quality review of a diff, PR, branch, or selected files with prioritized findings. Use when the user asks to review code, review a PR, audit current changes, or produce docs/initiatives/<initiative_name>/REVIEW.md for local findings.
---

# Review

Perform a strict maintainability-first code review. Working behavior is not enough: reject changes that make the codebase structurally worse, preserve avoidable complexity, or miss an obvious simplification.

## Review target

- For local review, default to the current diff against the base branch or merge-base when discoverable.
- If the user names files, a branch, a commit, or a PR, review that target instead.
- Read surrounding code for context, but do not comment on unrelated pre-existing issues unless the change worsens them.
- For PR-style review, anchor findings to exact `file:line` where possible. For cross-cutting issues, use the best representative anchor and mention related files.
- For local review, overwrite `docs/initiatives/<initiative_name>/REVIEW.md` with the findings. Infer `<initiative_name>` from the current branch, initiative docs, or user request when obvious; ask if ambiguous.

## Hard guardrails

- Read-only review: do not edit source files as part of review.
- Do not run tests, lint, typecheck, formatters, builds, package installs, or other verification commands. Assume the author has already verified correctness.
- Do not auto-fix anything.
- Do not flood the review with cosmetic nits. Prefer fewer, high-conviction, actionable comments.
- Checks line should say: `Checks not run by design.`

## What to look for aggressively

Prioritize findings in this order:

1. Structural code-quality regressions.
2. Missed opportunities for dramatic simplification / code-judo restructuring.
3. Spaghetti growth: ad-hoc conditionals, flags, nullable modes, and special cases inserted into busy flows.
4. Boundary, abstraction, and type-contract problems that make the code harder to reason about.
5. File-size and decomposition concerns.
6. Modularity, canonical-layer, and helper-reuse issues.
7. Legibility and maintainability concerns.

Ask these questions for every meaningful change:

- Is there a code-judo move that would delete whole branches, helpers, modes, conditionals, or layers?
- Can the change be reframed so fewer concepts are needed?
- Did a cohesive module become more coupled, stateful, or hard to scan?
- Is logic living in the right file, package, module, or layer?
- Did the diff add repeated conditionals that indicate a missing model, policy, dispatcher, or helper?
- Is an abstraction earning its keep, or is it a pass-through wrapper?
- Did the diff introduce `any`, `unknown`, casts, optionality, or silent fallback that obscures the real invariant?
- Did it duplicate a canonical helper or invent a bespoke one-off?
- Is orchestration unnecessarily sequential or non-atomic when a cleaner structure is obvious?

## Approval bar

Treat these as presumptive `critical` or `high` findings unless the author has a strong justification:

- A plausible code-judo move would delete substantial incidental complexity.
- The PR pushes a file from below 1000 lines to above 1000 lines.
- New ad-hoc branching makes an existing flow more tangled.
- Feature checks are scattered across shared code instead of isolated behind an appropriate abstraction.
- A wrapper, generic mechanism, cast-heavy contract, or magical fallback makes simple behavior more indirect.
- Logic lands outside the canonical owning layer or duplicates an existing helper.

Prefer remedies that delete complexity: collapse duplicate branches, move ownership to the right boundary, replace condition chains with an explicit model or dispatcher, split giant files, remove unhelpful wrappers, make type boundaries explicit, reuse canonical helpers, and make related updates atomic.

## Priority scale

Use exactly these priorities:

- `critical` — blocker: correctness, security, data-loss, or severe maintainability regression that should not merge.
- `high` — serious issue: structural regression, obvious missed simplification, large-file threshold breach, boundary leak, or risky design debt.
- `medium` — worthwhile maintainability, testability, edge-case, or design issue.
- `low` — minor but genuinely actionable clarity or maintainability improvement. Avoid style-only nits.

## Output format

For PR-style review, output review comments only:

```md
## Summary
[1-3 sentences on overall review result]

## Findings

### critical
- `path/to/file.ext:123` — Title
  Rationale. Suggested fix.

### high
- ...

### medium
- ...

### low
- ...

## Checks
Checks not run by design.
```

For local review, overwrite `docs/initiatives/<initiative_name>/REVIEW.md` using the same format, with a short header naming the reviewed target/diff. If there are no findings, write/say `No review findings` and include what was inspected plus the checks line.

## Learning loop

Do not capture lessons during normal review. If the user explicitly asks to retro, capture lessons, or learn from review feedback, follow the Retro workflow: store recurring lessons durably in the first applicable place, preferring lint rules, then ast-grep rules, then `docs/LEARNINGS.md`. Do not claim future memory was updated unless a durable file was actually changed.
