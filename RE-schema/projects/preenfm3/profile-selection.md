<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Profile Selection Manifest — PreenFM3

> **Manifest ID:** `RE-MANIFEST-PREENFM3-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Project:** PreenFM3

---

## Selected Master

| Schema | Version | File |
|--------|---------|------|
| `RE-STAGE1-EMBEDDED-SYNTH-MASTER` | `6.0` | `../RE-STAGE1-EMBEDDED-SYNTH-MASTER.md` |

---

## Selected Architecture Profiles

| Profile | Required? | Reason |
|---|---:|---|
| `fm-profile.md` | Yes | 6-operator FM synthesis with algorithm routing, operator parameters, and feedback paths |
| `operator-fm-profile.md` | Yes | DX7-class FM with per-operator multi-stage envelopes, DX7 SysEx import/export, algorithm topology graphs |
| `multitimbral-profile.md` | Yes | Multi-instrument architecture with per-instrument MIDI channel, per-instrument voice allocation, combo presets |
| `subtractive-profile.md` | No | No subtractive filter/VCA signal chain — synthesis is FM |
| `wavetable-va-profile.md` | No | No wavetable oscillators |
| `sample-based-profile.md` | No | Sample support is not an oscillator engine |
| `drum-groovebox-profile.md` | No | Not a drum synth or groovebox |
| `voicecard-satellite-profile.md` | No | Self-contained architecture; no separate voicecards |
| `modular-cv-profile.md` | No | No CV/gate I/O |

---

## Selected Project Overlays

| Overlay | Reason |
|---|---|
| _(none)_ | No PreenFM3-specific project overlay exists yet. Create `profiles/project/preenfm3-overlay.md` if project-specific sub-phases or traces are needed (e.g., stronger boot/update/storage emphasis, flash layout differences from PreenFM2, firmware update protocol specifics). |

---

## Selected Future-Work Overlays

| Overlay | Enabled? | Reason |
|---|---:|---|
| `refactor-readiness.md` | No | Not currently targeting a refactor |
| `generated-registry-readiness.md` | No | Not currently targeting generated registries |

---

## Explicit Non-Applicable Profiles

| Profile | N/A Justification | Evidence Phase |
|---|---|---|
| `subtractive-profile.md` | FM-only synthesis; no oscillator→filter→VCA architecture | R-0 (confirmed at R-6b) |
| `wavetable-va-profile.md` | No wavetable or VA oscillators | R-6b |
| `sample-based-profile.md` | No sample-based synthesis engine | R-6b |
| `drum-groovebox-profile.md` | Not a drum synth; no step sequencer or pattern-based triggering | R-0 |
| `voicecard-satellite-profile.md` | Single-MCU architecture; no inter-board communication | R-3 |
| `modular-cv-profile.md` | No CV/gate I/O hardware | R-3 |

---

## Conflict Resolution Table

| Conflicting Profiles | Conflicting Requirement | Resolution | Rationale |
|---|---|---|------|
| _(none)_ | — | — | No conflicting profile requirements detected |

---

## Dependency Graph Notes

The standard dependency graph applies. Compared to PreenFM2, the PreenFM3 may require stronger emphasis on:

- **R-4** (Boot and Update Flow) — likely different bootloader/firmware split and update protocol
- **R-9** (Storage and Persistence) — possibly different flash layout, filesystem, or preset format

If these differences are significant, a project overlay should be created. If not, the standard phases and enabled profiles are sufficient.

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial manifest for PreenFM3 project |

