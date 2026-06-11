<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Profile Selection Manifest — LXR / LXRTTL

> **Manifest ID:** `RE-MANIFEST-LXR-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Project:** LXR / LXRTTL drum synthesizer

---

## Selected Master

| Schema | Version | File |
|--------|---------|------|
| `RE-STAGE1-EMBEDDED-SYNTH-MASTER` | `6.0` | `../RE-STAGE1-EMBEDDED-SYNTH-MASTER.md` |

---

## Selected Architecture Profiles

| Profile | Required? | Reason |
|---|---:|---|
| `drum-groovebox-profile.md` | Yes | Multi-track drum synth with step sequencer, choke groups, accent, parameter locks, per-track engine variants |
| `fm-profile.md` | Yes | Multiple tracks use FM-based engine variants with operator routing and feedback paths |
| `sample-based-profile.md` | Yes | Multiple tracks use sample-based engine variants with sample playback, pitch shifting, and sample memory management |
| `subtractive-profile.md` | No | Not a subtractive filter/VCA architecture — engines are drum-specific |
| `wavetable-va-profile.md` | No | No wavetable scanning or VA polyphonic oscillators |
| `multitimbral-profile.md` | No | Drum tracks are not timbral parts — the `drum-groovebox` profile covers track topology |
| `voicecard-satellite-profile.md` | No | Dual-MCU (STM32 + ATmega AVR) is a front-panel architecture, not a voicecard/satellite topology |
| `modular-cv-profile.md` | No | No CV/gate I/O |
| `operator-fm-profile.md` | No | Not DX7-class — FM engines are simplified drum-oriented operators, not full 6-operator with DX7 SysEx |

---

## Selected Project Overlays

| Overlay | Reason |
|---|---|
| `../profiles/project/lxr-overlay.md` | LXR-specific dual-MCU front-panel protocol (AVR↔STM32), sequencer runtime, pattern/automation width audit, V3 refactor readiness gate |

---

## Selected Future-Work Overlays

| Overlay | Enabled? | Reason |
|---|---:|---|
| `refactor-readiness.md` | Yes | The LXRTTL project targets a V3 refactor; needs explicit readiness gate before Stage 2 code changes |
| `generated-registry-readiness.md` | Yes | AVR menu metadata and STM32 parameter registry are candidates for generation from a single source of truth; needs feasibility evaluation |

---

## Explicit Non-Applicable Profiles

| Profile | N/A Justification | Evidence Phase |
|---|---|---|
| `subtractive-profile.md` | LXR engines are drum-synthesis variants (FM, sample, transient, noise), not general-purpose subtractive oscillators | R-0 (confirmed at R-6b) |
| `wavetable-va-profile.md` | No wavetable oscillators detected in source tree | R-0 (confirmed at R-6b) |
| `multitimbral-profile.md` | Drum tracks are voice channels with per-track engines, not multi-part timbral layers | R-6b |
| `voicecard-satellite-profile.md` | AVR is a UI controller, not a satellite voice processor | R-3 |
| `modular-cv-profile.md` | No CV/gate I/O hardware or code | R-3 |
| `operator-fm-profile.md` | FM engines do not implement per-operator multi-stage envelopes or DX7 SysEx compatibility | R-6b |

---

## Conflict Resolution Table

| Conflicting Profiles | Conflicting Requirement | Resolution | Rationale |
|---|---|---|------|
| `drum-groovebox` + `sample-based` | Both define R-6b oscillator/variant tasks | `drum-groovebox` owns the engine variant registry structure; `sample-based` adds sample-specific task details to sample-based engine variants within that registry | Drum tracks use different engine types; the registry unifies them |
| `drum-groovebox` + `fm` | Both define R-6b oscillator tasks | `drum-groovebox` owns the per-track engine binding; `fm` adds FM-specific algorithm routing and operator parameter tasks to FM-based engine variants | FM is a sub-profile of drum engine variants |

---

## Dependency Graph Additions

Per `lxr-overlay.md`:

```
R-5 → R-5b (AVR menu metadata)
R-5b, R-6c, R-7 → R-8d (front-panel protocol)
R-6c, R-7, R-8/R-8d → R-8b (sequencer runtime)
R-6b, R-6c, R-8/R-8d, R-8b, R-9/R-9b → R-9c (track topology / writer precedence)
R-10 → R-10b (V3 readiness gate)
```

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial manifest for LXR / LXRTTL project |

