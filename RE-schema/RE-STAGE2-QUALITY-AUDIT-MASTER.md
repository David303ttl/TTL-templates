<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# RE-STAGE2-QUALITY-AUDIT-MASTER — Stage 2 Risk Resolution and Quality Audit Master Schema

> **Schema ID:** `RE-STAGE2-QUALITY-AUDIT-MASTER`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Purpose:** Reusable Stage 2 framework for embedded synthesizer and audio firmware. This document defines how Stage 2 consumes Stage 1 evidence, resolves risk and question backlogs, and produces the safe path to future work. Stage 2 is decision- and validation-oriented: it does not authorize code changes by itself.
>
> **Layered structure:**
> - Layer 1: Core Stage 2 Schema (Q-0 through Q-5)
> - Layer 2: Entry-Control and Decision Rules
> - Layer 3: Project / Future-Work Overlays
>
> **Primary input artifacts:**
> - `docs/reference/process/risk-register.md`
> - `docs/reference/process/trace-plan.md`
> - `docs/reference/process/e2e-trace-matrix.md`
> - `docs/reference/architecture/10-coupling-map.md`
> - `docs/reference/architecture/07-clock-and-timing-map.md`
> - `docs/reference/process/file-manifest.md`
> - `docs/reference/process/phase-r10-evidence/R-10-q0-handoff.md`
> - `docs/reference/process/stage2-risk-resolution-plan.md`

---

## Contents

