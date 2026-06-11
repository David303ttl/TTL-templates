<!--
SPDX-License-Identifier: Apache-2.0
Copyright (c) 2026 David303ttl
-->

# Reverse-Engineering Schema

Reusable reverse-engineering schema, profiles, and documentation scaffolding for embedded synth/audio firmware.

This repository is intended to be consumed by downstream projects as a read-only Git submodule.

If you are using this repository from another project, start with [HOWTO.md](HOWTO.md).

## About

This repository contains:

- the Stage 1 master schema
- the Stage 2 master schema
- reusable architecture profiles
- project and future-work overlays
- project manifest scaffolding
- validation and documentation scaffold files

## Quickstart

1. Read [RE-STAGE1-EMBEDDED-SYNTH-MASTER.md](RE-schema/RE-STAGE1-EMBEDDED-SYNTH-MASTER.md).
2. Select the matching architecture profile(s) and project overlay(s).
3. Instantiate the project manifest in `RE-schema/projects/<project>/profile-selection.md`.
4. Optionally generate the machine-readable companion from [profile-selection.example.yaml](RE-schema/projects/profile-selection.example.yaml).
5. Use the scaffold in [docs/](docs/) to capture Stage 1 outputs.
6. Close Stage 1 with the R-10 handoff package.
7. Run Stage 2 from [RE-STAGE2-QUALITY-AUDIT-MASTER.md](RE-schema/RE-STAGE2-QUALITY-AUDIT-MASTER.md) and [stage2-risk-resolution-plan.md](docs/reference/process/stage2-risk-resolution-plan.md).
8. Do not start implementation until Q-5 is complete and the readiness decisions are recorded.

## Documentation Scaffold

- [`RE-schema/schema-validation-checklist.md`](RE-schema/schema-validation-checklist.md) for schema and manifest validation
- [`RE-schema/RE-STAGE2-QUALITY-AUDIT-MASTER.md`](RE-schema/RE-STAGE2-QUALITY-AUDIT-MASTER.md) for the Stage 2 master schema
- [`docs/README.md`](docs/README.md) for the current docs tree
- [`examples/README.md`](examples/README.md) for the reserved example workspace
- [`docs/reference/process/stage2-risk-resolution-plan.md`](docs/reference/process/stage2-risk-resolution-plan.md) for the Stage 2 entry-control plan
- [`RE-schema/RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`](RE-schema/RE-STAGE1-EMBEDDED-SYNTH-MASTER.md) for the master Stage 1 schema

## License

This project is licensed under Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Author

**David303ttl** - [GitHub Profile](https://github.com/David303ttl/)

