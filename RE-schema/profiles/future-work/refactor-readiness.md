<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Future Refactor / Redesign Readiness Gate

> **Overlay ID:** `RE-OVERLAY-FUTURE-REFACTOR-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Optional closeout gate for projects planning a significant refactor, redesign, or port. This overlay defines the generic R-10b readiness gate. Project-specific overlays (e.g., `lxr-overlay.md`) may specialize the gate for their architecture.

---

## When to Enable

Enable this overlay when a Stage 1 reverse engineering project must also produce a readiness assessment for:

- Width-expanding refactors (e.g., `uint8_t` → `uint16_t` parameter IDs)
- Generated code injection (e.g., auto-generated parameter registry)
- Protocol migration (e.g., inter-MCU protocol widening)
- Storage format migration
- Port to new hardware

---

## R-10b: Future Refactor / Redesign Readiness Gate

**Goal:** Evaluate feasibility, identify blockers, and produce a safe implementation order for planned refactoring or redesign work.

**Entry Criteria:**
- R-10 complete (integration, coupling map, trace matrix, risk closeout)

**Classification levels:**

| Level | Meaning |
|-------|---------|
| **Ready** | All prerequisites met; change can proceed with standard verification |
| **Blocked by missing evidence** | Stage 1 evidence insufficient; additional RE needed |
| **Blocked by byte-width contract** | Parameter/field width constraints prevent the change |
| **Blocked by protocol coupling** | Inter-component protocol constrains the change |
| **Blocked by storage format** | Persisted data format prevents migration |
| **Blocked by timing or memory uncertainty** | CPU/memory budget unclear for the change |
| **Blocked by ownership ambiguity** | Unclear who owns the affected state or code |

**Required Inputs:**

The specific inputs depend on the type of refactor. Cross-reference the coupling map (R-10), trace matrix (R-10), risk register (R-10), and relevant subsystem docs to identify all dependencies.

**Required Outputs:**

- Readiness report with classification per change target
- Safe implementation order (leaves-first, core-last)
- Blocked items with specific missing prerequisites

**Task Breakdown:**

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Readiness classification | Classify each planned change as Ready / Blocked with reason | Readiness report |
| Feasibility synthesis | Identify cross-cutting blockers (protocol, storage, memory, timing, ownership) | Feasibility report |
| Safe implementation order | Order changes by coupling dependency (leaves-first) | Readiness report |

**Rule:** R-10b is a separate optional sub-playbook. It does not add a Task 12 to R-10. Keep the full consolidation work in R-10 and the readiness classification in the R-10b closeout report.

---

## Project-Specific Specialization

Project overlays may specialize this gate. For example:

```markdown
# In lxr-overlay.md:

For LXR V3, R-10b specifically evaluates:
- Generated parameter registry feasibility
- Byte-width widening (uint8_t → uint16_t for ParamId, CC, NRPN, automation)
- AVR/STM32 protocol synchronization
- Storage format migration
- AVR PROGMEM headroom
- Safe implementation order
```

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial overlay extracted from `stage1-master-schema-corrected.md` v5.8 |

