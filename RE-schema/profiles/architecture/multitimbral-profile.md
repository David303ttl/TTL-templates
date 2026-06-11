<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Multi-Part / Multitimbral Profile

> **Profile ID:** `RE-PROFILE-MULTITIMBRAL-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Profile for multi-part / multitimbral synthesizer firmware where multiple parts, instruments, timbres, or independent voice groups coexist. Enable this profile alongside the appropriate synthesis architecture profile(s).

---

## When to Enable

Enable this profile when the firmware implements:

- Multiple parts, instruments, timbres, zones, or channels
- Part-specific MIDI channel assignment
- Part-specific voice allocation and voice count limits
- Combo / multi / performance preset formats
- Independent part output routing

---

## Required Artifacts

| Artifact | Description |
|----------|-------------|
| Part / instrument inventory | All parts with IDs, names, MIDI channels, voice counts, parameters |
| Part → voice allocation matrix | How many voices each part gets; fixed vs. dynamic allocation; total polyphony budget |
| MIDI channel → part mapping | One-to-one, layered (one channel → multiple parts), or split (note ranges → different parts) |
| Part parameter namespace | Per-part parameter isolation; which parameters are global vs. per-part |
| Part storage layout | Part parameters within patch/combo/performance preset binary format |
| Part output routing | Per-part bus assignment, FX sends, pan position |
| Part-level modulation | Per-part LFO, per-part envelope, per-part modulation sources |
| Combo / multi / performance preset format | Patch references, part enable/disable, part volume/pan/transpose, routing |
| Voice stealing and reserved voice policy | Dynamic allocation steal rules, part voice reservation, note priority per part |

---

## R-6b Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Part inventory | Part IDs, names, MIDI channels, voice count, engine type per part | Part inventory |
| Part voice allocation | Total polyphony budget, per-part voice limits, dynamic allocation algorithm, stealing | Voice allocation doc |
| Part parameter isolation | Which parameters are per-part vs. global; parameter namespace partitioning | Parameter system doc |
| Part output routing | Per-part bus, FX send levels, pan law, mute/solo behavior | Mixer doc |

---

## R-8 Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| MIDI channel → part mapping | CC 0 / CC 32 bank select, program change, channel mode messages per part | MIDI protocol doc |
| Per-part MIDI filtering | Channel filter, note range filter, velocity range filter per part | MIDI protocol doc |
| Part-level CC/NRPN | Per-part controller routing; global vs. per-part controllers | CC mapping table |

---

## R-9 Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Combo/multi preset format | Part enable flags, part patch references, part volume/pan/transpose, part FX sends | Patch format spec |
| Part parameter persistence | Per-part parameter storage in combo/multi file; part-independent patch files | Patch format spec |

---

## Required Traces

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| MIDI note → channel → part → voice allocation → engine → output | Channel routing, part lookup, per-part voice allocation, per-part engine render, per-part output mix | `R-XX-midi-trace.md` |
| Program/patch change → part state → DSP state | Program change → part patch load → engine parameter update → audio change | `R-XX-midi-trace.md` |
| Combo/multi load → all parts → voice defaults | Combo file parse → per-part enable → per-part patch load → runtime state | `R-XX-storage-trace.md` |
| Part mute/enable → voice allocation/output behavior | Part mute → voice termination/acoustic silence → output bus behavior | `R-XX-mixer-routing-trace.md` |
| Parameter edit → selected part → parameter store → DSP consumer | Part selection mechanism, parameter addressing, per-part parameter write | `R-XX-parameter-trace.md` |

---

## Multitimbral-Specific Risk Additions

| Category | Risk |
|----------|------|
| `ENGINE` | Per-part voice budget exhaustion, voice stealing across parts, dynamic allocation unfairness |
| `PARAMETER_MODEL` | Part parameter namespace collision, global vs. per-part parameter ambiguity |
| `STORAGE` | Combo referencing missing patches, part enable/disable not persisted |
| `PROTOCOL` | MIDI channel conflict between parts, program change affecting wrong part, bank select overflow |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **PreenFM2** | Multi-instrument with per-instrument MIDI channel, FM algorithms, combo presets; enable with `fm-profile.md` and `operator-fm-profile.md` |
| **PreenFM3** | Multi-instrument FM; enable with `fm-profile.md` and `operator-fm-profile.md` |
| **Mutable Instruments Ambika** | 6-part multitimbral with per-part voicecard assignment; enable with `voicecard-satellite-profile.md` |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile |

