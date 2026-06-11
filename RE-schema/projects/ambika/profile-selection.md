<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Profile Selection Manifest — Ambika

> **Manifest ID:** `RE-MANIFEST-AMBIKA-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Project:** Mutable Instruments Ambika

---

## Selected Master

| Schema | Version | File |
|--------|---------|------|
| `RE-STAGE1-EMBEDDED-SYNTH-MASTER` | `6.0` | `../RE-STAGE1-EMBEDDED-SYNTH-MASTER.md` |

---

## Selected Architecture Profiles

| Profile | Required? | Reason |
|---|---:|---|
| `subtractive-profile.md` | Yes | Hybrid subtractive per voicecard — oscillator waveform mixing, analog/digital filter per card, VCA envelope |
| `wavetable-va-profile.md` | Yes | Multiple voicecard types implement wavetable oscillators; digital VA oscillators on some cards |
| `multitimbral-profile.md` | Yes | 6-part multitimbral with per-part voicecard assignment, part-specific parameters, combo/multi presets |
| `voicecard-satellite-profile.md` | Yes | Mainboard controls up to 6 voicecards via SPI; inter-board protocol, voicecard discovery, per-card calibration |
| `fm-profile.md` | No | No FM operator-based synthesis |
| `operator-fm-profile.md` | No | No DX7-class FM |
| `sample-based-profile.md` | No | Voicecards are oscillator-based, not sample playback engines |
| `drum-groovebox-profile.md` | No | Not a drum synth or groovebox — no step sequencer, choke groups, or parameter locks |
| `modular-cv-profile.md` | No | No CV/gate I/O on standard configuration |

---

## Selected Project Overlays

| Overlay | Reason |
|---|---|
| _(none)_ | No Ambika-specific project overlay exists yet. Create `profiles/project/ambika-overlay.md` if project-specific sub-phases or traces are needed. |

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
| `fm-profile.md` | No FM operator-based synthesis in any voicecard type | R-0 (confirmed at R-6b) |
| `operator-fm-profile.md` | No DX7-class FM | R-0 |
| `sample-based-profile.md` | Voicecards are oscillator-based; no sample playback engines | R-6b |
| `drum-groovebox-profile.md` | Not a drum synth; no step sequencer, choke groups, or pattern-based triggering | R-0 |
| `modular-cv-profile.md` | No CV/gate I/O | R-3 |

---

## Conflict Resolution Table

| Conflicting Profiles | Conflicting Requirement | Resolution | Rationale |
|---|---|---|------|
| `subtractive-profile` + `wavetable-va-profile` | Both add filter tasks to R-6b | `subtractive-profile` filter tasks take precedence for filter topology and VCA envelope; `wavetable-va-profile` adds digital filter model specifics | Filter is per-voicecard and may be analog or digital depending on card type |
| `subtractive-profile` + `wavetable-va-profile` | Both add oscillator tasks to R-6b | Both apply additively — voicecard type determines which oscillator tasks are relevant per card | Multiple voicecard types coexist; inventory per card type |
| `multitimbral-profile` + `voicecard-satellite-profile` | Both define part/voicecard assignment tasks | `multitimbral-profile` owns the part inventory; `voicecard-satellite-profile` owns the mainboard→voicecard protocol; both apply without conflict | Parts are logical; voicecards are physical; the mapping is the intersection |
| `multitimbral-profile` + `voicecard-satellite-profile` | Both define voice allocation tasks | `multitimbral-profile` owns per-part voice budgets; `voicecard-satellite-profile` owns per-card voice routing; the allocator combines both | The mainboard allocator distributes parts' voice budgets across available voicecards |

---

## Dependency Graph Notes

The standard dependency graph applies. No project-specific phase additions. Key topology considerations:

- **R-3** must inventory voicecard MCU types and inter-board bus per `voicecard-satellite-profile.md`
- **R-4** must document voicecard boot/discovery per `voicecard-satellite-profile.md`
- **R-6b** must produce a per-voicecard-type engine inventory (wavetable, subtractive, hybrid) — treat each voicecard type as a separate engine variant
- **R-9** must document combo/multi preset format per `multitimbral-profile.md`

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial manifest for Ambika project |

