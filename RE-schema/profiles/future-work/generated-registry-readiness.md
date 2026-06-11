<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Generated Registry and Width-Expansion Readiness

> **Overlay ID:** `RE-OVERLAY-GENERATED-REGISTRY-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Overlay:** `refactor-readiness.md`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Specific readiness assessment for projects planning to generate a parameter registry or metadata tables from the codebase, or widen byte-width contracts across the system.

---

## When to Enable

Enable this overlay when the refactor plan includes:

- Auto-generated parameter registry from source annotations or extracted tables
- Auto-generated menu metadata, CC/NRPN mapping tables, or patch field layouts
- Byte-width widening (e.g., `uint8_t` parameter IDs → `uint16_t`)
- Cross-MCU constant synchronization via code generation

---

## Feasibility Assessment Factors

| Factor | Question | Evidence Source |
|--------|----------|-----------------|
| **Parameter identity completeness** | Are all parameters identified with canonical IDs, ranges, and bindings? | R-6c parameter identity matrix |
| **Byte-width audit** | Which fields are byte-width constrained? Where would widening break? | R-6c byte-width audit |
| **Protocol dependencies** | Which protocol fields carry ordinal-limited or byte-width-limited values? | R-8 MIDI CC/NRPN tables, R-8d protocol tables |
| **Storage dependencies** | Which persisted fields carry byte-width contracts? | R-9/R-9b patch/pattern format specs |
| **AVR/menu metadata** | Can menu label/value tables be generated? What is the PROGMEM budget? | R-5b AVR menu metadata map |
| **Cross-MCU constant sync** | Are shared constants (command IDs, enums, ordinals) identical across MCUs? | R-5b/R-8d constant sync audit |
| **Ownership map** | Who writes each registry candidate? Who reads it? | R-10 coupling map |

---

## Required Evidence Inputs

| Input | Produced By | Required For |
|-------|-------------|-------------|
| Parameter identity matrix | R-6c | Registry table generation |
| Byte-width audit | R-6c | Width widening assessment |
| DSP binding matrix | R-6 | Runtime consumer identification |
| MIDI CC/NRPN mapping tables | R-8 | Generated CC mapping |
| Front-panel protocol command table | R-8d | Protocol widening blockers |
| AVR menu metadata map | R-5b | PROGMEM budget |
| AVR/STM32 constant sync audit | R-5b/R-8d | Cross-MCU consistency |
| Pattern/automation width audit | R-9b | Storage migration risk |
| Coupling map | R-10 | Blast radius of generated code |
| Trace matrix | R-10 | Verification trace coverage |

---

## Readiness Report Schema

```markdown
# Generated Parameter Registry Feasibility Report

> **Document ID:** `[PROJECT]-FEASIBILITY-REGISTRY-01`
> **Phase:** R-10b
> **Date:** YYYY-MM-DD

## 1. Registry Candidate Tables

| Table | Source Evidence | Generation Maturity | Width Risk | Protocol Risk | Storage Risk |
|-------|----------------|--------------------|------------|---------------|-------------|
| [name] | `file:line` | Ready / Needs Annotation / Needs Extraction | None / Low / Med / High | None / ... | None / ... |

## 2. Width-Widening Blockers

| Field | Current Width | Target Width | Blockers | Resolution |
|-------|--------------|-------------|----------|------------|
| [ParamId] | uint8_t | uint16_t | [reasons] | [plan] |

## 3. AVR PROGMEM Headroom

| Current Footprint | Available | Generated Estimate | Headroom |
|-------------------|-----------|--------------------|----------|
| [N] bytes | [M] bytes | [G] bytes | [M - N - G] bytes |

## 4. Protocol/Storage Dependencies

| Dependency | Owning Phase | Impact | Safe Change Order |
|-----------|-------------|--------|-------------------|
| [dep] | [phase] | [impact] | [order] |

## 5. Classification Summary

| Item | Classification | Reason |
|------|---------------|--------|
| [Registry table X] | Ready / Blocked by X | [reason] |

## 6. Safe Implementation Order

1. [Leaves-first change]
2. [Intermediate change]
3. [Core change]
```

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial overlay extracted from `stage1-master-schema-corrected.md` v5.8 |

