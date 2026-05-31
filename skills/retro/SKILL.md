---
name: retro
description: Turn feedback from a plan, PR review, code review, or user correction into durable learnings that prevent repeated mistakes. Use when the user asks to retro, learn from feedback, capture review comments, update working rules, or avoid the same issue next time.
---

# Retro

Capture actionable lessons from feedback and apply them to the agent's future behavior.

## When triggered

Use this skill when feedback appears in any of these forms:

- User comments on a plan, spec, implementation, or pull request.
- PR/code-review comments, Plannotator annotations, or reviewer notes.
- A rejected proposal, failed implementation, or repeated correction.
- Requests like "retro this", "learn from this", "don't do this again", "update your rules", or "why did this happen?".

## Workflow

1. **Collect the source material**
   - If given a PR URL, review comments, annotations, or plan file, read the relevant content first.
   - If feedback is ambiguous, ask one clarifying question before writing rules.
   - Separate explicit user preferences from one-off project constraints.

2. **Extract only important learnings**
   Keep lessons simple: one bullet per learning, only for issues likely to recur.
   Each bullet should say: when this applies, what to do next time, and a concrete example or refactor opportunity when useful.

3. **Store the lesson explicitly**
   Learnings produced by the work should be committed with the same PR whenever practical, not left as chat-only memory or postponed until after merge.

   Produce the first possible durable output in this order:
   - **Lint rule**: add or update a project lint rule when the lesson can be enforced by the existing linter or formatter.
   - **ast-grep rule**: add or update an ast-grep rule when the lesson is structural code guidance that lint cannot easily express.
   - **`docs/LEARNINGS.md`**: append important learning here when it cannot be enforced mechanically.
   - Never store durable lessons only in the chat transcript.

4. **Patch the smallest durable artifact**
   - Add concise, imperative guidance.
   - Avoid vague lessons like "be more careful"; rewrite as observable behavior.
   - Preserve existing style and structure.
   - Treat `docs/LEARNINGS.md` as the single learnings file; do not create other learnings files.
   - If updating `docs/LEARNINGS.md`, keep it as a bullet-pointed list and keep the file under 200 lines.

5. **Report the retro**
   Summarize:
   - Lessons captured.
   - Files updated.
   - Any feedback intentionally not codified and why.

## Learning quality bar

A good `docs/LEARNINGS.md` bullet is:

- Important enough to change future behavior.
- Specific about when it applies.
- Actionable about what to do next time.
- Supported by an example or refactor opportunity when that helps.

Use this simple format:

```md
- When <context>, <do this next time>. Example: <short example>. Refactor opportunity: <optional>.
```

## Common patterns

- If review caught style issues: prefer a lint rule.
- If review caught unsafe or unwanted code structure: prefer an ast-grep rule.
- If the agent overbuilt: add a `docs/LEARNINGS.md` bullet to confirm scope before adding abstractions, unless this can be detected mechanically.
- If the agent missed existing code: add a `docs/LEARNINGS.md` bullet to search/read before designing, unless this can be detected mechanically.
- If the plan ignored constraints: add a `docs/LEARNINGS.md` bullet to list assumptions and verify them, unless this can be detected mechanically.
- If feedback exposes a recurring design problem: add a refactor opportunity to `docs/LEARNINGS.md`.
- If feedback needs clarification: include a concise example in `docs/LEARNINGS.md`.
- If feedback is a nitpick or one-off preference: do not add it to `docs/LEARNINGS.md`.
- If feedback conflicts with existing guidance: ask the user which rule wins before editing.

## Guardrails

- Do not claim future memory was updated unless an actual durable file was changed and named.
- Do not store secrets, private credentials, or sensitive personal data in lessons.
- Do not rewrite broad instructions from a single example without confirming scope.
- Do not add nitpicks to `docs/LEARNINGS.md`; focus on important learning.
- Do not let `docs/LEARNINGS.md` exceed 200 lines; consolidate or remove lower-value bullets first.
- Do not add redundant rules if an existing rule already covers the lesson; tighten it instead.