1. [Philosophy and Design Principles](#1-philosophy-and-design-principles)
2. [Stage 2 Input Contract](#2-stage-2-input-contract)
3. [Standard Q-Phase Sequence](#3-standard-q-phase-sequence)
4. [Phase Definitions (Q-0 through Q-2)](#4-phase-definitions-q-0-through-q-2)
5. [Phase Definitions (Q-3 through Q-5)](#5-phase-definitions-q-3-through-q-5)
6. [Resolution and Disposition Schema](#6-resolution-and-disposition-schema)
7. [Playbook Schema Reference](#7-playbook-schema-reference)
8. [Documentation Structure](#8-documentation-structure)
9. [Required Stage 2 Artifacts](#9-required-stage-2-artifacts)
10. [Handoff and Entry Rules](#10-handoff-and-entry-rules)
11. [Cross-Phase Tracking Schema](#11-cross-phase-tracking-schema)
12. [Stage Boundary Rules](#12-stage-boundary-rules)
13. [Revision History](#13-revision-history)

---

## 1. Philosophy and Design Principles

### 1.1 Core Philosophy

Stage 2 turns Stage 1 observations into engineering decisions.

Stage 1 answers:

> "What does the firmware do, what is coupled to what, and what is not yet known?"

Stage 2 answers:

> "What must be resolved, validated, accepted, or gated before future work begins?"

Stage 2 is not implementation. It is the decision layer that makes implementation safe.

### 1.2 Design Principles

| # | Principle | Description |
|---|-----------|-------------|
| 1 | **No code changes** | Stage 2 produces no code changes. It audits, classifies, validates, and plans only. |
| 2 | **Evidence-first** | Every decision must reference Stage 1 artifacts or direct validation evidence. |
| 3 | **Disposition required** | Every open item must be assigned a disposition: blocker, constraint, test, validation, accept, or out of scope. |
| 4 | **Entry-controlled** | Q-0 may not start until the R-10 handoff package is complete. |
| 5 | **Traceable decisions** | Every risk, question, and coupling hotspot must be cross-referenced to evidence. |
| 6 | **Safe future work only** | The final output of Stage 2 is a safe path for future work, not a refactor itself. |
| 7 | **Stage 1 remains observational** | Stage 1 remains read-only and evidence-focused; Stage 2 consumes its outputs. |
| 8 | **Decision records are authoritative** | The Stage 2 plan, disposition tables, and readiness summary become the decision authority for downstream work. |

### 1.3 Disposition Model

| Disposition | Meaning | Stage 2 Action |
|---|---|---|
| **Blocker** | Work is unsafe until resolved | Create an investigation task and gate the affected path |
| **Design Constraint** | Must shape the solution | Carry forward into the design record |
| **Test Requirement** | Needs verification | Convert to a test or measurement task |
| **Hardware Validation Needed** | Static analysis is insufficient | Create a device-level validation task |
| **Accept / Monitor** | Known and acceptable for now | Record the acceptance rule and monitoring trigger |
| **Out of Scope** | Not relevant to the current objective | Archive with justification |

---

## 2. Stage 2 Input Contract

### 2.1 Required Inputs

| Input Artifact | Purpose | Required? |
|---|---|---:|
| `docs/reference/process/risk-register.md` | Authoritative list of open risks and open questions | Yes |
| `docs/reference/process/trace-plan.md` | Defines required traces and evidence expectations | Yes |
| `docs/reference/process/e2e-trace-matrix.md` | Trace completeness and gap detection | Yes |
| `docs/reference/architecture/10-coupling-map.md` | Blast radius and coupling hotspots | Yes |
| `docs/reference/architecture/07-clock-and-timing-map.md` | Timing, rate-domain, and callback constraints | Yes |
| `docs/reference/process/file-manifest.md` | Repository inventory and phase mapping | Yes |
| `docs/reference/process/phase-r10-evidence/R-10-q0-handoff.md` | The explicit R-10 to Q-0 handoff package | Yes |
| `docs/reference/process/stage2-risk-resolution-plan.md` | Entry-control backlog and gate definition | Yes |

### 2.2 Input Validation Rules

1. The risk register must contain all open items discovered during Stage 1 closeout.
2. The coupling map must be complete enough to support Q-0 and Q-4 decisions.
3. The E2E trace matrix must identify any blocked or static-only traces.
4. The timing map must identify unresolved timing risks and unverified budgets.
5. The file manifest must be current enough to anchor the path and provenance checks used by Q-0.

### 2.3 Input Precedence

If two Stage 1 artifacts conflict, the order of authority is:

1. Evidence-backed source files and line references
2. The R-10 evidence archive
3. The risk register
4. The coupling map and timing map
5. The trace matrix
6. The file manifest

Conflicts must be recorded, not silently resolved.

---

## 3. Standard Q-Phase Sequence

### 3.1 Phase Dependency Graph

```
R-10 closeout
  └──► Stage 2 entry control
        └──► Q-0 (Static Analysis and Entry Control)
              └──► Q-1 (SOLID and Coupling Review)
                    └──► Q-2 (Bug Catalog and Tech Debt Triage)
                          └──► Q-3 (Race Condition and Concurrency Audit)
                                └──► Q-4 (Refactoring Plan)
                                      └──► Q-5 (Test Strategy and Verification Plan)
```

### 3.2 Phase Table

| Phase | Name | Goal | Mandatory? | Output Doc |
|---|---|---|:---:|---|
| **Q-0** | Static Analysis and Entry Control | Classify unresolved items, identify coupling hotspots, and produce the Q-0 backlog | Yes | `docs/reference/process/q0-static-analysis.md` |
| **Q-1** | SOLID and Coupling Review | Identify SRP/OCP/DIP issues, listener chains, and architecture violations | Yes | `docs/reference/process/q1-solid-review.md` |
| **Q-2** | Bug Catalog and Tech Debt Triage | Convert medium/high items into actionable bugs and debt records | Yes | `docs/reference/process/q2-bug-catalog.md` |
| **Q-3** | Race Condition and Concurrency Audit | Audit ISR/shared-state, DMA, buffers, and temporal hazards | Yes | `docs/reference/process/q3-race-condition-audit.md` |
| **Q-4** | Refactoring Plan | Define safe change order, dependencies, and blast-radius constraints | Yes | `docs/reference/process/q4-refactoring-plan.md` |
| **Q-5** | Test Strategy and Verification Plan | Convert risks and constraints into verification coverage and acceptance criteria | Yes | `docs/reference/process/q5-test-strategy.md` |

---

## 4. Phase Definitions (Q-0 through Q-2)

### 4.1 Q-0: Static Analysis and Entry Control

**Goal:** Consume the Stage 1 handoff package, classify all unresolved items, and identify static-analysis outputs that later Q-phases require.

**Consumes:**

- risk register
- open questions
- coupling map
- trace matrix
- timing map
- file manifest
- R-10 handoff package

**Produces:**

- entry-control backlog
- disposition table
- coupling hotspot summary
- unresolved-item map
- Q-0 reference doc at `docs/reference/process/q0-static-analysis.md`

### 4.2 Q-1: SOLID and Coupling Review

**Goal:** Review structural design quality using the Stage 1 coupling and trace data.

**Produces:**

- SOLID violation candidates
- listener and temporal coupling findings
- accidental-vs-architectural coupling notes
- Q-1 reference doc at `docs/reference/process/q1-solid-review.md`

### 4.3 Q-2: Bug Catalog and Tech Debt Triage

**Goal:** Convert meaningful unresolved items into a tracked bug or technical-debt catalog.

**Produces:**

- prioritized bug list
- tech debt backlog
- acceptance candidates
- Q-2 reference doc at `docs/reference/process/q2-bug-catalog.md`

---

## 5. Phase Definitions (Q-3 through Q-5)

### 5.1 Q-3: Race Condition and Concurrency Audit

**Goal:** Audit shared state, ISR/main-loop interactions, DMA ownership, and timing-sensitive hazards.

**Produces:**

- race condition candidates
- buffer ownership clarifications
- synchronization gaps
- Q-3 reference doc at `docs/reference/process/q3-race-condition-audit.md`

### 5.2 Q-4: Refactoring Plan

**Goal:** Define a safe future change order using coupling, blast radius, and dependency information.

**Produces:**

- refactoring sequence
- blast-radius table
- safe-change ordering
- preconditions for implementation
- Q-4 reference doc at `docs/reference/process/q4-refactoring-plan.md`

### 5.3 Q-5: Test Strategy and Verification Plan

**Goal:** Turn Stage 1 and Stage 2 findings into a test and verification plan.

**Produces:**

- verification strategy
- coverage mapping
- acceptance criteria
- runtime or hardware validation requirements
- Q-5 reference doc at `docs/reference/process/q5-test-strategy.md`

---

## 6. Resolution and Disposition Schema

### 6.1 Stage 2 Disposition Workflow

```
Open item
  └──► classify disposition
        ├──► blocker -> investigation task + gate
        ├──► design constraint -> design record
        ├──► test requirement -> test task
        ├──► hardware validation needed -> measurement task
        ├──► accept / monitor -> acceptance record
        └──► out of scope -> archive with justification
```

### 6.2 Resolution Table

| Item Type | Typical Source | Output |
|---|---|---|
| Risk | Risk register | Gate, mitigation, or acceptance decision |
| Open question | Risk register open question section | Validation task or documented answer |
| Coupling hotspot | Coupling map | Refactoring constraint or safe change order input |
| Timing unknown | Timing map | Measurement task or static-only acceptance |
| Trace gap | E2E trace matrix | Test requirement or blocked-trace record |

### 6.3 Acceptance Rule

An item may be accepted only if the acceptance record states:

1. Why the risk is acceptable.
2. What evidence supports acceptance.
3. What monitoring rule applies.
4. What future-work path remains affected.

### 6.4 Future-Work Readiness

Stage 2 finishes by answering:

- Which risks block implementation?
- Which questions require hardware validation?
- Which items become tests?
- Which items become design constraints?
- Which risks are accepted?
- Which future-work path is safe to start?

---

## 7. Playbook Schema Reference

Stage 2 playbooks should remain operational and task-driven.

### 7.1 Required Sections

| Section | Purpose | Required? |
|---|---|:---:|
| Purpose | Why the phase exists | Yes |
| Inputs | Which Stage 1 artifacts are consumed | Yes |
| Tasks | Ordered work items with evidence requirements | Yes |
| Outputs | Files or artifacts produced | Yes |
| Exit Criteria | Pass/fail closeout checklist | Yes |
| Revision History | Document version tracking | Yes |

### 7.2 Task Rules

1. Tasks are linear and ordered.
2. Each task depends on the previous task.
3. Every task must create, update, or verify a document.
4. The final task must be the phase closeout and sync step.

---

## 8. Documentation Structure

### 8.1 Stage 2 Reference Layout

```text
docs/
├── 00-project-overview.md
├── 01-implementation-plan.md
├── DOCUMENTATION_STRUCTURE.md
├── reference/
│   ├── architecture/
│   │   ├── 07-clock-and-timing-map.md
│   │   └── 10-coupling-map.md
│   └── process/
│       ├── risk-register.md
│       ├── trace-plan.md
│       ├── e2e-trace-matrix.md
│       ├── file-manifest.md
│       ├── stage2-risk-resolution-plan.md
│       ├── q0-static-analysis.md
│       ├── q1-solid-review.md
│       ├── q2-bug-catalog.md
│       ├── q3-race-condition-audit.md
│       ├── q4-refactoring-plan.md
│       └── q5-test-strategy.md
```

### 8.2 Authority Rules

- Stage 1 evidence stays in Stage 1 artifacts.
- Stage 2 decisions stay in Stage 2 reference docs.
- The Stage 2 plan consumes Stage 1 outputs; it does not replace them.

---

## 9. Required Stage 2 Artifacts

| Artifact | Purpose | Doc Path |
|---|---|---|
| Stage 2 risk resolution plan | Entry-control backlog and disposition map | `docs/reference/process/stage2-risk-resolution-plan.md` |
| Q-0 static analysis doc | Static analysis and backlog assembly | `docs/reference/process/q0-static-analysis.md` |
| Q-1 SOLID review doc | Design-quality review | `docs/reference/process/q1-solid-review.md` |
| Q-2 bug catalog | Bug and tech-debt triage | `docs/reference/process/q2-bug-catalog.md` |
| Q-3 race-condition audit | Concurrency and shared-state audit | `docs/reference/process/q3-race-condition-audit.md` |
| Q-4 refactoring plan | Safe change ordering | `docs/reference/process/q4-refactoring-plan.md` |
| Q-5 test strategy | Verification strategy and acceptance criteria | `docs/reference/process/q5-test-strategy.md` |

---

## 10. Handoff and Entry Rules

### 10.1 Entry Gate

Q-0 may not start until:

1. The R-10 handoff package exists.
2. The risk register is populated with unresolved items.
3. The coupling map and timing map are complete enough for analysis.
4. The trace matrix identifies blocked, static-only, or missing traces.
5. The Stage 2 risk resolution plan is present.

### 10.2 Q-Phase Continuity

1. Q-1 re-reads the outputs of Q-0.
2. Q-2 re-reads the risk register and Q-1 findings.
3. Q-3 re-reads the coupling map, trace matrix, and timing map.
4. Q-4 consumes the full coupling and hazard picture.
5. Q-5 consumes all prior Q-phase outputs and produces the downstream verification plan.

---

## 11. Cross-Phase Tracking Schema

### 11.1 Input-to-Output Mapping

| Stage 1 Input | Stage 2 Consumer | Primary Use |
|---|---|---|
| Risk register | Q-0, Q-2, Q-5 | Classify, triage, and test risk |
| Open questions | Q-0, Q-3 | Turn unknowns into tasks or validations |
| Coupling map | Q-0, Q-1, Q-4 | Identify structural risk and safe change order |
| Trace matrix | Q-0, Q-5 | Detect gaps and build verification coverage |
| Timing map | Q-0, Q-3, Q-5 | Detect timing hazards and measurement needs |
| File manifest | Q-0 | Anchor repository and provenance checks |
| R-10 handoff package | Q-0 | Define the initial backlog and gate conditions |

---

## 12. Stage Boundary Rules

### 12.1 Stage 1 Rules

| Rule | Description |
|---|---|
| Stage 1 is observational | It produces evidence, not decisions about implementation |
| Stage 1 closes with handoff | It must produce the R-10 handoff package |
| Stage 1 feeds Stage 2 | It prepares the artifacts Stage 2 consumes |

### 12.2 Stage 2 Rules

| Rule | Description |
|---|---|
| Stage 2 is decision-oriented | It classifies, validates, and plans |
| Stage 2 does not change code | It produces no implementation changes |
| Stage 2 ends with readiness | It identifies what can safely happen next |

### 12.3 Implementation Boundary

Code changes come after Q-5, and only after the Stage 2 outputs establish the safe path.

### 12.4 Stage 3 Boundary

Stage 3 is the first implementation stage. It begins only after Q-5 has completed and the readiness outputs establish a safe path for code changes, refactoring, migration, or bug fixing.

---

## 13. Revision History

| Version | Date | Changes |
|---|---|---|
| 1.0 | 2026-06-11 | Initial Stage 2 master schema: introduced Q-0 through Q-5, entry-control rules, disposition model, and Stage 2 docs structure. |

