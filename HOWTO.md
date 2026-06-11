# Using the RE Schema Repository as a Git Submodule

This repository is meant to be consumed from a host project as a read-only Git submodule. The submodule provides the reusable RE schema; the host project owns all project-specific evidence, manifests, plans, and implementation.

## 1. Add the submodule

From the root of your host project:

```bash
mkdir -p external
git submodule add https://github.com/David303ttl/RE-schema.git external/RE-schema
git commit -m "Add RE schema submodule"
```

If you prefer a different path, change it once and keep every reference consistent.

To clone a host project with the submodule already initialized:

```bash
git clone --recurse-submodules <host-repo-url>
```

If the host project was cloned without submodules:

```bash
git submodule update --init --recursive
```

To inspect submodule state:

```bash
git submodule status
```

To update the submodule intentionally, pin it to a tag or commit:

```bash
cd external/RE-schema
git fetch --tags
git checkout <tag-or-commit>
cd ../..
git add external/RE-schema
git commit -m "Bump RE schema submodule"
```

## 2. Repository Roles

| Location | Role |
| --- | --- |
| `external/RE-schema/` | Read-only schema source. Contains `RE-schema/`, reusable profiles, and validation checklists. |
| `projects/<project>/` | Host-project profile-selection manifest and any companion metadata. |
| `docs/reference/` | Host-project reverse-engineering evidence, traces, risks, plans, and handoff artifacts. |
| `.agent/` | Optional agent instructions for the host project. |

Do not edit files inside the submodule unless you are updating the schema repository itself. Any project-specific artifact belongs in the host project.

## 3. Create the Host-Project Workspace

Mirror the artifact set that the schema expects. A minimal layout looks like this:

```text
projects/
  <project>/
    profile-selection.md

docs/
  reference/
    architecture/
      07-clock-and-timing-map.md
      10-coupling-map.md
    process/
      file-manifest.md
      risk-register.md
      trace-plan.md
      e2e-trace-matrix.md
      stage2-risk-resolution-plan.md
      q0-static-analysis.md
      q1-solid-review.md
      q2-bug-catalog.md
      q3-race-condition-audit.md
      q4-refactoring-plan.md
      q5-test-strategy.md
      phase-r10-evidence/
        README.md
        R-10-q0-handoff.md
        R-10-verification-report.md

.agent/
  stage1-agent-instructions.md
  stage2-agent-instructions.md
  stage3-agent-instructions.md
```

If your host project already has a different docs layout, keep the same artifact roles and update the paths consistently.

## 4. Instantiate the Profile-Selection Manifest

Start from the manifest scaffold in the submodule:

- `external/RE-schema/RE-schema/projects/README.md`
- `external/RE-schema/RE-schema/projects/profile-selection.example.yaml`

The host project should keep the active manifest in `projects/<project>/profile-selection.md` or an equivalent path owned by the host repo.

The manifest should record:

- the submodule path
- the exact schema commit or tag
- the selected architecture profiles
- the selected project overlays
- the selected future-work overlays
- every explicitly non-applicable profile and its justification

Reference the schema files from the submodule, for example:

- `external/RE-schema/RE-schema/RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`
- `external/RE-schema/RE-schema/RE-STAGE2-QUALITY-AUDIT-MASTER.md`
- `external/RE-schema/RE-schema/profiles/README.md`

## 5. Stage 1

Stage 1 is discovery only. Do not change host-project source code in this phase.

Stage 1 artifacts live in the host project and should remain evidence-focused:

- `docs/reference/process/file-manifest.md`
- `docs/reference/process/risk-register.md`
- `docs/reference/process/trace-plan.md`
- `docs/reference/process/e2e-trace-matrix.md`
- `docs/reference/architecture/10-coupling-map.md`
- `docs/reference/architecture/07-clock-and-timing-map.md`
- `docs/reference/process/phase-r10-evidence/R-10-q0-handoff.md`
- `docs/reference/process/phase-r10-evidence/R-10-verification-report.md`

Open questions belong in the risk register unless your host project explicitly adds a separate backlog file.

Use the Stage 1 master from the submodule:

`external/RE-schema/RE-schema/RE-STAGE1-EMBEDDED-SYNTH-MASTER.md`

## 6. Stage 2

Stage 2 consumes Stage 1 output and turns it into risk resolution, validation planning, and readiness decisions. Stage 2 does not implement product changes.

Stage 2 artifacts live in the host project and should remain decision-focused:

- `docs/reference/process/stage2-risk-resolution-plan.md`
- `docs/reference/process/q0-static-analysis.md`
- `docs/reference/process/q1-solid-review.md`
- `docs/reference/process/q2-bug-catalog.md`
- `docs/reference/process/q3-race-condition-audit.md`
- `docs/reference/process/q4-refactoring-plan.md`
- `docs/reference/process/q5-test-strategy.md`

Use the Stage 2 master from the submodule:

`external/RE-schema/RE-schema/RE-STAGE2-QUALITY-AUDIT-MASTER.md`

Do not start implementation until the Stage 2 readiness gate explicitly allows it.

## 7. Stage 3

Stage 3 is the first phase where host-project code changes are allowed.

Rules for Stage 3:

- Modify only files required by the approved work package.
- Keep changes traceable to Stage 1 evidence and Stage 2 decisions.
- Update documentation when behavior or architecture changes.
- Add or update tests required by Stage 2.
- Avoid opportunistic refactors.

## 8. Agent Rules

When using a coding agent, keep these rules in the prompt:

1. Treat `external/RE-schema/` as read-only.
2. Keep all project-specific artifacts in the host repository.
3. Stage 1 is evidence gathering only.
4. Stage 2 is risk resolution and readiness planning only.
5. Stage 3 is the first implementation phase.
6. Every factual claim should cite source evidence.
7. Every unknown should become an open question.
8. Every unsafe area should become a risk-register entry.
9. Every code change should trace back to Stage 1 evidence and Stage 2 readiness.
10. Do not edit vendor, generated, binary, or third-party files unless explicitly authorized.

## 9. High-Level Workflow

```text
Add the schema repository as a submodule
  -> create the host-project workspace
  -> instantiate the profile-selection manifest
  -> run Stage 1
  -> run Stage 2
  -> approve a specific Stage 3 work package
  -> implement, test, document, and review only that approved work package
```

Core rule:

```text
Stage 1 discovers.
Stage 2 decides.
Stage 3 changes code.
```
