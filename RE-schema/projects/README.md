<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Project Profile-Selection Manifests

> **Directory:** `projects/`
> **Parent Schema:** `RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
> **Version:** `1.1`
> **Date:** `2026-06-11`

---

## What This Directory Contains

Each subdirectory holds a **profile-selection manifest** for a real firmware target. The manifest is the binding document that instantiates the master schema + selected architecture profiles + project overlays into a concrete project configuration.

A manifest answers: "Which profiles apply to this firmware, and why?"

A machine-readable companion may be used for validation and tooling:

- `projects/profile-selection.example.yaml`

---

## Manifest Scaffold

Copy this scaffold to create a new manifest at `projects/<project>/profile-selection.md`:

```markdown
# Profile Selection Manifest — [Project Name]

> **Manifest ID:** `RE-MANIFEST-[PROJECT]-01`
> **Version:** `1.0`
> **Date:** YYYY-MM-DD
> **Status:** `Active`
> **Project:** [Project description]

---

## Selected Masters

| Role | Schema | Version | File |
|------|--------|---------|------|
| Stage 1 master | `RE-STAGE1-EMBEDDED-SYNTH-MASTER` | `X.Y` | `../RE-STAGE1-EMBEDDED-SYNTH-MASTER.md` |
| Stage 2 master | `RE-STAGE2-QUALITY-AUDIT-MASTER` | `X.Y` | `../RE-STAGE2-QUALITY-AUDIT-MASTER.md` |

---

## Selected Architecture Profiles

| Profile | Required? | Reason |
|---|---:|---|
| `subtractive-profile.md` | Yes/No | ... |
| `fm-profile.md` | Yes/No | ... |
| `operator-fm-profile.md` | Yes/No | ... |
| `wavetable-va-profile.md` | Yes/No | ... |
| `sample-based-profile.md` | Yes/No | ... |
| `drum-groovebox-profile.md` | Yes/No | ... |
| `multitimbral-profile.md` | Yes/No | ... |
| `voicecard-satellite-profile.md` | Yes/No | ... |
| `modular-cv-profile.md` | Yes/No | ... |

---

## Selected Project Overlays

| Overlay | Reason |
|---|---|
| `profiles/project/xxx-overlay.md` | ... |

---

## Selected Future-Work Overlays

| Overlay | Enabled? | Reason |
|---|---:|---|
| `refactor-readiness.md` | Yes/No | ... |
| `generated-registry-readiness.md` | Yes/No | ... |

---

## Explicit Non-Applicable Profiles

| Profile | N/A Justification | Evidence Phase |
|---|---|---|
| `xxx-profile.md` | Source-backed justification after relevant discovery phase | R-X |

---

## Conflict Resolution Table

| Conflicting Profiles | Conflicting Requirement | Resolution | Rationale |
|---|---|---|------|
| `profile-a` + `profile-b` | [Describe the overlap] | [Which takes precedence or how they merge] | [Why] |

---

## Dependency Graph Notes

[Any project-specific phase additions, ordering constraints, or topology considerations.]

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | YYYY-MM-DD | Initial manifest |
```

---

## Manifests by Project

| Project | Manifest | Status |
|---------|----------|--------|
| **LXR / LXRTTL** | `lxr/profile-selection.md` | Active |
| **Mutable Instruments Ambika** | `ambika/profile-selection.md` | Active |
| **PreenFM2** | `preenfm2/profile-selection.md` | Active |
| **PreenFM3** | `preenfm3/profile-selection.md` | Active |
| **Jeannie** | `jeannie/profile-selection.md` | Active |

---

## Manifest Validation Rules

Before committing a manifest, verify:

1. The master schema version matches the version actually in use
2. Every enabled profile exists at the referenced path
3. Every non-applicable profile has a source-backed justification and an evidence phase
4. Required? column values are explicit (Yes/No — no blanks or TBD for non-applicable profiles)
5. If two enabled profiles define potentially overlapping requirements, the conflict resolution table has a row
6. Project overlay paths are correct relative to the manifest location
7. Future-work overlays are explicitly enabled or disabled
8. If the YAML companion is present, its schema and enabled profile list match the Markdown manifest

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.1 | 2026-06-11 | Added Stage 2 master to the scaffold, added YAML companion guidance, and expanded validation rules |

