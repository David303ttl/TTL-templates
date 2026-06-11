<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Drum Synth / Groovebox Profile

> **Profile ID:** `RE-PROFILE-DRUM-GROOVEBOX-01`
> **Version:** `1.0`
> **Date:** `2026-06-11`
> **Status:** `Active`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
>
> **Purpose:** Profile for drum synthesizer and groovebox firmware — per-track/per-instrument engines, step sequencing, choke groups, accent/velocity, pattern runtime, and track-level output routing. This profile is additive: combine with `fm-profile.md`, `sample-based-profile.md`, or `subtractive-profile.md` when individual tracks use different synthesis methods.

---

## When to Enable

Enable this profile when the firmware implements:

- Multiple tracks, instruments, or lanes with independent sound engines
- Step sequencer with track lanes, pattern memory, and parameter locks
- Drum-specific behaviors: choke groups, accent, mute/solo, flam/roll
- Per-track output routing and FX sends

---

## Required Artifacts

| Artifact | Description | Producing Phase |
|----------|-------------|-----------------|
| Track / instrument / lane inventory | Track IDs, public names, engine type, trigger sources, voice/choke rules, parameter namespace, UI/MIDI/sequencer/storage bindings, output routing, FX sends, mute/solo/accent behavior | R-6b |
| Engine variant registry | Engine IDs/names, used-by tracks, render entry, state structs, tone/noise/transient paths, envelopes, filters/drive, output format, CPU risk, storage fields | R-6b |
| Parameter writer precedence matrix | All writers (defaults/load, UI, MIDI/SysEx, p-lock, automation, LFO/modulation, calibration), timing domains, conflict rules, smoothing, applied-at timing | R-6c |
| Sequencer transport state map | Running state, playhead, tick counters, external sync gate, start/stop/continue reaction | R-8b |
| Pattern runtime ownership map | Active/edit/staged pattern bank, UI mirror, temp pattern, switch lifecycle | R-8b |
| Step evaluation / trigger dispatch trace | Step scheduler, rolls, probability, note ownership, trigger command boundary | R-8b |
| Sequencer parameter-lock contract | Step-local destination/value path and canonical ParamId requirement | R-8b |
| Pattern / automation width audit | Pattern step payloads, automation destination widths, persistence risks | R-9b |

---

## R-6b Task Additions

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Track / instrument / lane inventory | Every track ID, public name, engine binding, trigger source, parameter namespace, UI/MIDI/seq/storage binding, output bus, FX sends, mute/solo/accent behavior | Track / instrument / lane inventory |
| Engine variant registry | Per-engine render entry, state struct, tone/noise/transient source, envelopes, filter/drive path, modulation inputs, output format, CPU risk, storage fields | Engine variant registry |
| Drum behavior ordering | Deterministic order for accent, mute, solo, choke, flam/roll/ratchet/probability/microtiming effects on voice/output behavior | Drum behavior trace |
| Per-track output routing | Track → main/sub/FX bus routing, send levels, pan law, mute/bypass gating | Mixer doc |
| FX send per track | Send level, send point (pre/post fader), bus assignment | Effects topology doc |

---

## R-8b Task Additions (Sequencer / Transport / Pattern Runtime)

| Task | Focus | Expected Artifact |
|------|-------|-------------------|
| Transport state | Running state, playhead, tick counters, external sync gate | Sequencer transport state map |
| Step evaluation | Step active/probability/length/rotation/roll evaluation → trigger decision | Step evaluation trace |
| Track lane event path | Step/pad/MIDI event → track/lane resolution → engine binding → trigger dispatch → output path | Track lane event trace |
| Parameter lock | Step destination/value → runtime automation node / ParamId-bound consumer | Sequencer parameter-lock contract |
| Pattern runtime ownership | Active, edit, staged, temp pattern; switch lifecycle; atomicity assumptions | Pattern runtime ownership map |
| Trigger dispatch | Sequencer/UI/MIDI preview trigger boundary → voice trigger command path | Trigger dispatch trace |

---

## Minimum Happy-Path Trace Additions

Add to the E2E trace matrix:

