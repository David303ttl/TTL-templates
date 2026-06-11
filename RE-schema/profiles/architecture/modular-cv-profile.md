<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Modular / CV-Controlled Profile

> **Profile ID:** `RE-PROFILE-MODULAR-CV-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Profile for synthesizer firmware that implements CV (control voltage) / gate inputs and/or outputs, clock I/O, and modular patch-point routing. Enable this profile when the firmware reads CV inputs via ADC, outputs CV via DAC or PWM, or uses gate/trigger signals.

---

## When to Enable

Enable this profile when the firmware implements:

- CV inputs via ADC (pitch, modulation, expression, etc.)
- CV outputs via DAC or filtered PWM
- Gate/trigger inputs via GPIO
- Gate/trigger outputs
- Clock input/output for modular sync
- CV-to-parameter mapping or scaling curves

---

## Required Artifacts

| Artifact | Description |
|----------|-------------|
| CV input hardware map | ADC channels, input ranges, scaling, sampling rate, filtering |
| CV output hardware map | DAC channels, output ranges, update rate, calibration |
| Gate/trigger input map | GPIO pins, edge detection, debounce, note/trigger conversion |
| Gate/trigger output map | GPIO pins, pulse width, timing |
| CV→parameter scaling table | Input voltage → normalized parameter value; per-destination curves |
| Calibration/config fields | CV offset, scaling factor, per-input calibration persistence |

---

## R-3 Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| CV input ADC mapping | ADC channels, input voltage range, reference voltage, sampling rate, hardware filtering | Peripheral map |
| CV output DAC mapping | DAC channels, output range, update mechanism (timer/DMA/manual) | Peripheral map |
| Gate/trigger GPIO mapping | GPIO pins, edge interrupts, pull-up/down, Schmitt trigger config | Peripheral map |

---

## R-7 Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| ADC scan trace | ADC scan sequence, DMA, sampling rate, filtering/averaging; CV noise floor | I/O trace |
| DAC update trace | DAC output path, timer-triggered updates, calibration scaling, output protection | I/O trace |
| Gate input ISR trace | GPIO edge ISR, debounce algorithm, gate→note conversion, clock→tempo | I/O trace |

---

## Minimum Happy-Path Trace Additions

1. CV input → ADC read → scaling → parameter normalization → DSP consumer
2. Gate input → GPIO edge → note trigger → voice allocation
3. Clock input → GPIO timer capture → tempo derivation → sequencer/LFO sync
4. CV output → DAC write → scaling → output jack
5. Gate output → GPIO set → timed pulse → output jack

---

## Minimum Negative-Path Trace Additions

1. CV input out of range → clamp/ignore/wrap behavior
2. Gate bounce → debounce rejection → valid gate detection
3. Clock input jitter/noise → tempo filtering → sync drift behavior
4. CV output glitch on parameter change → smoothing/interpolation → output settling

---

## Modular/CV-Specific Risk Additions

| Category | Risk |
|----------|------|
| `HARDWARE` | ADC input overvoltage, DAC output short protection, GPIO input protection |
| `ENGINE` | CV sampling aliasing, gate debounce missing fast triggers, clock jitter affecting LFO/sequencer |
| `PARAMETER_MODEL` | CV scaling curve mismatch (linear vs. exponential), incorrect voltage-to-octave scaling (1V/oct vs. Hz/V) |
| `STORAGE` | Calibration data loss, factory calibration vs. user calibration |

---

## Trace Additions

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| CV input trace | ADC read → filtering/averaging → scaling → parameter binding → DSP | `R-XX-cv-trace.md` |
| Gate input trace | GPIO edge → debounce → note/trigger event → voice allocation | `R-XX-cv-trace.md` |
| Clock I/O trace | GPIO timer capture → tempo calculation → LFO/seq sync; internal tempo → clock output | `R-XX-cv-trace.md` |
| CV output trace | Parameter → scaling → DAC write → output voltage | `R-XX-cv-trace.md` |
| Calibration trace | Factory cal data → retrieval → correction → applied to ADC/DAC path | `R-XX-cv-trace.md` |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **Mutable Instruments modules** | CV/gate I/O as primary interface |
| **Eurorack digital modules** | CV-controlled digital synthesis |
| **Hybrid synths** | Synths with CV expansion or CV input support |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile |

