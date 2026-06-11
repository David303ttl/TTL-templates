<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Implementation Plan

> Status: Draft

## 1. Stage 1

Stage 1 discovers and records the system.

Outputs:

- trace plan
- risk register
- open questions
- E2E trace matrix
- coupling map
- timing map
- Q-0 handoff
- Stage 2 entry-control plan
- Q-0 through Q-5 reference docs

## 2. Stage 2

Stage 2 consumes Stage 1 outputs and decides what happens next.

Outputs:

- validated blockers
- design constraints
- test requirements
- hardware-validation tasks
- accepted risks
- safe future-work path

## 3. Current Scaffold Work

| Order | Task | Output |
|---|---|---|
| 1 | Create the reference tree | `docs/reference/architecture/` and `docs/reference/process/` |
| 2 | Seed Stage 1 output artifacts | `trace-plan.md`, `risk-register.md`, `e2e-trace-matrix.md`, `file-manifest.md`, `10-coupling-map.md`, `07-clock-and-timing-map.md`, `phase-r10-evidence/` |
| 3 | Add Stage 2 entry control and Q-phase scaffold | `RE-STAGE2-QUALITY-AUDIT-MASTER.md`, `stage2-risk-resolution-plan.md`, `R-10-q0-handoff.md`, `q0-static-analysis.md`, `q1-solid-review.md`, `q2-bug-catalog.md`, `q3-race-condition-audit.md`, `q4-refactoring-plan.md`, `q5-test-strategy.md` |
| 4 | Add project-specific playbooks and reference docs | Future project content |

## 4. Open Dependency

The document set stays generic until a target codebase is selected.

