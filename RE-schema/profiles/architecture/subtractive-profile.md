<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Subtractive Synthesis Architecture Profile

> **Profile ID:** `RE-PROFILE-SUBTRACTIVE-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Additive profile for subtractive synthesizer firmware. Enable this profile when the firmware implements oscillator waveform mixing, filter-based spectral shaping, and VCA/VCF envelope control.

---

## When to Enable

Enable this profile when the firmware implements:

- Oscillator waveform generation (saw, square, triangle, sine, noise)
- Filter with variable cutoff/resonance
- Envelope-controlled amplifier and/or filter
- Voice architecture with oscillator → filter → amplifier signal chain

---

## R-6b Task Additions

Add these tasks to the Voice / Oscillator / Filter / Modulation / Effects phase:

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Oscillator waveform inventory | Waveform types (saw, pulse, tri, sine, noise), pulse width, oscillator sync, ring modulation, cross-modulation | Oscillator doc |
| Filter topology | Filter types (LPF, HPF, BPF, notch), poles, self-oscillation, key tracking, envelope modulation depth | Filter doc |
| VCA / amplifier envelope | Amplifier envelope routing, velocity→VCA depth, pan modulation | Envelope doc |
| Voice mixer / oscillator balance | Per-oscillator level, waveform mix weights, noise balance, sub-oscillator | Mixer doc |
| Oscillator detune / drift | Detune cents/Hz range, analog drift simulation, phase reset behavior | Oscillator doc |

---

## R-6c Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Filter parameter identity | Cutoff, resonance, envelope amount, key tracking, filter type; CC/NRPN mappings | Parameter identity matrix |
| Oscillator waveform select | Waveform index per oscillator; CC/SysEx mappable; menu slot binding | Parameter identity matrix |
| VCA envelope parameters | Attack, decay, sustain, release per voice; velocity depth; level | Parameter identity matrix |

---

## Minimum Happy-Path Trace Additions

Add to the E2E trace matrix minimum inventory:

1. MIDI note-on → voice allocation → oscillator waveform → filter → amplifier envelope → DAC
2. MIDI CC / knob → parameter normalization → oscillator/filter/amp parameter → DSP consumer
3. Patch load → binary decode → runtime parameters → audio behavior
4. Oscillator → mixer → filter → amplifier → DAC (gain staging)
5. Filter modulation → envelope → LFO → key tracking → cutoff response
6. Voice stacking/unison → detune spread → voice sum → clipping
7. Effects chain → chorus/flanger/delay/reverb → dry/wet → output

---

## Subtractive-Specific Risk Additions

Add to the risk register under `ENGINE`:

| Risk | Description |
|------|-------------|
| Filter instability | Self-oscillation amplitude runaway, coefficient overflow at high resonance |
| Oscillator aliasing | Naive sawtooth/square generation without band-limiting |
| Voice summing overflow | Multiple voices summed without headroom, causing hard clipping |
| Envelope zippering | Control-rate envelope updates causing audible steps |

---

## Trace Additions

### Oscillator / Noise Trace

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Oscillator waveform trace | Waveform source, phase accumulation, pulse width modulation, sync, ring mod, sub-oscillator, anti-aliasing | `R-XX-oscillator.md` |
| Noise generator trace | Noise type (white/pink), seed/reseed, spectral shaping, noise injection points | `R-XX-oscillator.md` |

### Filter Trace

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Filter coefficient trace | Cutoff→coefficient mapping, resonance→Q mapping, nonlinearities, saturation, key tracking curve | `R-XX-filters-effects.md` |
| Filter modulation trace | Envelope depth, LFO depth, velocity→cutoff, aftertouch→cutoff, smoothing/rate limiting | `R-XX-filters-effects.md` |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **Jeannie** | 8-voice VA/wavetable polyphonic; use with `wavetable-va-profile.md` |
| **Mutable Instruments Ambika** | Hybrid subtractive per voicecard; use with `multitimbral-profile.md` and `voicecard-satellite-profile.md` |
| **Sequential Prophet emulations** | Classic subtractive architecture |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile extracted from `stage1-master-schema-corrected.md` v5.8 |

