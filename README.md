# RPG2X360 Porting Toolkit — RMXP-First Codex Project Pack v2.0

**Project:** RPG2X360 Porting Toolkit  
**Primary source engine:** RPG Maker XP (RGSS1)  
**Host application:** C# / WPF / MVVM on .NET 10 LTS  
**Runtime target:** Unity 5.4.1f Xbox 360 project  
**Console target:** RGH/JTAG Xbox 360 through the user's authorized Xbox 360 development toolchain  
**Package version:** 2.0  
**Date:** August 27, 2026

## What changed from v1.0

Version 1.0 treated RPG Maker MV as the first importer and deferred RPG Maker XP/RGSS to a future research milestone. Version 2.0 reverses that priority and makes **RPG Maker XP the primary implementation target from the first importer milestone onward**.

The project is also re-scoped from a generic data converter into a disciplined porting pipeline with four explicit products:

1. a safe **RMXP project reader** that never executes the source game;
2. a **compatibility analyzer** for maps, events, assets and RGSS scripts;
3. a versioned **RPG2X360 Intermediate Representation (IR)**;
4. a **Unity 5.4.1f Xbox 360 runtime/export target** capable of progressively reproducing supported RMXP behavior.

## Non-negotiable engineering rule

RPG2X360 must **never execute `Game.exe`, load the project's RGSS DLL, or evaluate arbitrary Ruby/RGSS code while importing or analyzing a source game**. Source games are treated as untrusted data. `.rxdata` is decoded statically and `Scripts.rxdata` is extracted/decompressed for analysis only.

## Core recommendation implemented in v2.0

Do **not** begin by promising a universal Ruby-to-C# transpiler. RGSS1 games can contain monkey patches, dynamic method definitions, `eval`, Win32/native calls and scripts that replace large portions of the stock engine. Instead, RPG2X360 uses compatibility tiers:

- **Tier A — Native IR:** behavior represented directly by the RPG2X360 data/event model.
- **Tier B — Auto-translatable RGSS subset:** deterministic Ruby constructs that can be converted safely.
- **Tier C — Runtime shim:** calls mapped to a known Xbox runtime equivalent.
- **Tier D — Generated manual port stub:** script is extracted, explained and scaffolded for a developer to port.
- **Tier E — Unsupported/blocking:** behavior cannot currently be represented safely.

The compatibility report must make these distinctions visible and must never silently remove unsupported functionality.

## Package contents

- `RPG2X360_RMXP_First_Master_Milestone_v2.0.md` — master implementation roadmap and requirements.
- `AGENTS.md` — permanent Codex engineering rules.
- `CODEX_MASTER_IMPLEMENTATION_PROMPT.md` — handoff prompt and milestone execution policy.
- `commands/M00_BOOTSTRAP.txt` — first Codex command.
- `docs/ARCHITECTURE.md` — component architecture and boundaries.
- `docs/RMXP_DATA_MODEL_AND_IMPORT.md` — RMXP detection, Marshal/RXDATA and source import requirements.
- `docs/RGSS_COMPATIBILITY_STRATEGY.md` — RGSS extraction, static analysis, translation tiers and shim strategy.
- `docs/INTERMEDIATE_FORMAT_SPEC.md` — versioned IR contract between the Windows converter and Unity runtime.
- `docs/UNITY_XBOX360_RUNTIME_SPEC.md` — Unity 5.4.1f runtime responsibilities and Xbox 360 constraints.
- `docs/UI_UX_SPEC.md` — WPF workflow and compatibility-centric interface.
- `docs/TESTING_AND_ACCEPTANCE.md` — fixture, regression and acceptance policy.
- `docs/WORKFLOW_DIAGRAM.md` — Mermaid and ASCII workflow diagrams.
- `diagrams/RPG2X360_Workflow.png` / `.svg` — rendered workflow diagram.
- `RPG2X360_Project_Structure.txt` — recommended repository structure.
- `RPG2X360_Milestone_Checklist.md` — concise milestone checklist.
- `schemas/rpg2x-project.schema.json` — starter JSON Schema for exported project manifests.
- `milestone_manifest.json` — machine-readable roadmap summary.

## First Codex action

Start with **M00 only**. Do not skip directly to RGSS conversion or Unity runtime work. The first gate establishes repository structure, safety rules, deterministic test fixtures and the source-project read-only contract.
