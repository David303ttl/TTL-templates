<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Architecture Profiles and Overlays — Index

> **Schema ID:** `RE-PROFILE-INDEX-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`

---

## What This Directory Contains

This directory holds the **Layers 3 and 4** of the reverse-engineering documentation system:

| Layer | Subdirectory | Purpose |
|-------|-------------|---------|
| **Layer 3** | `architecture/` | Additive profiles for specific synthesis architectures, voice topologies, and interface types |
| **Layer 4** | `project/` | Project-specific overlays that bind profiles to a real firmware codebase |
| **Layer 4** | `future-work/` | Optional readiness gates for planned refactoring, generated registries, protocol migration, or storage format changes |

Architectures that do not match any profile use only the core Stage 1 schema (master schema Sections 1–12, 16–20) and the embedded-synth domain profile (Sections 13–15). Profiles are additive — enabling one never requires disabling another.

---

## How to Select Profiles

1. **Start with the master schema** (`RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`). Every project uses this.
2. **Survey the firmware** at R-0 (repository discovery) and R-1 (build baseline). Determine what the firmware actually implements.
3. **Match synthesis methods to architecture profiles.** Enable every profile whose "When to Enable" criteria match observed facts.
4. **Select complementary profiles** for topology (voicecard/satellite), interface (modular/CV), or part management (multitimbral).
5. **Add project and future-work overlays** if a project overlay exists or if you need a refactor-readiness gate.
6. **Record selections** in a profile-selection manifest at `projects/<project>/profile-selection.md`.

---

## Profile Compatibility

### Synthesis Architecture Profiles

These describe *how sound is generated*:

| Profile | File | Description |
|---------|------|-------------|
| **Subtractive** | `architecture/subtractive-profile.md` | Oscillator → filter → VCA; waveform mixing, filter topology, envelope control |
| **FM** | `architecture/fm-profile.md` | Operator-based FM; algorithm routing, operator parameters, feedback paths |
| **Operator-FM (DX7)** | `architecture/operator-fm-profile.md` | Stricter FM for DX7-class synths; per-operator envelopes, SysEx mapping |
| **Wavetable / VA** | `architecture/wavetable-va-profile.md` | Wavetable scanning, VA polyphonic oscillators, digital filters |
| **Sample-Based** | `architecture/sample-based-profile.md` | Sample playback, multi-sample mapping, loop points, sample memory |

All synthesis profiles are **additive** — a hybrid synth can enable multiple.

### Topology and Interface Profiles

These describe *how the device is organized* — not *what sound it makes*:

| Profile | File | Description |
|---------|------|-------------|
| **Drum / Groovebox** | `architecture/drum-groovebox-profile.md` | Multi-track with step sequencer, choke groups, accent, parameter locks |
| **Multi-Part / Multitimbral** | `architecture/multitimbral-profile.md` | Parts, instruments, timbres; per-part voice allocation, combo presets |
| **Voicecard / Satellite** | `architecture/voicecard-satellite-profile.md` | Mainboard ↔ voicecard communication, inter-board protocol, asymmetric firmware |
| **Modular / CV** | `architecture/modular-cv-profile.md` | CV/gate I/O, ADC/DAC scaling, clock I/O, calibration |

All topology/interface profiles are compatible with all synthesis profiles and with each other.

### Compatibility Rules

| Rule | Detail |
|------|--------|
| **Any + Any** | Profiles are additive; enabling one never disables another |
| **`operator-fm` depends on `fm`** | If you enable `operator-fm-profile.md`, also enable `fm-profile.md` |
| **Synthesis + Topology** | Always compatible. Synthesis profiles describe sound generation; topology profiles describe organization |
| **Multiple synthesis profiles** | Always compatible. A hybrid synth with FM + subtractive voices enables both |
| **Multiple topology profiles** | Compatible when the architecture supports it. A multitimbral voicecard synth enables both |

### Profiles That Need Explicit Reconciliation

| Combination | Why Reconciliation Is Needed |
|---|---|
| `drum-groovebox` + `multitimbral` | Both define "part/track" inventories. The manifest must clarify whether drum tracks are a subset of timbral parts or a separate concept. |
| `subtractive-profile` + `wavetable-va-profile` | Both add oscillator tasks to R-6b and filter tasks. The manifest must note that the filter tasks overlap — the stricter profile's requirements take precedence. |

When profiles conflict, resolve in the profile-selection manifest following Section 1.5.7 of the master schema.

---

## Project Overlays

Project overlays bind profiles to a specific firmware codebase. They add:

- Project-specific sub-phases (e.g., inter-MCU protocol discovery)
- Project-specific trace types and artifact paths
- Project-specific risk categories
- Dependency graph edges for project-specific phases
- Future-work readiness gates

| Overlay | File | For |
|---------|------|-----|
| **LXR Overlay** | `project/lxr-overlay.md` | LXR / LXRTTL drum synthesizer |

---

## Future-Work Overlays

Future-work overlays define **optional readiness gates** that evaluate whether the project is ready for specific future changes. They do not add discovery phases — they add evaluation checklists and classification criteria.

| Overlay | File | Purpose |
|---------|------|---------|
| **Refactor Readiness** | `future-work/refactor-readiness.md` | Evaluates whether the codebase is understood well enough to begin refactoring |
| **Generated Registry Readiness** | `future-work/generated-registry-readiness.md` | Evaluates whether parameter/UI metadata can be generated from a single source of truth |

---

## Profile-Selection Manifests

Every real firmware target must have a profile-selection manifest at `projects/<project>/profile-selection.md`. See `projects/README.md` for the manifest scaffold.

Expected manifests:

| Project | Manifest |
|---------|----------|
| **LXR** | `projects/lxr/profile-selection.md` |
| **Ambika** | `projects/ambika/profile-selection.md` |
| **PreenFM2** | `projects/preenfm2/profile-selection.md` |
| **PreenFM3** | `projects/preenfm3/profile-selection.md` |
| **Jeannie** | `projects/jeannie/profile-selection.md` |

---

## Quick Reference: Synth → Profile Mapping

| Firmware | Architecture Profiles | Topology/Interface Profiles | Project Overlay | Future-Work Overlays |
|----------|----------------------|----------------------------|-----------------|---------------------|
| **LXR** | `fm`, `sample-based` | `drum-groovebox` | `lxr-overlay` | `refactor-readiness`, `generated-registry-readiness` |
| **Ambika** | `subtractive`, `wavetable-va` | `multitimbral`, `voicecard-satellite` | — (ambika overlay TBD) | — |
| **PreenFM2** | `fm`, `operator-fm` | `multitimbral` | — | — |
| **PreenFM3** | `fm`, `operator-fm` | `multitimbral` | — | — |
| **Jeannie** | `wavetable-va`, `subtractive` | — | — | — |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial index created alongside master schema v6.0 composition rules |

