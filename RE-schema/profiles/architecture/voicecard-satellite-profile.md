<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Voicecard / Satellite Processor Profile

> **Profile ID:** `RE-PROFILE-VOICECARD-SATELLITE-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Profile for synthesizer firmware architectures where a main/control board communicates with one or more voicecards, DSP boards, analog boards, or satellite MCUs. Use for any host/satellite synth design, not just specific projects.

---

## When to Enable

Enable this profile when the firmware implements:

- A mainboard that controls one or more satellite voice processors
- Inter-board communication protocol for voice assignment and parameter updates
- Voicecard discovery and capability detection
- Voicecard-specific calibration
- Asymmetric firmware (different binaries on mainboard vs. voicecard)

---

## Required Artifacts

| Artifact | Description |
|----------|-------------|
| Mainboard ↔ voicecard protocol map | Command set, framing, addressing, payload format, error handling |
| Voicecard inventory | All voicecard types, identifiers, capabilities, voice count per card |
| Voicecard firmware/build artifact inventory | Separate build targets, binaries, versions for each voicecard type |
| Voice assignment → voicecard routing | How the mainboard allocator picks a voicecard for a given voice |
| Voicecard calibration map | Calibration data per voicecard, storage, retrieval, correction application |
| Analog control signal map | CV/gate/analog signals between mainboard and voicecard |
| Error/recovery behavior | Missing card, mismatched card, communication timeout, card reset |
| Firmware compatibility matrix | Which voicecard firmware versions work with which mainboard firmware |

---

## R-0 Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Voicecard firmware artifacts | Separate voicecard repositories, build targets, binary artifacts, versions | File manifest |
| Voicecard hardware design files | PCB, schematics per voicecard type | Hardware design inventory |

---

## R-1 Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Voicecard build targets | Per-voicecard build invocation, toolchain, artifacts | Build setup reference |
| Voicecard binary equivalence | Per-voicecard binary comparison to releases | Binary equivalence report |

---

## R-3 Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Inter-board physical interface | Connector pinout, SPI/I2C/UART bus between boards, voltage levels | Hardware architecture |
| Voicecard MCU identification | MCU type per voicecard, clock, memory layout | Peripheral map per voicecard |

---

## R-4 Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Voicecard boot/discovery | How the mainboard discovers and initializes voicecards; capability detection | Boot flow reference |
| Voicecard firmware update | How voicecard firmware is updated (from mainboard or independently) | Update flow reference |

---

## R-6b Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Voicecard command protocol | Command IDs, parameter writes, note events, voice allocation commands | Protocol spec |
| Voice assignment routing | Mainboard voice allocator → voicecard selection → voicecard voice engine | Voice allocation doc |
| Voicecard parameter sync | How parameter changes propagate from mainboard state to voicecard DSP | Parameter system doc |
| Voicecard mix/audio return | Audio signal from voicecard back to mainboard; analog or digital return path | Audio signal flow |

---

## Required Traces

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Note event → mainboard allocator → voicecard command → voice output | Voice allocation, card selection, protocol message, voicecard render | `R-XX-note-path.md` |
| Parameter edit → mainboard state → voicecard update → analog/DSP consumer | Parameter write, sync protocol, voicecard parameter application | `R-XX-parameter-trace.md` |
| Voicecard boot/discovery → capability detection → runtime assignment | Boot sequence, card enumeration, capability query, assignment table | `R-XX-boot-flow-trace.md` |
| Calibration load → voicecard parameter correction | Calibration data retrieval, per-voicecard correction, DSP application | `R-XX-storage-trace.md` |
| Voicecard communication error → retry/timeout → fallback/mute | Error detection, retry logic, timeout, safe state | `R-XX-irq-dma-trace.md` |

---

## Voicecard-Specific Risk Additions

| Category | Risk |
|----------|------|
| `HARDWARE` | Voicecard connector wear, inter-board signal integrity, power sequencing |
| `PROTOCOL` | Inter-board protocol corruption, framing errors, sync loss |
| `BOOT` | Voicecard not responding at boot, mismatched firmware versions, discovery timeout |
| `ENGINE` | Voicecard voice budget exhaustion, load imbalance across cards |
| `INTEGRATION` | Mainboard/voicecard state desync, calibration drift, voicecard replacement requiring recalibration |

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **Mutable Instruments Ambika** | 6 voicecard slots, hybrid voicecards with analog filters, mainboard ↔ voicecard SPI communication |
| **Oberheim Matrix-12 / Xpander** | Voice chip per voice architecture |
| **DSI Prophet '08 / Rev2** | Voice card per voice |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile |

