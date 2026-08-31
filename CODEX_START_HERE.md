# RPG2X360 — Codex Start Here

## Canonical repository

`https://github.com/CGameDev/RPG2X360`

## Current implementation target

**M00 — Repository Foundation & Safety Contract**

GitHub tracking issue: **#1**

Recommended implementation branch:

`milestone/m00-foundation-safety-contract`

## Read before changing code

Read these files in this exact order:

1. `AGENTS.md`
2. `CODEX_MASTER_IMPLEMENTATION_PROMPT.md`
3. `RPG2X360_RMXP_First_Master_Milestone_v2.0.md`
4. `RPG2X360_Milestone_Checklist.md`
5. `docs/ARCHITECTURE.md`
6. `docs/RMXP_DATA_MODEL_AND_IMPORT.md`
7. `docs/RGSS_COMPATIBILITY_STRATEGY.md`
8. `docs/INTERMEDIATE_FORMAT_SPEC.md`
9. `docs/TESTING_AND_ACCEPTANCE.md`
10. `docs/CODEX_GITHUB_WORKFLOW.md`
11. `MILESTONE_STATUS.md`
12. `commands/M00_BOOTSTRAP.txt`

Do not begin implementation until this reading pass and a repository/local-toolchain inspection are complete.

## Exact first execution instruction

Execute the contents of `commands/M00_BOOTSTRAP.txt`.

That command intentionally limits the first implementation run to **M00 only**.

## Non-negotiable project identity

RPG2X360 v2.0 is an **RMXP-first** porting toolkit:

```text
RPG Maker XP source project
        ↓
read-only static source analysis
        ↓
RXDATA / Ruby Marshal decoding
        ↓
RMXP database, map and event reconstruction
        ↓
static RGSS1 analysis
        ↓
compatibility classification
        ↓
versioned RPG2X360 IR
        ↓
Unity 5.4.1f compatibility runtime
        ↓
authorized local Xbox 360 build toolchain
        ↓
RGH/JTAG Xbox 360 validation
```

Do not silently substitute RPG Maker MV/MZ/VX/VX Ace as the initial target.

## Safety rules

During importer/converter operation Codex must preserve the following architecture:

- source RMXP projects are read-only/untrusted input;
- do not execute `Game.exe`;
- do not load/execute the project's RGSS DLL;
- do not evaluate arbitrary Ruby/RGSS source;
- do not execute source native DLLs;
- do not silently discard unsupported content;
- do not commit RPG Maker RTP/RGSS runtime content;
- do not commit Xbox SDK/XDK binaries, signing material or licensed middleware.

## Git workflow

1. Pull/clone the current repository.
2. Check out `milestone/m00-foundation-safety-contract` for M00 work.
3. Preserve all existing user/repository changes.
4. Implement M00 according to the master milestone.
5. Build/test all affected host projects.
6. Update `MILESTONE_STATUS.md`.
7. Write the M00 completion report under `evidence/M00/`.
8. Commit meaningful checkpoint(s).
9. Push the milestone branch.
10. STOP after M00. Do not begin M01 without explicit authorization.

## Completion reporting

At the M00 gate report:

- branch;
- commit SHA;
- files created/changed;
- build result;
- unit/regression/safety-test result;
- evidence that source-project mutation is prevented;
- evidence that no executable/Ruby execution path exists in the importer/analyzer foundation;
- known limitations;
- exact next action for M01.

If a requirement appears ambiguous, consult the master milestone and technical documents before making an assumption. If the repository does not contain enough evidence to resolve it, record the gap instead of silently choosing new product behavior.
