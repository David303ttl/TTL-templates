<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Profile Selection Manifest — Jeannie

> **Manifest ID:** `RE-MANIFEST-JEANNIE-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Project:** Jeannie (8-voice VA/wavetable polyphonic synthesizer)

---

## Selected Master

| Schema | Version | File |
|--------|---------|------|
| `RE-STAGE1-EMBEDDED-SYNTH-MASTER` | `6.0` | `../RE-STAGE1-EMBEDDED-SYNTH-MASTER.md` |

---

## Selected Architecture Profiles

| Profile | Required? | Reason |
|---|---:|---|
| `wavetable-va-profile.md` | Yes | 8-voice polyphonic with wavetable oscillators, wavetable position scanning and interpolation, digital VA oscillator models |
| `subtractive-profile.md` | Yes | Filter/VCA per voice — oscillator→filter→amplifier signal chain with envelope control |
| `fm-profile.md` | No | No FM operator-based synthesis |
| `operator-fm-profile.md` | No | No DX7-class FM |
| `sample-based-profile.md` | No | No sample playback engine |
| `drum-groovebox-profile.md` | No | Not a drum synth or groovebox |
| `multitimbral-profile.md` | TBD — confirm at R-6b | May have parts/timbral layers if the architecture supports multi-part operation. Set to No if purely monotimbral. |
| `voicecard-satellite-profile.md` | No | Self-contained single-MCU architecture on ARM Cortex-M7 |
| `modular-cv-profile.md` | No | No CV/gate I/O expected |

---

## Selected Project Overlays

| Overlay | Reason |
|---|---|
| _(none)_ | No Jeannie-specific project overlay exists yet. Create `profiles/project/jeannie-overlay.md` if project-specific sub-phases or traces are needed. |

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
| `fm-profile.md` | No FM operator-based synthesis | R-0 (confirmed at R-6b) |
| `operator-fm-profile.md` | No DX7-class FM | R-0 |
| `sample-based-profile.md` | No sample playback engine | R-6b |
| `drum-groovebox-profile.md` | Not a drum synth; no step sequencer or pattern-based triggering | R-0 |
| `voicecard-satellite-profile.md` | Single-MCU ARM Cortex-M7 architecture | R-3 |
| `modular-cv-profile.md` | No CV/gate I/O hardware | R-3 |

---

## Conflict Resolution Table

| Conflicting Profiles | Conflicting Requirement | Resolution | Rationale |
|---|---|---|------|
| `wavetable-va-profile` + `subtractive-profile` | Both add filter tasks to R-6b | Both apply additively — `subtractive-profile` defines filter topology and VCA envelope; `wavetable-va-profile` adds digital filter model specifics and coefficient smoothing | The Jeannie has digital filters with VA-style topology |
| `wavetable-va-profile` + `subtractive-profile` | Both add oscillator tasks to R-6b | `wavetable-va-profile` owns oscillator definition (wavetable position, interpolation, scanning); `subtractive-profile` adds waveform mixing and detune behavior | Wavetable oscillators are the primary sound source; subtractive waveform characteristics are secondary |

---

## Dependency Graph Notes

The standard dependency graph applies. No project-specific phase additions. Key considerations:

- **R-6b** must produce wavetable source maps and interpolation behavior per `wavetable-va-profile.md`
- **R-6b** must produce filter topology and VCA envelope routing per `subtractive-profile.md`
- **R-6c** must include wavetable position as a parameter identity target
- **R-9** may include wavetable bank storage if using external flash or SD card

If the architecture supports multi-part/timbral layering (confirmed at R-6b), enable `multitimbral-profile.md` and update this manifest.

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial manifest for Jeannie project |

