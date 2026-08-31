# Codex Master Implementation Prompt — RPG2X360 v2.0

You are implementing **RPG2X360 Porting Toolkit v2.0**, an RMXP-first Windows converter and Xbox 360 runtime pipeline.

Read, in this order:
1. `AGENTS.md`
2. `RPG2X360_RMXP_First_Master_Milestone_v2.0.md`
3. `docs/ARCHITECTURE.md`
4. `docs/RMXP_DATA_MODEL_AND_IMPORT.md`
5. `docs/RGSS_COMPATIBILITY_STRATEGY.md`
6. `docs/INTERMEDIATE_FORMAT_SPEC.md`
7. `docs/TESTING_AND_ACCEPTANCE.md`

Then inspect the repository before making changes.

## Mandatory operating constraints
- Treat every imported RMXP project as untrusted read-only input.
- Do not execute `Game.exe`, RGSS DLLs or Ruby code.
- Do not implement a general-purpose Ruby interpreter as part of the first release.
- Do not silently discard unsupported events/scripts/assets.
- Keep the WPF UI independent from import/conversion logic.
- Keep the Unity 5.4.1f runtime behind the serialized RPG2X360 IR boundary.
- Do not include proprietary Xbox or RPG Maker runtime files.
- Do not start the next milestone until the current milestone's acceptance gate is reported.

## Development approach
Build the safest deterministic path to a playable RMXP vertical slice first:

`Detect -> Decode RXDATA -> Import DB/Maps/Events -> Extract RGSS -> Analyze -> Create IR -> Export -> Unity Runtime -> Xbox 360 Smoke Test`

Event-command and stock-engine behavior should be represented in the IR directly when possible. RGSS should be handled with explicit compatibility tiers and a growing shim catalog rather than speculative full-language translation.

## First execution
Perform **M00 — Repository Foundation & Safety Contract only**. At completion, report:
- repository path and branch;
- files created/changed;
- build/test results;
- safety-gate evidence;
- known limitations;
- exact next action for M01.

Stop after the M00 report.
