---
name: tdd
description: Test-driven implementation of one approved vertical slice or accepted review fix using red-green-refactor. Use when implementing a spec-backed issue, fixing a bug with known behavior, addressing review-requested changes through tests/refactors, mentions "red-green-refactor", wants integration tests, or asks for test-first development.
license: MIT
---

# Test-Driven Development

## Philosophy

**Core principle**: Tests should verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't.

**Good tests** are integration-style: they exercise real code paths through public APIs. They describe _what_ the system does, not _how_ it does it. A good test reads like a specification - "user can checkout with valid cart" tells you exactly what capability exists. These tests survive refactors because they don't care about internal structure.

**Bad tests** are coupled to implementation. They mock internal collaborators, test private methods, or verify through external means (like querying a database directly instead of using the interface). The warning sign: your test breaks when you refactor, but behavior hasn't changed. If you rename an internal function and tests fail, those tests were testing implementation, not behavior.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Anti-Pattern: Horizontal Slices

**DO NOT write all tests first, then all implementation.** This is "horizontal slicing" - treating RED as "write all tests" and GREEN as "write all code."

This produces **crap tests**:

- Tests written in bulk test _imagined_ behavior, not _actual_ behavior
- You end up testing the _shape_ of things (data structures, function signatures) rather than user-facing behavior
- Tests become insensitive to real changes - they pass when behavior breaks, fail when behavior is fine
- You outrun your headlights, committing to test structure before understanding the implementation

**Correct approach**: Vertical slices via tracer bullets. One approved behavior at a time: RED → GREEN → REFACTOR → repeat. Each test responds to what you learned from the previous cycle. Because you just wrote the code, you know exactly what behavior matters and how to verify it.

```
WRONG (horizontal):
  RED:   test1, test2, test3, test4, test5
  GREEN: impl1, impl2, impl3, impl4, impl5
  REFACTOR: cleanup after all behavior is already piled up

RIGHT (vertical):
  RED→GREEN→REFACTOR: test1→impl1→cleanup1
  RED→GREEN→REFACTOR: test2→impl2→cleanup2
  RED→GREEN→REFACTOR: test3→impl3→cleanup3
  ...
```

## Stepping-Stone Tests

Stepping-stone tests are allowed when they help you reach the intended behavior or learn the shape of the problem. They are temporary scaffolding, not automatically part of the final suite.

Before finishing, re-check every added or changed test. Keep tests that specify durable behavior through a public interface. Delete or consolidate tests that are useless, low-value, overlapping, redundant with higher-value coverage, or coupled to implementation details.

A smaller valuable suite is better than preserving every test written during the loop.

## Workflow

### 1. Pin the Approved Slice and Spec

Implement one user-approved vertical slice at a time. PRD/TRD/BDD specs, bug reports, review findings, and PR context are supporting references, not substitutes for slice approval. If only high-level specs exist, route back to `/to-issues` or ask the user to explicitly approve one vertical slice or bug-fix behavior before coding.

When exploring the codebase, use the project's domain glossary so that test names and interface vocabulary match the project's language, and respect ADRs in the area you're touching.

Before writing any code:

- [ ] Locate and read the approved issue/slice, PRD/TRD, BDD feature files, bug report, accepted review findings/requested changes, or PR context.
- [ ] Map the approved slice or accepted review fix to observable behaviors from the spec; treat BDD scenarios as the behavioral contract when present.
- [ ] For review findings, classify each accepted fix as either a behavior-preserving refactor or a spec-backed behavior change before entering the loop.
- [ ] Identify the public interface under test and confirm whether it actually needs to change.
- [ ] Choose the first tracer-bullet behavior from the approved slice, prioritizing the critical path or highest-risk logic.
- [ ] Identify opportunities for [deep modules](deep-modules.md) (small interface, deep implementation).
- [ ] Design interfaces for [testability](interface-design.md) without expanding the public surface unnecessarily.
- [ ] Present the behavior plan below and get user approval before coding.

Behavior plan template:

- **Approved target**: issue/slice ID, bug report, or accepted review finding.
- **Source refs**: PRD/TRD sections, BDD scenarios, review report lines, bug reproduction notes, or code/docs inspected.
- **Ordered behaviors/fixes**: the smallest sequence to implement, one behavior or refactor at a time.
- **Public interface under test**: API, CLI, UI flow, command, event, or other observable surface.
- **First tracer bullet**: the first failing test/check and why it proves the path.
- **Non-goals**: nearby behavior intentionally excluded.
- **Ambiguities/deviations needing approval**: contradictions, interface changes, or scope choices not already approved.

Ask the user only when:

- No approved issue/slice, accepted review finding, or approved bug-fix behavior can be found.
- The spec contradicts itself or conflicts with the current code.
- The target public interface or required interface change is ambiguous.
- Behavior priority is unclear after reading the spec.

**You can't test everything.** Focus testing effort on the approved slice's critical path and complex logic. Do not add speculative edge cases beyond the spec unless the user approves them.

### 2. Tracer-Bullet Cycle

Write ONE test that confirms ONE spec-backed behavior through the public interface:

```
RED:      Write focused test for first behavior → run it → fail for the expected reason
GREEN:    Write minimal code to pass → run it → pass
REFACTOR: Improve structure while green → rerun the focused test(s)
```

This is your tracer bullet - proves the path works end-to-end and keeps design clean before adding the next behavior.

### 3. Incremental Red-Green-Refactor Loop

For each remaining approved behavior:

```
RED:      Write next focused behavior test → run it → fail for the expected reason
GREEN:    Minimal code to pass → run it → pass
REFACTOR: Apply [refactor candidates](refactoring.md) while green → rerun relevant tests
```

Rules:

- One behavior at a time.
- Only enough code to pass the current test.
- Don't anticipate future tests or unapproved scope.
- Keep tests focused on observable behavior from the spec.
- After each GREEN, immediately look for duplication, shallow modules, awkward boundaries, or interface cleanup before moving on.
- **Never refactor while RED.** Get to GREEN first.

### 4. Final Verification

After every approved behavior has completed RED → GREEN → REFACTOR:

- [ ] Run the relevant full test suite or the project's requested checks.
- [ ] Re-read the issue/spec/BDD scenarios and confirm the slice is covered.
- [ ] Review every added or changed test; delete or consolidate stepping-stone, useless, low-value, overlapping, or implementation-detail tests.
- [ ] Summarize implemented behaviors, tests added, tests deleted, and any approved deviations or follow-up gaps.

## Checklist Per Cycle

```
[ ] Behavior or refactor traces back to the approved issue/slice, accepted review finding, or BDD scenario
[ ] Test describes behavior, not implementation
[ ] Test uses public interface only
[ ] Test would survive internal refactor
[ ] Code is minimal for this test
[ ] Refactor step completed while GREEN
[ ] Any remaining stepping-stone tests are valuable and behavior-focused
[ ] No speculative features added
```
