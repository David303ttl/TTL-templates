<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# LXR / LXRTTL Project Overlay

> **Overlay ID:** `RE-OVERLAY-LXR-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
> **Required Profiles:** `drum-groovebox-profile.md`, `fm-profile.md`, `sample-based-profile.md`
>
> **Purpose:** Project-specific overlay for the LXR / LXRTTL drum synthesizer firmware reverse engineering. This overlay adds LXR-specific phases (R-5b, R-8d, R-9b, R-9c, R-10b), artifacts, traces, risk categories, and V3 future-work gates. It must not weaken the core Stage 1 evidence, citation, trace, risk, or closeout rules.

---

## LXR Architecture Summary

The LXR firmware is a dual-MCU architecture:

- **STM32F4** — main DSP and control processor: synthesis engine, audio output, MIDI I/O, storage, sequencer
- **ATmega (AVR)** — front-panel controller: encoders, buttons, OLED display, menu system
- **Communication:** Internal AVR↔STM32 front-panel protocol over UART/SPI
- **Synthesis:** Drum synth with per-track engine variants (FM, sample, transient, noise), 6-voice polyphony

---

## LXR-Specific Phase Additions

The following sub-phases are LXR-specific. Generalize them for similar dual-MCU front-panel architectures when reusing.

### R-5b: AVR UI Parameter / Menu Metadata

> **Applicable when:** The UI firmware runs on a separate MCU with its own flash-resident menu tables.

**Goal:** Document AVR flash-resident menu pages, display labels, value tables, ParamId bindings, and PROGMEM footprint.

**Entry Criteria:**
- R-5 complete (UI ownership and control-surface mechanics documented)
- AVR source repository accessible

**Tasks:**

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Menu/page inventory | Menu pages, labels, value tables, display kinds | AVR menu metadata map |
| ParamId binding | Menu rows, ParamId mapping, alias detection | AVR menu metadata map |
| PROGMEM footprint | Flash/RAM footprint, label compression, table growth | PROGMEM footprint report |
| Generated-registry candidate | Which tables can be generated vs. manual | Generated parameter registry feasibility report (input) |

**Deliverables:**
- AVR menu metadata map (`docs/reference/subsystems/XX-parameter-system.md`)
- PROGMEM footprint analysis

**Required Traces:**
- AVR menu metadata trace: `R-XX-avr-menu-metadata.md`
- AVR/STM32 constant sync audit: `R-XX-constant-sync-audit.md`

---

### R-8d: Inter-MCU Front-Panel Protocol

> **Applicable when:** A separate front-panel MCU communicates with the main DSP MCU via an internal protocol.

**Goal:** Document the AVR↔STM32 front-panel command set, framing, ACK/timeout behavior, and error handling.

**Entry Criteria:**
- R-5b complete (AVR menu metadata map)
- R-6c complete (parameter identity contract)
- R-7 complete (hardware I/O, UART/SPI bus config)

**Tasks:**

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Frame format | Framing, command IDs, payload widths, version byte | Front-panel protocol command table |
| AVR encode path | Menu/encoder event → frame payload → transmit | Front-panel protocol trace |
| STM32 decode path | Receive → parse → dispatch → param/state update | Front-panel protocol trace |
| Constant sync | AVR and STM32 enums/ordinals/widths stay identical | AVR/STM32 constant sync audit |
| Error handling | ACKs, timeouts, malformed frames, retries | Front-panel protocol trace |

**Deliverables:**
- Front-panel protocol command table (`docs/specifications/31-front-panel-protocol.md`)
- AVR/STM32 constant sync audit

**Required Traces:**
- Front-panel protocol trace: `R-XX-front-panel-protocol.md`
- AVR/STM32 constant sync audit: `R-XX-constant-sync-audit.md`

**Boundary:** R-8d owns the internal front-panel transport mechanics. Keep MIDI/USB/CV protocol discovery in R-8a/b/c and do not mix the control-surface transport with the external MIDI protocol docs.

---

### R-9b: Pattern / Automation / Mod-Destination Storage Width

> **Applicable when:** Pattern step data, automation destinations, or modulation targets have stored byte-width contracts that may need widening.

**Goal:** Audit stored pattern fields, automation destinations, and mod targets for byte-width limits, widening risks, and migration triggers.

**Entry Criteria:**
- R-6 complete (engine parameter model)
- R-7 complete (I/O)
- R-8 complete (MIDI/protocol)
- R-8b complete (sequencer runtime)

**Tasks:**

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Stored field inventory | Pattern steps, automation slots, mod destinations, payload sizes | Pattern automation width audit |
| Width contract | Which fields fit in `uint8_t` vs. require widening | Pattern automation width audit |
| Migration risk | What breaks if widths change | Pattern automation width audit |

**Required Traces:**
- Pattern / automation width trace: `R-XX-pattern-automation-width.md`
- Automation / modulation width audit: `R-XX-automation-mod-target-width-audit.md`

---

### R-9c: Track / Engine Topology and Writer Precedence Overlay

> **Applicable when:** Tracks, parts, lanes, instruments, or role-specific voices exist.

**Goal:** Consolidate track/lane/instrument topology, engine binding, drum behavior ordering, and parameter writer conflict rules after R-6, R-8b, and R-9 evidence exists.

**Entry Criteria:**
- R-6b/R-6c complete (engine, parameter identity)
- R-8/R-8d complete (MIDI, front-panel protocol)
- R-8b complete (sequencer runtime)
- R-9/R-9b complete (storage, width audit)

**Tasks:**

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Track / instrument / lane inventory | Track IDs, public names, engine type, trigger sources, voice/choke rules, parameter namespace, UI/MIDI/sequencer/storage bindings, output bus, FX sends, mute/solo/accent | Track / instrument / lane inventory |
| Engine variant registry | Engine IDs, render entries, state structs, tone/noise/transient sources, envelopes, filter/drive, modulation inputs, output format, CPU risk, storage fields | Engine variant registry |
| Track event and drum behavior traces | Step/pad/MIDI event → track/lane → engine → output; accent/mute/solo/choke ordering | Track lane event trace, drum behavior trace |
| Parameter writer precedence | Defaults/load, UI, MIDI/SysEx, p-locks, automation, LFO/modulation, calibration writers, conflict rules, smoothing, applied-at timing | Parameter writer precedence matrix |
| R-10 handoff | Cross-check R-6/R-8/R-8b/R-9/R-9b consistency and record R-10 consumer notes | R-10 handoff note |

---

## LXR Sequencer Runtime Split (R-8b)

For LXRTTL's sequencer-heavy runtime, add a dedicated R-8b playbook for transport decoding, step scheduling, and live pattern staging. This layer is documented in `docs/playbooks/phase-r8b-sequencer-transport-pattern-runtime.md`.

R-8b produces:
- `docs/reference/subsystems/32-sequencer-transport-pattern-runtime.md`
- `docs/specifications/32-sequencer-runtime-contract.md`
- `docs/reference/process/phase-r8b-evidence/`
- R-8b trace-plan rows
- R-SEQ / sequencer-runtime risks
- Sequencer timing notes for `docs/reference/architecture/07-clock-and-timing-map.md`

**R-8b Rules for LXR:**
- Document observed legacy behavior first
- Classify each major runtime finding by V3 fate before closeout
- Split external MIDI sync handoff from front-panel pattern handoff when they are different evidence paths

---

## LXR-Specific Traces

These traces are LXR-specific. Generalize for similar architectures.

### Inter-MCU Traces

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Front-panel protocol trace | AVR encoder/menu event → front-panel frame → STM32 dispatch → parameter/state update, ACK/timeout behavior, malformed frame handling | `R-XX-front-panel-protocol.md` |
| AVR menu metadata trace | Menu pages, display labels, value tables, ParamId bindings, display footprint, PROGMEM usage | `R-XX-avr-menu-metadata.md` |
| AVR/STM32 constant sync audit | Shared command IDs, parameter ordinals, payload widths, enum values, version bytes, mismatch detection | `R-XX-constant-sync-audit.md` |

### Sequencer / Pattern Runtime Traces

| Trace Type | What to Discover | Evidence File Pattern |
|------------|-----------------|----------------------|
| Sequencer transport trace | Decoded start/stop/continue/clock → sequencer running state, playhead, tick counters, external sync gate | `R-XX-sequencer-transport-state.md` |
| Pattern runtime trace | Active pattern, shown/edit pattern, temp/staged pattern, switch lifecycle, atomicity assumptions | `R-XX-pattern-runtime.md` |
| Step evaluation trace | Step active/probability/length/rotation/roll evaluation → trigger decision | `R-XX-sequencer-trigger-path.md` |
| Trigger dispatch trace | Sequencer/UI/MIDI preview trigger boundary → voice trigger command path | `R-XX-sequencer-trigger-path.md` |
| Track lane event trace | Step/pad/MIDI event → track/lane resolution → engine binding → trigger dispatch → output path | `R-XX-track-lane-event-trace.md` |
| Drum behavior trace | Accent, mute, solo, choke, flam/roll/ratchet/probability/microtiming → deterministic voice/output behavior | `R-XX-drum-behavior-trace.md` |
| Parameter-lock trace | Step destination/value → runtime automation node / ParamId-bound consumer | `R-XX-sequencer-paramlock-path.md` |
| Front-panel pattern handoff trace | AVR block transfer → STM32 decode → runtime pattern state effect | `R-XX-front-panel-pattern-handoff-map.md` |
| Sequencer runtime contract trace | Observed legacy fact → Preserve/Replace/Isolate/Remove/Open Decision V3 contract clause | `R-XX-sequencer-v3-contract.md` |

---

## LXR-Specific Risk Categories

Add these to the risk register in addition to the drum/groovebox profile risks:

| Category | Scope | Typical Sources |
|----------|-------|-----------------|
| `SEQUENCER_RUNTIME` | Sequencer transport state, step evaluation, pattern runtime, parameter-lock dispatch, active/edit/staged pattern ownership | R-8b, R-10 |
| `FRONT_PANEL` | AVR↔STM32 protocol desync, menu state corruption, display timing, PROGMEM overflow | R-5b, R-8d |

---

## LXR V3 Refactor Readiness Gate (R-10b)

The LXR project overlay adds R-10b as a V3-specific readiness gate.

**Goal:** Evaluate generated parameter registry feasibility, protocol/storage/memory blockers, and safe V3 implementation order.

**Classification levels:**
- Ready
- Blocked by missing evidence
- Blocked by byte-width contract
- Blocked by AVR/STM32 protocol coupling
- Blocked by storage format
- Blocked by timing or memory uncertainty
- Blocked by ownership ambiguity

**Inputs:**
- R-5b menu metadata and PROGMEM footprint
- R-6c parameter identity and runtime binding
- R-6d runtime patch/preset-facing state
- R-8d front-panel protocol
- R-9b pattern/automation storage width
- R-9c track / engine topology and writer precedence
- R-10 coupling map and trace matrix

**Outputs:**
- Generated parameter registry feasibility report (`docs/reference/process/generated-param-registry-feasibility.md`)
- V3 refactor readiness report (`docs/reference/process/v3-refactor-readiness.md`)
- Safe implementation order

---

## LXR-Specific Artifact Paths

These artifacts are specific to the LXR / LXRTTL project:

| Artifact | Output Document |
|----------|----------------|
| AVR menu metadata map | `docs/reference/subsystems/XX-parameter-system.md` |
| Front-panel protocol command table | `docs/specifications/31-front-panel-protocol.md` |
| AVR/STM32 constant sync audit | `docs/reference/process/generated-param-registry-feasibility.md` |
| Sequencer transport state map | `docs/reference/subsystems/32-sequencer-transport-pattern-runtime.md` |
| Pattern runtime ownership map | `docs/reference/subsystems/32-sequencer-transport-pattern-runtime.md` |
| Step evaluation / trigger dispatch trace | `docs/reference/subsystems/32-sequencer-transport-pattern-runtime.md` |
| Sequencer parameter-lock contract | `docs/specifications/32-sequencer-runtime-contract.md` |
| Sequencer runtime contract | `docs/specifications/32-sequencer-runtime-contract.md` |
| Generated parameter registry feasibility | `docs/reference/process/generated-param-registry-feasibility.md` |
| V3 refactor readiness report | `docs/reference/process/v3-refactor-readiness.md` |

---

## LXR Dependency Graph Additions

```
R-5 → R-5b (AVR menu metadata)
R-5b, R-6c, R-7 → R-8d (front-panel protocol)
R-6c, R-7, R-8/R-8d → R-8b (sequencer runtime)
R-6b, R-6c, R-8/R-8d, R-8b, R-9/R-9b → R-9c (track topology / writer precedence)
R-10 → R-10b (V3 readiness gate)
```

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial overlay extracted from `stage1-master-schema-corrected.md` v5.8 |

