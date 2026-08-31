# RPG2X360 v2.0 Project Pack — GitHub Migration Audit

## Source pack

`RPG2X360_RMXP_First_Codex_Project_Pack_v2.0.zip`

The RMXP-first v2.0 project pack is the source used to initialize this repository for Codex.

## Integrity result

The original source-pack text/specification files were migrated into the repository without rewriting their content. Git blob identities were checked against the extracted source pack for the canonical files, including:

- `AGENTS.md`
- `CODEX_MASTER_IMPLEMENTATION_PROMPT.md`
- `README.md`
- `RPG2X360_Milestone_Checklist.md`
- `RPG2X360_Project_Structure.txt`
- `RPG2X360_RMXP_First_Master_Milestone_v2.0.md`
- `commands/M00_BOOTSTRAP.txt`
- `docs/ARCHITECTURE.md`
- `docs/INTERMEDIATE_FORMAT_SPEC.md`
- `docs/RESEARCH_NOTES.md`
- `docs/RGSS_COMPATIBILITY_STRATEGY.md`
- `docs/RMXP_DATA_MODEL_AND_IMPORT.md`
- `docs/TESTING_AND_ACCEPTANCE.md`
- `docs/UI_UX_SPEC.md`
- `docs/UNITY_XBOX360_RUNTIME_SPEC.md`
- `docs/WORKFLOW_DIAGRAM.md`
- `milestone_manifest.json`
- `schemas/rpg2x-project.schema.json`
- `diagrams/RPG2X360_Workflow.dot`
- `diagrams/RPG2X360_Workflow.svg`

The original `SHA256SUMS.txt` is also preserved as the source-pack checksum manifest.

## Workflow PNG note

The source pack also contained:

`diagrams/RPG2X360_Workflow.png`

Source-pack metadata:

- Size: `105375` bytes
- SHA-256: `5a190c790817ddfd047ca1359656861f23866e62a46fa7daceafe79936c5cd2c`
- Git blob identity of the source file: `50f73c234dd2074af5bfdd67ef624c7c38664682`

The GitHub connector used for the migration provides UTF-8 repository file writes, so the binary PNG was not inserted through that path. This does **not** block Codex or the project because the canonical Graphviz source (`RPG2X360_Workflow.dot`) and rendered vector version (`RPG2X360_Workflow.svg`) are present exactly.

If a raster copy is needed locally, regenerate it from the committed DOT source with Graphviz:

```text
dot -Tpng diagrams/RPG2X360_Workflow.dot -o diagrams/RPG2X360_Workflow.png
```

Byte-for-byte equality with the historical PNG is not required for implementation; the diagram is documentation only.

## GitHub-specific additions

The following files were added on top of the original v2.0 pack to make GitHub the project source of truth:

- `.gitignore`
- `CODEX_START_HERE.md`
- `MILESTONE_STATUS.md`
- `docs/CODEX_GITHUB_WORKFLOW.md`
- `docs/MIGRATION_AUDIT.md`
- `provenance/RPG2X360_RMXP_First_Codex_Project_Pack_v2.0_SHA256SUMS.txt`

These additions do not change the v2.0 technical roadmap. They define repository workflow, resumption state and migration provenance.

## GitHub implementation tracking

Active implementation issue:

`#1 — M00 — Repository Foundation & Safety Contract`

Recommended M00 branch:

`milestone/m00-foundation-safety-contract`

Codex should start at `CODEX_START_HERE.md` and execute `commands/M00_BOOTSTRAP.txt`.
