<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Operator-FM Profile (DX7-Class)

> **Profile ID:** `RE-PROFILE-OPERATOR-FM-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Profiles:** `fm-profile.md`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Stricter FM profile for operator-based FM synthesizers with per-operator envelopes, DX7 SysEx compatibility, and algorithm routing. Enable alongside `fm-profile.md`.

---

## When to Enable

Enable this profile when the firmware implements:

- Per-operator multi-stage envelopes (not just level scaling)
- Algorithm table with fixed operator topology
- DX7 SysEx parameter import/export
- Fixed set of operators (typically 4, 6, or 8)

---

## Required Artifacts

| Artifact | Description |
|----------|-------------|
| Algorithm table | All algorithm indices with carrier/modulator topology; operator routing graph per algorithm |
| Operator parameter matrix | Per-operator: ratio coarse/fine, detune, waveform, level, envelope rates/levels, keyboard scaling |
| Feedback path map | Per-algorithm feedback source operator, routing, and amount |
| Frequency ratio / fixed-frequency mode map | Which operators use ratio mode vs. fixed-frequency mode |
| Envelope-per-operator table | Envelope stages (rate/level pairs), looping, key scaling |
| Modulation destination matrix | Operator modulation inputs: LFO, velocity, aftertouch, pitch bend |
| DX7/SysEx compatibility map | SysEx parameter address → operator field mapping; manufacturer ID; checksum algorithm |

---

## R-6b Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Algorithm topology graph | Visual routing diagram per algorithm; carrier detection; modulation path depth | FM algorithms doc |
| Operator envelope definition | Per-operator: rate/level stages, rate scaling, level scaling, envelope looping points | Envelope doc |
| DX7 SysEx parameter mapping | SysEx bulk dump format, parameter address → operator/field, bit packing, checksum | SysEx spec |
| Per-operator CPU cost | Operator render cost estimate; total FM cost vs. voice polyphony budget | Timing budget |

---

## Trace Additions

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Algorithm change trace | Algorithm index change → routing graph rebuild → operator render path → voice restart behavior | `R-XX-fm-algorithms.md` |
| Operator ratio edit trace | Ratio parameter → frequency calculation → phase increment → operator output | `R-XX-oscillator.md` |
| Feedback amount edit trace | Feedback parameter → buffer gain → feedback state update → output timbre change | `R-XX-fm-algorithms.md` |
| SysEx import trace | SysEx bulk dump → parameter address decode → operator field → internal patch model → audio change | `R-XX-sysex.md` |
| Operator envelope trace | Envelope trigger → rate/level segment progression → operator level → release → voice deallocation | `R-XX-envelopes.md` |

---

## MIDI / SysEx Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| SysEx patch dump/load | Manufacturer ID, command IDs, parameter address format, bulk dump vs. parameter edit, checksum verification | Protocol spec |
| SysEx parameter address mapping | Parameter address → operator number → field within operator → bit position and width | SysEx mapping table |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **PreenFM2** | 6-operator FM with DX7 SysEx import; enable with `fm-profile.md` and `multitimbral-profile.md` |
| **PreenFM3** | 6-operator FM; enable with `fm-profile.md` and `multitimbral-profile.md` |
| **Dexed** | Reference DX7 emulation for SysEx format comparison |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile |

