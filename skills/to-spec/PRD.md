# Title of the PRD

<!--
Keep the PRD under 100 lines.

The PRD is a concise introduction: what we want to build and why it matters.
It is NOT the source of truth for detailed system capabilities or acceptance criteria.
-->

## Problem

<!-- The problem that the user is facing, from the user's perspective. -->

## Why Now

<!-- Why this matters now: user pain, opportunity, risk, timing, or strategic context. -->

## Solution

<!-- The proposed solution, from the user's perspective. -->

## Intended Outcomes

<!-- The measurable or observable outcomes we expect if this works. -->

## BDD Source of Truth

<!--
Detailed capabilities, user journeys, acceptance criteria, and edge cases belong in `features/<domain>/*.feature` files, not in this PRD.

The `.feature` files are the canonical behavioral specification for what the system can do. Write them in Gherkin so they can later become executable tests when the implementation exists. Organize by domain; slice further into subdirectories if a domain gets large.

Use the PRD to orient readers; use BDD to define behavior.
-->
