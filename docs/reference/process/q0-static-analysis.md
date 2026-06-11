<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Q-0 Static Analysis

> Document ID: `PROJECT-Q0-01`
> Phase: Q-0
> Status: Draft
> Paired Master: `RE-schema/RE-STAGE2-QUALITY-AUDIT-MASTER.md`

## 1. Purpose

Perform static analysis on the Stage 1 outputs, classify unresolved items, and assemble the Q-0 backlog.

## 2. Inputs

- `docs/reference/process/risk-register.md`
- `docs/reference/process/trace-plan.md`
- `docs/reference/process/e2e-trace-matrix.md`
- `docs/reference/architecture/10-coupling-map.md`
- `docs/reference/architecture/07-clock-and-timing-map.md`
- `docs/reference/process/file-manifest.md`
- `docs/reference/process/phase-r10-evidence/R-10-q0-handoff.md`

## 3. Analysis Focus

| Target | Evidence Source | Output |
|---|---|---|
| Coupling hotspots | Coupling map | Static-analysis notes and blast-radius candidates |
| Timing unknowns | Timing map | Measurement candidates or acceptance candidates |
| Trace gaps | Trace matrix | Blocked-trace list or test requirements |
| Open questions | Risk register | Disposition table and backlog items |

## 4. Outputs

- Q-0 backlog
- disposition table
- static-analysis summary
- candidate gates

## 5. Exit Criteria

- Every open item has a disposition.
- The initial Q-0 backlog is traceable to Stage 1 evidence.
- Any blocked or unknown item has a follow-up action.

## 6. Revision History

| Version | Date | Notes |
|---|---|---|


