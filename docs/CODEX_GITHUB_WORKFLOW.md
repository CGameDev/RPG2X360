# RPG2X360 — GitHub-First Codex Workflow

This document is additive repository coordination guidance. It does not replace the RMXP-first v2.0 engineering requirements.

## Canonical repository

`https://github.com/CGameDev/RPG2X360`

GitHub is the durable source of truth. Codex should normally work from a local clone so it can use the developer machine's installed .NET/Visual Studio/Unity/Xbox development tools without committing proprietary SDK material.

## Required reading order

Before implementing the active milestone, Codex must read:

1. `AGENTS.md`
2. `CODEX_MASTER_IMPLEMENTATION_PROMPT.md`
3. `RPG2X360_RMXP_First_Master_Milestone_v2.0.md`
4. `RPG2X360_Milestone_Checklist.md`
5. `MILESTONE_STATUS.md`
6. every technical document referenced by the active milestone

Codex must inspect the current repository and local toolchain before assuming paths, versions or prior completion state.

## Local workflow

```text
GitHub repository
    ↓ clone / pull
Local RPG2X360 working copy
    ↓
Codex
    ├─ source changes
    ├─ WPF/.NET build and tests
    ├─ Unity 5.4.1f work when its milestone is active
    ├─ local authorized Xbox toolchain when its milestone is active
    └─ evidence / milestone reports
    ↓
meaningful Git commits
    ↓
push milestone branch
    ↓
review / test gate
    ↓
merge to main
```

## Branch discipline

For substantial milestones use a branch such as:

`milestone/m00-foundation-safety-contract`

Do not use a later milestone branch to bypass an incomplete current gate.

## Milestone execution

The source package explicitly says to start with M00 only. A normal Codex session should therefore execute `commands/M00_BOOTSTRAP.txt` and stop after the M00 completion report unless the project owner explicitly authorizes continuation.

For every milestone:

- preserve existing user work;
- state affected components and test plan before coding;
- implement only the active milestone scope;
- build all affected host projects;
- run appropriate unit/regression/safety tests;
- record accepted/partial/failed criteria;
- update `MILESTONE_STATUS.md`;
- place supporting evidence under `evidence/` where appropriate;
- commit the checkpoint;
- do not claim Xbox hardware success without hardware evidence.

## Repository vs local-only material

Commit:

- source code;
- schemas;
- synthetic fixtures that are legal to redistribute;
- documentation;
- build configuration templates;
- milestone reports;
- compatibility evidence that contains no proprietary game content;
- runtime source intended for the project.

Keep local/ignored:

- Xbox XDK/SDK files;
- signing keys;
- proprietary middleware;
- RPG Maker RTP content;
- commercial/community game projects used for local compatibility testing;
- credentials/secrets;
- generated caches/build outputs unless a release process explicitly requires an artifact.

## Recovery rule

A future Codex session must be able to resume from GitHub without relying on an old chat session. Therefore confirmed architecture decisions, compatibility limitations, milestone completion evidence and next-action notes belong in the repository.
