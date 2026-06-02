---
name: to-issues
description: Break a plan, spec, or PRD into independently-grabbable issues
---

Break a plan into independently-grabbable issues using vertical slices (tracer bullets). Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests) - A completed slice is demoable or verifiable on its own - Prefer many thin slices over few thick ones

Present the proposed breakdown as a numbered list. For each slice, show:

- Title: short descriptive name
- Blocked by: which other slices (if any) must complete first
- User stories covered: which user stories this addresses (if the source material has them)

Ask the user:

Does the granularity feel right? (too coarse / too fine)
Are the dependency relationships correct?
Should any slices be merged or split further?

Iterate until the user approves the breakdown.

When approves the breakdown, publish the issues into the user's issue tracker. When issue tracker is not specified, create markdown files for each issue in `docs/issues/*.md`.

