<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# FM Synthesis Architecture Profile

> **Profile ID:** `RE-PROFILE-FM-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Additive profile for FM (frequency modulation) synthesizer firmware. Use this profile for any firmware that implements operator-based FM synthesis, algorithm routing, and operator parameters. For DX7-class FM with SysEx import and per-operator envelopes, also enable `operator-fm-profile.md`.

---

## When to Enable

Enable this profile when the firmware implements:

- FM operators with configurable frequency ratios
- Algorithm-based operator routing (carrier/modulator assignment)
- Feedback paths (per-operator or per-algorithm)
- Operator envelope generators

---

## R-6b Task Additions

Add these tasks to the Voice / Oscillator / Filter / Modulation / Effects phase:

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| FM algorithm routing | Algorithm table, operator topology, carrier/modulator assignments, algorithm count, algorithm switch behavior | FM algorithms doc |
| Operator parameter map | Per-operator: ratio/fixed frequency, waveform, level, envelope assignment, keyboard scaling | Oscillator doc |
| Feedback path map | Feedback source (single operator / algorithm output), feedback amount, feedback buffer/state, clipping | Oscillator doc |
| Operator phase behavior | Phase reset on note-on, phase sync across operators, phase modulation depth | Oscillator doc |
| Key scaling | Keyboard rate scaling, level scaling per operator, break point, curve depth | Oscillator doc |

---

## R-6c Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Algorithm parameter | Algorithm select parameter; CC/NRPN/SysEx mapping; menu binding; runtime effect on voice restart | Parameter identity matrix |
| Operator parameter set | Per-operator: ratio coarse/fine, detune, level, envelope rate/level, keyboard scaling rates/levels, waveform select | Parameter identity matrix |
| Feedback parameter | Feedback amount per algorithm; range, resolution, effect on timbre | Parameter identity matrix |

---

## Minimum Happy-Path Trace Additions

Add to the E2E trace matrix minimum inventory:

1. MIDI note-on → voice allocation → operator routing → FM algorithm → audio buffer → DAC
2. MIDI CC / knob → parameter normalization → modulation matrix → operator parameter → DSP consumer
3. Patch load → binary decode → runtime operator parameters → audio behavior
4. Audio callback → block render → voice mix → buffer ownership → DMA/DAC output
5. UI control → event queue → parameter update → display feedback
6. Preset save → serialization → checksum → flash/filesystem write
7. Clock/sync → transport/tempo → arpeggiator/LFO timing
8. Oscillator/wavetable → FM algorithm → voice gain → filter → effect → mixer → DAC (gain staging)
9. Multiple voices → FM summing → clipping/saturation → output buffer (mixer)
10. Effect insert chain → dry/wet routing → feedback path → output (effects topology)
11. DIN MIDI RX ISR → parser → running status → dispatch → voice allocation
12. USB MIDI endpoint → packet decode → dispatch → parameter update
13. NRPN select + data entry → 14-bit value → normalized parameter → operator parameter
14. SysEx patch dump → manufacturer ID → command ID → parameter data → storage/engine
15. MIDI input → thru/merge → MIDI output queue → UART/USB output
16. Internal/external clock → tempo state → MIDI clock output
17. Algorithm change → operator routing remap → voice restart behavior

---

## FM-Specific Risk Additions

Add to the risk register under `ENGINE`:

| Risk | Description |
|------|-------------|
| FM feedback overflow | Feedback buffer exceeding range, causing wraparound or instability |
| Algorithm change artifacts | Voice restart or phase discontinuity on algorithm switch |
| Operator aliasing | High modulation index causing aliasing, especially with high-frequency ratios |
| Ratio resolution | Coarse ratio enumeration limiting harmonic possibilities |

---

## Trace Additions

### FM Algorithm Trace

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| FM algorithm routing trace | Operator topology, carrier/modulator assignments, algorithm index → routing map, feedback paths, algorithm count | `R-XX-fm-algorithms.md` |
| Operator ratio trace | Frequency ratio table, fixed-frequency mode, ratio→frequency calculation, phase increment derivation | `R-XX-oscillator.md` |
| Feedback trace | Feedback buffer, feedback amount→buffer gain, feedback clipping/wraparound, per-operator vs. algorithmic feedback | `R-XX-fm-algorithms.md` |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **PreenFM2** | FM with algorithms, operator params, DX7 import; use with `operator-fm-profile.md` and `multitimbral-profile.md` |
| **PreenFM3** | FM with algorithms, bootloader/firmware split; use with `operator-fm-profile.md` and `multitimbral-profile.md` |
| **DX7-class synths** | Use with `operator-fm-profile.md` |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile extracted from `stage1-master-schema-corrected.md` v5.8 |

