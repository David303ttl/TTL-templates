<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Documentation Scaffold

This repository uses the documentation layout defined by the RE schema. The files here are the reusable scaffold for Stage 1 outputs and Stage 2 consumption.

## Current split

- Stage 1 produces evidence and closeout artifacts.
- Stage 2 consumes those artifacts and turns them into validation, gating, and planning work.

## Scaffold vs Examples

- This folder is the reusable scaffold, not a finished project instantiation.
- Example project docs, if present, belong in `examples/` or in a downstream project repository.

## Current structure

```text
docs/
  00-project-overview.md
  01-implementation-plan.md
  DOCUMENTATION_STRUCTURE.md
  reference/
    architecture/
      07-clock-and-timing-map.md
      10-coupling-map.md
    process/
      trace-plan.md
      risk-register.md
      e2e-trace-matrix.md
      file-manifest.md
      stage2-risk-resolution-plan.md
      q0-static-analysis.md
      q1-solid-review.md
      q2-bug-catalog.md
      q3-race-condition-audit.md
      q4-refactoring-plan.md
      q5-test-strategy.md
      phase-r10-evidence/
        README.md
        R-10-q0-handoff.md
        R-10-verification-report.md
```

## Notes

- The Stage 1 master remains observational.
- The Stage 2 plan is the first entry-control document.
- Open questions stay inside the risk register until they are promoted into Stage 2 work items.

