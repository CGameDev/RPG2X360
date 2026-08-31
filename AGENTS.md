# AGENTS.md — RPG2X360 Permanent Engineering Rules

These rules apply to every Codex session in this repository.

## 1. Product definition
RPG2X360 is an **RMXP-first porting toolkit**. It reads RPG Maker XP projects, produces compatibility evidence, converts supported behavior into a versioned intermediate representation, and exports packages for a separate Unity 5.4.1f Xbox 360 runtime.

## 2. Source projects are read-only and untrusted
- Never modify the user's RMXP source project in place.
- Never launch `Game.exe` as part of import, detection, analysis or testing.
- Never load or execute the source RGSS DLL.
- Never `eval`, invoke, interpret or dynamically load Ruby code from `Scripts.rxdata` during converter operation.
- Never run arbitrary native DLLs referenced by a game.
- Copy test inputs into disposable fixture directories when mutation is required by a test.
- Output is written only under an explicitly selected RPG2X360 workspace/output directory.

## 3. No silent compatibility loss
Every source feature must end in a visible status: Supported, AutoTranslated, RuntimeShim, ManualPortRequired, Unsupported, Unknown or Blocking.

If data is not understood, preserve its raw representation or a traceable source reference where legally/technically possible. Do not drop it.

## 4. RGSS scope discipline
Do not claim general Ruby semantic equivalence until a test corpus proves it. Prefer a conservative static subset, explicit runtime shims and generated manual-port stubs.

Dynamic/metaprogramming features such as `eval`, dynamic class mutation, runtime code generation, native/Win32 calls or unknown binary extensions are never auto-translated merely because parsing succeeded.

## 5. Runtime separation
- The Windows converter may use modern .NET 10/C#.
- The Unity 5.4.1f runtime must use language/runtime features compatible with that Unity/Xbox 360 toolchain.
- Do not share compiled modern .NET assemblies directly with Unity 5.4.1f.
- The stable integration boundary is the versioned RPG2X360 IR package/schema.

## 6. Xbox development boundary
Do not add, copy or distribute proprietary Xbox SDK/XDK files, signing keys or licensed middleware. Build integration must use paths/configuration supplied on the authorized development machine.

## 7. Proprietary RPG Maker content
Do not bundle Enterbrain/Gotcha Gotcha Games runtime DLLs, RTP assets or third-party game assets in the repository or fixtures. Tests use synthetic fixtures or user-supplied local content excluded by `.gitignore`.

## 8. Milestone discipline
Implement one milestone at a time. Before coding:
1. inspect repository status and existing user changes;
2. read this file and the current milestone section;
3. state affected components and test plan;
4. preserve source compatibility and previous acceptance gates.

After coding:
1. build all affected projects;
2. run unit/regression tests;
3. record accepted/partial/failed criteria;
4. update docs and compatibility matrix;
5. stop at the milestone gate unless explicitly instructed to continue.

## 9. Determinism and traceability
Every conversion output should include source fingerprints, tool version, IR format version, compatibility summary and deterministic IDs where possible. Two conversions of the same unchanged source with the same settings should be semantically identical.

## 10. Performance priority
Favor streaming parsers, bounded memory use and cancellable async host operations. Xbox runtime code must avoid unnecessary allocations in per-frame paths and should be profiled against real console constraints before optimization claims are accepted.