1. Sequencer step → track lane → trigger dispatch → instrument engine → envelope start → mixer → DAC
2. MIDI note → track resolution → trigger dispatch → instrument engine → mixer → DAC
3. Front-panel pad/trigger → track selection → trigger dispatch → instrument engine → mixer → DAC
4. Step accent → velocity/accent scaling → envelope/gain/modulation consumer → output level
5. Step parameter lock → destination resolution → per-track parameter write → smoothing/update timing → DSP consumer
6. Track mute → trigger suppression or audio gate → mixer/output behavior
7. Track solo → mute mask computation → trigger/audio routing behavior
8. Choke group event → previous voice termination → new voice start → output behavior
9. Pattern switch → staged pattern → active pattern → per-track state transition
10. Kit load → per-track engine/parameter state → runtime voice defaults → audio behavior
11. Track output routing → main/sub/FX bus → DAC/output path
12. FX send per track → send bus → effect buffer → return mix → output

---

## Minimum Negative-Path Trace Additions

Add to the E2E trace matrix negative-path inventory:

1. Invalid track ID → rejected/default/fallback behavior
2. Invalid engine ID → rejected/default/fallback behavior
3. Parameter lock targets parameter not valid for that track engine → compatibility behavior
4. Pattern references missing/unsupported kit or engine → load recovery
5. Choke/mute/solo race with active trigger → deterministic ordering
6. Pattern switch during sustained/choked voice → voice lifecycle behavior
7. Step event exceeds scheduler budget → skip/drop/late-trigger behavior
8. Per-track output bus unavailable/misconfigured → fallback routing

---

## Drum/Groovebox-Specific Risk Additions

| Category | Risk |
|----------|------|
| `SEQUENCER_RUNTIME` | Pattern switch race, step budget overrun, transport stall, parameter-lock aliasing |
| `ENGINE` | Choke group not terminating old voice, accent scaling overflow, engine variant CPU variance, mute/solo state inconsistency |
| `INTEGRATION` | Track topology gap between R-6, R-8, R-8b, and R-9 docs; engine/track binding mismatch; writer precedence conflict |
| `PARAMETER_MODEL` | Parameter lock destination not in ParamId contract; per-track parameter namespace collision |

---

## Track / Instrument / Lane Inventory Schema

```markdown
| Track ID | Public Name | Engine Type | Source Files | Trigger Sources | Voice/Choke Rules | Parameter Namespace | UI Binding | MIDI Binding | Sequencer Binding | P-Lock Support | Storage Fields | Output Bus | FX Sends | Mute/Solo/Accent | Risks |
|----------|-------------|-------------|--------------|-----------------|-------------------|---------------------|------------|--------------|-------------------|----------------|----------------|------------|----------|------------------|-------|
| T1 | Kick | [engine] | `file:line` | MIDI / seq / UI | [rule] | [ParamId range] | [menu/page] | [note/CC/ch] | [lane/step] | Yes/No/[limited] | [kit fields] | Main | [send] | [behavior] | [RISK-ID] |
```

## Engine Variant Registry Schema

```markdown
| Engine ID | Engine Name | Used By Tracks | Source Files | Render Entry | State Struct | Osc/Tone Source | Noise Source | Click/Transient | Envelopes | Filter/Drive | Mod Inputs | Output Format | CPU Risk | Storage Fields |
|-----------|-------------|----------------|--------------|--------------|--------------|-----------------|--------------|-----------------|-----------|--------------|------------|---------------|----------|----------------|
| [id] | [name] | T1,T2 | `file:line` | `render()` | `[struct]` | [source] | [source] | [source] | [EGs] | [filter/drive] | [params] | [format] | Low/Med/High | [fields] |
```

## Parameter Writer Precedence Matrix Schema

```markdown
| Target Parameter | Writers | Timing Domain | Conflict Rule | Smoothing | Applied At | Evidence |
|------------------|---------|---------------|---------------|-----------|------------|----------|
| Track cutoff | UI, MIDI CC, p-lock, LFO | control/event/audio | p-lock overrides base for step duration; UI updates base | Yes/No/[method] | step boundary/block/sample | `file:line` |
```

---

## Example Applicable Synths

| Synth | Notes |
|-------|-------|
| **LXR** | Drum synth with FM/hybrid engines, step sequencer, parameter locks |
| **LXRTTL** | LXR variant |

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-06-11 | Initial profile extracted from `stage1-master-schema-corrected.md` v5.8 |

