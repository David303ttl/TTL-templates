<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Sample-Based Synthesis Profile

> **Profile ID:** `RE-PROFILE-SAMPLE-BASED-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Profile for sample-based synthesizer firmware — sample playback, multi-sample mapping, pitch shifting, and sample import/management.

---

## When to Enable

Enable this profile when the firmware implements:

- Sample playback with pitch shifting
- Multi-sample mapping (velocity layers, keyboard zones)
- Sample loading from flash, SD card, or external memory
- Sample-based oscillator engine

---

## Required Artifacts

| Artifact | Description |
|----------|-------------|
| Sample playback engine | Sample rate conversion, pitch shifting algorithm, interpolation method |
| Multi-sample mapping | Velocity range → sample index, note range → sample index, velocity crossfade |
| Sample load path | Source (flash/SD/USB) → format decode → sample buffer → engine binding |
| Sample format support | Bit depth (8/16/24), encoding (PCM, ADPCM, floating), sample rate handling |
| Sample memory layout | Sample buffer allocation, memory pool, streaming vs. preload, polyphony memory constraint |

---

## R-6b Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Sample playback engine | Pitch shifting algorithm (linear, cubic, sinc), sample rate conversion, fractional phase accumulator | Oscillator doc |
| Multi-sample zone map | Note range → sample index table, velocity range → sample index table, crossfade zone | Oscillator doc |
| Sample loop points | Loop start/end, loop type (forward, bidirectional, one-shot), crossfade looping | Oscillator doc |
| Sample memory accounting | Per-sample size, total pool size, polyphony × sample memory budget, loading priority | Memory map update |

---

## R-9 Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Sample import path | File open → header parse → format validation → decode → sample buffer write → engine register | Storage trace |
| Sample metadata storage | Sample ID, name, loop points, root note, tuning, volume, pan; storage format | Patch format spec |
| Sample bank management | Bank files, sample count limit, sample slot allocation, delete/replace behavior | Storage doc |

---

## Minimum Happy-Path Trace Additions

1. MIDI note-on → voice allocation → sample select → pitch shift → interpolation → DAC
2. Multi-sample mapping → velocity/note range → sample select → crossfade → output
3. Sample load → flash/SD read → decode → sample buffer → engine access
4. Sample loop → loop start → loop end → crossfade → seamless transition
5. Sample pitch → note number → playback rate → phase increment → interpolation

---

## Sample-Specific Risk Additions

| Risk | Description |
|------|-------------|
| Sample memory exhaustion | Polyphony × sample requirements exceeding SRAM budget |
| Aliasing on pitch shift | Low-quality interpolation at high pitch shifts |
| Loop click | Non-zero crossing loop points causing audible clicks |
| SD card latency | Read latency during polyphonic sample playback causing underrun |

---

## Trace Additions

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Sample playback trace | Sample select → pitch calculation → phase accumulator → interpolation → output | `R-XX-oscillator.md` |
| Multi-sample zone trace | Note/velocity → zone match → sample select → crossfade weights → mix | `R-XX-oscillator.md` |
| Sample load trace | File path → open → read header → allocate buffer → decode samples → engine bind | `R-XX-storage-trace.md` |
| Loop boundary trace | Phase reaches loop end → wrap to loop start → interpolation across boundary | `R-XX-oscillator.md` |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **LXR** | Drum synth with sample-based engines; use with `drum-groovebox-profile.md` |
| **PreenFM2** | Has user sample support in some firmware revisions |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile |

