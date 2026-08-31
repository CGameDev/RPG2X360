# RPG2X360 — Codex Repository Instructions

## Authority

This repository is the canonical source of truth for the RPG2X360 project.

The project milestone and specifications migrated from `RPG2X360_RMXP_First_Codex_Project_Pack_v2.0` are authoritative for the first implementation effort. Codex MUST read the repository documentation and active milestone before changing source code.

If a repository document and an assumption conflict, the repository document wins.

## Core project rule

RPG2X360 is not permission to redesign the requested workflow or silently substitute a different RPG Maker generation, runtime, export target, architecture, or development stack.

For the RMXP milestone:

- RPG Maker XP / RMXP is the required source-project target unless the milestone explicitly says otherwise.
- Preserve the milestone's conversion workflow, compatibility model, deliverables, validation gates, and Xbox 360 target strategy.
- Do not silently switch the first implementation target to RPG Maker MV, VX, VX Ace, MZ, or another engine generation.
- Do not invent missing requirements. Record a requirement gap and continue with work that is unblocked.
- Do not delete milestone documentation when implementation begins.

## Work discipline

Before implementation:

1. Read this `AGENTS.md`.
2. Read the root `README.md`.
3. Read the active milestone and all documents it names as required reading.
4. Inspect the current repository instead of assuming project state.
5. Inspect the local development environment instead of inventing installed SDK/tool paths.
6. Record material environment findings in repository documentation when the milestone requires it.

During implementation:

- Work in small, coherent checkpoints.
- Keep architecture separable between project analysis/import, intermediate representation/conversion, export/runtime generation, UI, and platform-specific code where specified by the milestone.
- Prefer deterministic conversion and auditable compatibility reporting over hidden best-effort behavior.
- Preserve unsupported/partial/converted status reporting where required.
- Keep generated build output, local SDK files, credentials, proprietary toolchains, caches, and machine-specific artifacts out of Git.
- Do not commit Xbox 360 XDK files or other non-redistributable SDK components.
- Do not claim target-hardware success without target-hardware evidence.
- Do not mark a milestone complete while required acceptance criteria are unresolved.

## Testing rule

Batch meaningful implementation work before expensive/manual target-hardware testing when the active milestone permits it. Do not replace required validation with superficial smoke tests.

A passing desktop/unit test does not prove Xbox 360 runtime compatibility.

## Documentation rule

When implementation reveals a confirmed constraint, compatibility limitation, architectural decision, or toolchain requirement, update the relevant documentation in the same workstream so future Codex sessions do not have to rediscover it.

## Scope protection

Do not:

- rewrite the project into an unrelated game engine;
- collapse the conversion pipeline into UI code;
- hide unsupported RPG Maker constructs;
- fabricate successful conversion of unsupported scripts/plugins/assets;
- commit proprietary SDK/toolchain binaries;
- overwrite working milestone checkpoints without preserving history;
- start later milestones merely because implementation is possible when the current milestone's acceptance gates are incomplete.

## Git workflow

Use meaningful commits and keep `main` in a recoverable state. For substantial implementation milestones, prefer a milestone/feature branch and a reviewable merge back to `main`.

Repository documentation is part of the product and must remain versioned alongside source code.
