<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Schema Validation Checklist

> **Scope:** `RE-schema/` and project manifest scaffolds
> **Purpose:** Manual validation before publishing a new schema revision or instantiated manifest.

## 1. Master Schema Checks

- [ ] Stage 1 master file exists and is referenced correctly
- [ ] Stage 2 master file exists and is referenced correctly
- [ ] Stage boundary rules are explicit
- [ ] Stage 1 remains observational and read-only
- [ ] Stage 2 remains decision-oriented and non-implementing

## 2. Profile Selection Checks

- [ ] Every enabled architecture profile exists at the referenced path
- [ ] Every enabled project overlay exists at the referenced path
- [ ] Every enabled future-work overlay exists at the referenced path
- [ ] Every non-applicable profile has a source-backed justification
- [ ] Any overlapping profile requirements have a conflict-resolution entry

## 3. Manifest Pairing Checks

- [ ] Markdown manifest and YAML companion agree on project name
- [ ] Markdown manifest and YAML companion agree on master schema versions
- [ ] Markdown manifest and YAML companion agree on enabled profiles
- [ ] Markdown manifest and YAML companion agree on non-applicable profiles
- [ ] Markdown manifest and YAML companion agree on conflict resolution entries

## 4. Repository Layout Checks

- [ ] `RE-schema/` contains reusable master schemas and profile files
- [ ] `docs/` contains the reusable scaffold for project documentation
- [ ] `examples/` contains example instantiations only, not authoritative schema files

## 5. Output Checks

- [ ] Root README links Stage 1 master, Stage 2 master, docs scaffold, and validation checklist
- [ ] Project README explains the Markdown manifest and the YAML companion
- [ ] Stage 2 entry-control document is linked from the Stage 1 master


