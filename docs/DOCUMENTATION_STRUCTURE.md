<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Documentation Structure

## 1. Purpose

This file records how the docs tree is organized and how the major artifacts relate to each other.

## 2. Top-Level Flow

```text
Stage 1
  -> evidence archives
  -> risk register
  -> trace plan
  -> trace matrix
  -> coupling map
  -> timing map
  -> Q-0 handoff

Stage 2
  -> stage2-risk-resolution-plan.md
  -> q0-static-analysis.md
  -> q1-solid-review.md
  -> q2-bug-catalog.md
  -> q3-race-condition-audit.md
  -> q4-refactoring-plan.md
  -> q5-test-strategy.md
  -> validation tasks
  -> entry gates
  -> future-work readiness decisions
```

## 3. Authority Rules

- Stage 1 documents should stay evidence-focused and observational.
- Stage 2 documents should stay decision-focused and action-oriented.
- The risk register is the primary cross-phase handoff artifact.

## 4. Current Scaffold

- `docs/00-project-overview.md`
- `docs/01-implementation-plan.md`
- `docs/reference/architecture/07-clock-and-timing-map.md`
- `docs/reference/architecture/10-coupling-map.md`
- `docs/reference/process/risk-register.md`
- `docs/reference/process/trace-plan.md`
- `docs/reference/process/e2e-trace-matrix.md`
- `docs/reference/process/file-manifest.md`
- `docs/reference/process/stage2-risk-resolution-plan.md`
- `docs/reference/process/q0-static-analysis.md`
- `docs/reference/process/q1-solid-review.md`
- `docs/reference/process/q2-bug-catalog.md`
- `docs/reference/process/q3-race-condition-audit.md`
- `docs/reference/process/q4-refactoring-plan.md`
- `docs/reference/process/q5-test-strategy.md`
- `docs/reference/process/phase-r10-evidence/`

