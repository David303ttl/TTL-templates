<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Wavetable / VA Polyphonic Profile

> **Profile ID:** `RE-PROFILE-WAVETABLE-VA-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Profile for wavetable and virtual-analog polyphonic synthesizer firmware. Enable this profile when the firmware implements wavetable scanning/interpolation, digital VA oscillators, polyphonic voice allocation, and digital filters.

---

## When to Enable

Enable this profile when the firmware implements:

- Wavetable oscillators with position scanning and interpolation
- Virtual-analog oscillator models (saw, pulse with variable width, etc.)
- Polyphonic voice architecture (typically 4-16 voices)
- Digital filter models
- Wavetable import or selection

---

## Required Artifacts

| Artifact | Description |
|----------|-------------|
| Oscillator/wavetable source inventory | All wavetable banks, VA oscillator types, noise sources |
| Wavetable interpolation/scanning behavior | Position resolution, interpolation method (linear, cubic, etc.), scan rate modulation |
| Filter topology and coefficient update path | Filter types, coefficient calculation, control-rate smoothing |
| Voice allocation and stealing policy | Allocation algorithm, oldest/quietest/newest stealing, voice limit |
| Modulation source/destination matrix | Sources (LFO, env, velocity, aftertouch, etc.) → destinations (pitch, wavetable pos, filter, amp) |
| Oversampling / anti-aliasing / interpolation strategy | Oversampling factor, decimation filter, band-limited wavetables |
| Polyphonic modulation behavior | Per-voice vs. global modulation sources, voice-specific LFO phase |
| Effects topology | Insert order, feedback paths, buffer allocation |

---

## R-6b Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Wavetable source map | Wavetable banks, sample count per table, format (float, int16, etc.), import method | Oscillator doc |
| Wavetable position control | Position parameter (manual, LFO, envelope, velocity), scan speed, modulation depth | Oscillator doc |
| Interpolation method | Linear, cubic, sinc; phase→sample computation; anti-aliasing integration | Oscillator doc |
| Polyphonic voice behavior | Per-voice modulation phase, LFO reset policy, unison detune/spread | Voice allocation doc |
| Digital filter implementation | Filter model (biquad, state-variable, etc.), coefficient update smoothing, self-oscillation behavior | Filter doc |

---

## Minimum Happy-Path Trace Additions

Add to the E2E trace matrix minimum inventory:

1. MIDI note-on → voice allocation → wavetable/VA oscillator → filter → amplifier → DAC
2. Wavetable position modulation → scan rate → interpolation method → oscillator output
3. Wavetable select/load → flash/SD read → wavetable buffer → engine access
4. Multi-wavetable mapping → velocity/note range → wavetable select → crossfade
5. Filter cutoff edit → control-rate smoothing → coefficient update → audio output
6. Voice stealing → envelope/voice reset → artifact risk
7. Polyphonic modulation → per-voice LFO → detune/depth → stereo image

---

## Wavetable/VA-Specific Risk Additions

Add to the risk register under `ENGINE`:

| Risk | Description |
|------|-------------|
| Wavetable interpolation aliasing | Low-resolution or no interpolation causing aliasing at high frequencies |
| Position modulation zippering | Control-rate position updates causing audible discontinuities |
| Voice stealing clicks | Abrupt voice termination without crossfade or envelope reset |
| Polyphonic modulation phase lock | All voices sharing identical LFO phase, reducing stereo width |

---

## Trace Additions

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Wavetable position trace | Position parameter → scan rate → interpolation → sample output | `R-XX-oscillator.md` |
| Wavetable load trace | Import/load → format decode → bank allocation → engine binding | `R-XX-oscillator.md` |
| Polyphonic modulation trace | Per-voice LFO / envelope → phase offset → detune → stereo output | `R-XX-lfo-matrix.md` |
| Filter coefficient smoothing trace | Cutoff/resonance parameter → smoothing filter → coefficient update → audio response | `R-XX-filters-effects.md` |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **Jeannie** | 8-voice VA/wavetable polyphonic on ARM Cortex-M7 |
| **Mutable Instruments Ambika** | Wavetable/hybrid per voicecard; use with `voicecard-satellite-profile.md` |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile |

