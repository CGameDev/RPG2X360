# RPG2X360 Porting Toolkit
## RMXP-First Transpilation & Xbox 360 Runtime Roadmap — v2.0

## 1. Executive objective

Build a Windows application and companion Unity 5.4.1f runtime that can **analyze, convert and progressively reproduce RPG Maker XP games on Xbox 360**.

The word “convert” is intentionally divided into stages. RPG2X360 is not expected to transform arbitrary `Game.exe` binaries into `.xex` files. Instead it reconstructs the game from its project data and supported behavior:

```text
RPG Maker XP source project
        -> static project decoder
        -> source model
        -> compatibility analysis
        -> RPG2X360 IR
        -> Unity 5.4.1f runtime package
        -> Xbox 360 build through authorized local toolchain
```

### Definition of success
A successful v2.x project can take a representative RMXP game that stays within the supported compatibility profile and:
- detect it correctly;
- decode the database, maps, events and assets without modifying the source;
- inventory/extract RGSS scripts without executing them;
- explain compatibility blockers;
- compile supported event behavior into RPG2X360 IR;
- export a deterministic runtime package;
- load that package in Unity 5.4.1f;
- run a basic playable vertical slice on Xbox 360 with controller input, map rendering, events and save data.

## 2. Why RMXP is now first

The original v1.0 roadmap started with RPG Maker MV because JSON made the import stage easier. That is useful for a generic converter, but it does not optimize for this project's main goal. v2.0 makes RPG Maker XP/RGSS1 the first-class target so that the hard architecture—Ruby Marshal decoding, RGSS analysis, map/event reconstruction and RGSS runtime compatibility—is designed correctly from day one.

RPG Maker MV/MZ become future importer modules after the RMXP pipeline is proven.

## 3. Technical baseline

### Windows host
- C#
- WPF
- MVVM
- .NET 10 LTS
- Windows 10 LTSC/Enterprise and Windows 11 x64
- UI-independent import/conversion libraries
- CLI using the same core services

### Source target
- RPG Maker XP
- `Game.rxproj` / `Game.ini`
- `Data/*.rxdata`
- RGSS1 script bundle (`Scripts.rxdata` by default, configurable from `Game.ini`)
- project-local assets and RTP dependency inventory

### Xbox runtime target
- Unity 5.4.1f project compatible with the user's Xbox 360 Unity/XDK environment
- C# features constrained to what that Unity/Mono toolchain can compile
- no dependency on modern .NET host assemblies
- 640x480 logical RMXP game space by default, rendered through a configurable Xbox output presentation layer
- initial 16:9 behavior: preserve 4:3 gameplay semantics with pillarboxing or an explicitly enabled safe widescreen mode; do not stretch blindly

## 4. Product principles

1. **Read-only source.** Never mutate the original project.
2. **Static analysis first.** Do not execute source scripts during conversion.
3. **Evidence over optimism.** Compatibility claims come from tested mappings.
4. **No silent deletion.** Unknown or unsupported features are visible.
5. **Versioned IR boundary.** Host tooling and Unity runtime can evolve independently.
6. **Playable vertical slice before broad engine coverage.** Prove map/event/runtime flow early.
7. **Deterministic exports.** Same source/settings produce equivalent output.
8. **Legal separation.** Do not bundle proprietary RTP/RGSS/XDK components.

## 5. End-to-end workflow

See `docs/WORKFLOW_DIAGRAM.md` and `diagrams/RPG2X360_Workflow.png`.

High-level flow:

```text
Select RMXP Project
  -> Validate / fingerprint / detect dependencies
  -> Decode Ruby Marshal RXDATA
  -> Import DB + maps + events
  -> Extract & decompress Scripts.rxdata
  -> Static RGSS analysis
  -> Compatibility report
  -> Convert supported behavior to RPG2X360 IR
  -> Generate runtime-shim bindings + manual-port stubs
  -> Process/copy assets
  -> Export deterministic RPG2X package
  -> Unity 5.4.1f imports package
  -> Runtime executes maps/events/shims
  -> PC simulation tests
  -> Xbox 360 build & hardware validation
```

## 6. Architecture

Required projects:

```text
RPG2X360.sln
├── RPG2X360.Desktop
├── RPG2X360.Core
├── RPG2X360.Models
├── RPG2X360.Rmxp
├── RPG2X360.Rgss
├── RPG2X360.Analysis
├── RPG2X360.Conversion
├── RPG2X360.Export
├── RPG2X360.Diagnostics
├── RPG2X360.Cli
└── RPG2X360.Tests
```

The Unity runtime is a separate tree/project:

```text
runtime/Unity54/RPG2X360.Runtime.Unity54/
```

No direct project reference from Unity 5.4.1f to modern .NET host assemblies.

## 7. Compatibility taxonomy

Every meaningful source component receives both **status** and **severity**.

### Status
- `Supported`
- `AutoTranslated`
- `RuntimeShim`
- `ManualPortRequired`
- `Unsupported`
- `Unknown`
- `Blocking`

### Severity
- `Info`
- `Warning`
- `Error`
- `Critical`

### RGSS tier
- `A_NativeIR`
- `B_AutoSubset`
- `C_RuntimeShim`
- `D_ManualStub`
- `E_Unsupported`

A project-level readiness score must never hide critical items. The headline readiness is one of:
- Ready for runtime export
- Ready with warnings
- Manual RGSS port required
- Runtime feature missing
- Conversion blocked

## 8. RGSS strategy

### What v2.0 does
- Decode the script archive structure without executing it.
- Decompress individual Ruby script sources.
- Preserve script order and names.
- Tokenize/parse supported Ruby syntax through a safe static parser.
- Build symbol/class/module/method inventories.
- Identify references to known RGSS APIs and RPG Maker classes.
- Detect class reopening/monkey patches.
- Detect dynamic-risk features such as `eval`, dynamic send patterns, native calls and runtime file/network dependencies.
- Generate compatibility items and source-linked manual-port tasks.
- Auto-translate only a deliberately bounded subset.
- Generate C# stub files for Tier D scripts, with source script name, method signature when known and diagnostic IDs.

### What v2.0 does not do
- Promise arbitrary Ruby semantic equivalence.
- Execute Ruby scripts to discover behavior.
- Ship a full Ruby VM on Xbox as a hidden shortcut.
- Mark a script compatible merely because it parses.

### Runtime shim catalog
Create a catalog that grows by tests. Candidate surface areas include:
- input abstraction;
- graphics timing/brightness/transitions;
- audio playback abstraction;
- bitmap/sprite/viewport concepts;
- window/text presentation concepts;
- RPG data-class equivalents;
- cache/resource access;
- game switches/variables/self-switches;
- scene transitions supported by the runtime.

Every shim needs a unique ID, host-side detection rule, runtime implementation reference and tests.

## 9. RMXP data importer

The importer must support:
- project marker and configuration detection;
- `Game.ini` parsing;
- Ruby Marshal object graph decoding;
- preservation of unknown class names/instance variables;
- core database objects;
- map infos and maps;
- map tile tables and map metadata;
- events, pages, conditions, graphics, move routes and event commands;
- common events;
- system settings;
- tileset metadata;
- scripts archive extraction;
- project-local graphics/audio inventory;
- RTP references as external dependencies, not bundled assets.

Do not hard-code English-only paths or assume default `Scripts.rxdata` if `Game.ini` specifies another file.

## 10. Intermediate representation

The RPG2X360 IR is the long-term contract between source import and runtime.

Required root objects:

```text
Rpg2XProject
├── Metadata
├── SourceFingerprint
├── CompatibilitySummary
├── System
├── Actors / Classes / Skills / Items / Weapons / Armors
├── Enemies / Troops / States / Animations
├── Tilesets
├── Maps
│   └── Events -> Pages -> Commands
├── CommonEvents
├── Switches / Variables / SelfSwitch metadata
├── Scripts
│   ├── inventory
│   ├── compatibility tier
│   ├── translated modules
│   └── manual-port references
├── Assets
├── RuntimeShimBindings
└── Diagnostics
```

Event commands should compile to an engine-neutral opcode model rather than carrying RMXP implementation details into every runtime subsystem.

The IR format is versioned independently from application version.

## 11. Event runtime first-support profile

The first playable profile should prioritize common non-scripted RPG behavior:
- Show Text
- Show Choices
- Conditional Branch
- Control Switches
- Control Variables
- Control Self Switch
- Wait
- Transfer Player
- Set Move Route / Move Event
- Change Gold
- Change Items / Weapons / Armor
- Change Party Member
- Recover / change basic actor parameters when mapped
- Play/stop BGM, BGS, ME, SE where supported
- Change screen tone / simple fades where supported
- Call Common Event
- Label / Jump to Label
- Exit Event Processing
- script event commands: route to RGSS analyzer and compatibility status, never execute on the host

Additional command families are added only with tests and runtime support.

## 12. Asset pipeline

The converter must inventory rather than blindly copy everything.

For each asset:
- normalized logical path;
- source location;
- content hash;
- category;
- dimensions/format for images when readable;
- codec/metadata for audio when readable;
- referenced/unreferenced state when determinable;
- dependency origin: project-local or RTP;
- conversion requirement;
- runtime destination.

RTP assets are referenced as dependencies. The project package must not redistribute RTP content unless the user separately supplies content under terms that permit it.

Asset transforms must be cacheable and deterministic.

## 13. WPF application workflow

### Welcome
- New RMXP conversion
- Recent workspaces
- Open existing RPG2X workspace
- Documentation/settings

### Source Project
- Folder picker
- detection result
- Game.ini summary
- project fingerprint
- source read-only badge
- dependency/RTP summary

### Import Dashboard
Cards:
- Maps
- Events
- Database entries
- Scripts
- Images
- Audio
- RTP dependencies
- Unknown RXDATA objects

### RGSS Analyzer
- script load-order tree
- class/module/method browser
- shim matches
- dynamic-risk indicators
- monkey-patch indicators
- filter by Tier A-E
- source viewer
- generated manual-port tasks

### Compatibility
Search/filter by status, severity, category, map, event, script, shim and blocking state.

### Conversion
Step progress:
- snapshot/fingerprint
- RXDATA decode
- DB/map/event import
- RGSS extraction
- static analysis
- IR generation
- shim binding
- asset processing
- export validation

### Runtime Export
- choose Unity 5.4.1f target workspace/package
- validate IR version compatibility
- generate runtime manifest
- show missing runtime features before export

### Results
- readiness
- output package
- compatibility report
- manual-port workspace
- diagnostics
- hashes

## 14. Milestone roadmap

### M00 — Repository Foundation & Safety Contract
**Goal:** Create the buildable host solution, WPF shell and permanent safety boundaries.

Deliver:
- solution/project scaffold;
- DI/configuration/logging;
- workspace abstraction;
- read-only `ISourceProject` boundary;
- process-execution prohibition in importer layer;
- safe temp/output service;
- test fixture framework;
- WPF navigation shell and theme foundation;
- CLI shell;
- CI-friendly test command;
- AGENTS/docs integrated into repository.

Acceptance:
- solution builds;
- tests prove source inputs are not written;
- importer services expose no API that launches `Game.exe`;
- UI opens and can create/select a workspace without conversion logic.

### M01 — RMXP Detection, Game.ini & Source Snapshot
**Goal:** Reliably identify RMXP projects and fingerprint source state.

Deliver:
- detect `Game.rxproj`, `Game.ini`, Data directory and scripts path;
- parse Game.ini without loading RGSS;
- inventory project-local folders;
- fingerprint important files;
- dependency/RTP metadata inventory;
- validation diagnostics for malformed/missing structures.

Acceptance:
- synthetic valid/invalid layouts classified correctly;
- customized script path from Game.ini honored;
- no source modifications.

### M02 — Ruby Marshal / RXDATA Decoder Foundation
**Goal:** Decode RMXP serialized data safely without Ruby execution.

Deliver:
- bounded Marshal reader abstraction;
- object graph model preserving class name and ivars;
- codecs for core primitive/container types;
- explicit handlers for RMXP-specific binary structures such as tables/colors/tones when required;
- corruption/depth/size limits;
- golden synthetic fixtures and malformed-input tests.

Acceptance:
- deterministic round-trip-neutral reads of supported synthetic fixtures;
- malformed input fails safely with diagnostics;
- no dynamic Ruby runtime required.

### M03 — Database Import
**Goal:** Convert core RMXP database objects into source models.

Deliver:
- actors/classes/skills/items/weapons/armors;
- enemies/troops/states/animations;
- system settings;
- tilesets;
- common events metadata;
- switches/variables names;
- unknown-field preservation.

Acceptance:
- representative synthetic database fixture imports with stable IDs;
- unsupported fields appear in diagnostics rather than disappearing.

### M04 — Maps, Tables, Events & Event Commands
**Goal:** Reconstruct RMXP maps and event structure.

Deliver:
- MapInfos/import hierarchy;
- map dimensions and tile table;
- events/pages/conditions/graphics;
- move routes;
- event command list with code/parameters/source coordinates;
- common events;
- command inventory statistics.

Acceptance:
- fixtures reproduce map/event counts and command sequences exactly;
- script-command events are preserved and flagged, never executed.

### M05 — Scripts.rxdata Extraction & RGSS Inventory
**Goal:** Safely expose RGSS source and load order.

Deliver:
- locate script archive from Game.ini;
- decode script entry array;
- zlib-decompress source bytes;
- preserve original bytes/encoding diagnostics;
- script IDs/names/order;
- export optional `.rb` analysis copies into RPG2X workspace;
- source hash per script.

Acceptance:
- extracted source matches fixture content;
- malformed zlib/encoding is isolated per script;
- no Ruby execution.

### M06 — RGSS Static Analyzer & Compatibility Tiers
**Goal:** Turn script source into actionable compatibility evidence.

Deliver:
- safe lexer/parser adapter;
- symbols/classes/modules/methods;
- known RGSS API call inventory;
- monkey-patch/class reopen detection;
- dynamic-risk detectors;
- native/Win32 call detection;
- Tier A-E assignment rules;
- source-located diagnostics;
- shim registry interface;
- HTML/JSON/text report export.

Acceptance:
- curated script fixtures receive deterministic tiers;
- critical dynamic/native features are never auto-approved;
- analyzer continues after one bad script.

### M07 — RPG2X360 IR v1 & Event Compiler
**Goal:** Establish the stable source-to-runtime contract.

Deliver:
- IR models/schema/versioning;
- source fingerprints and provenance;
- event opcode compiler for first-support profile;
- unknown command preservation;
- compatibility references attached to IR nodes;
- deterministic JSON export;
- schema validation.

Acceptance:
- same source/settings yields semantically identical IR;
- every event command is converted or represented as explicit unsupported/manual work.

### M08 — RGSS Auto-Subset, Shim Bindings & Manual Port Stubs
**Goal:** Bridge common RGSS customizations without pretending to support all Ruby.

Deliver:
- bounded expression/control-flow translation subset;
- known API shim binding records;
- generated C# manual-port skeletons for Tier D;
- mapping manifest from script/method to runtime implementation;
- developer annotations that survive regeneration;
- conflict detection when multiple scripts patch the same behavior.

Acceptance:
- only allowlisted semantics auto-translate;
- regeneration does not silently overwrite user manual-port code;
- unresolved critical overrides block Ready status.

### M09 — Asset & RTP Dependency Pipeline
**Goal:** Build reproducible runtime assets.

Deliver:
- graphics/audio/font inventory;
- hashes and dependency graph;
- project-local copy/export;
- RTP external dependency report;
- deterministic transcode queue abstraction;
- image/audio conversion hooks;
- cache and invalidation.

Acceptance:
- no proprietary RTP copied from tool install automatically;
- missing dependencies block or warn according to use;
- repeated unchanged conversions reuse cache correctly.

### M10 — Converter UX, CLI & Preview Release
**Goal:** Make the host converter usable end-to-end.

Deliver:
- completed WPF screens;
- recent projects/workspaces;
- compatibility filters/search;
- conversion wizard;
- cancel/resume-safe stages where possible;
- CLI commands: detect, import, analyze, convert, validate;
- diagnostic package generator;
- preview installer/package plan.

Acceptance:
- a representative fixture can go from Select Project to validated IR package entirely through UI and CLI;
- UI and CLI produce equivalent results.

### M11 — Unity 5.4.1f Runtime Bootstrap
**Goal:** Create the legacy-compatible runtime host.

Deliver:
- Unity 5.4.1f project structure;
- IR loader/validator compatible with generated schema subset;
- boot scene;
- service registry compatible with old Unity/C# constraints;
- content root resolver;
- logical 640x480 presentation layer;
- PC editor simulation mode.

Acceptance:
- exported package loads in Unity 5.4.1f without using modern host assemblies;
- unsupported IR version fails clearly.

### M12 — Map Rendering, Tiles, Camera & Player Movement
**Goal:** Render RMXP maps and move a player through them.

Deliver:
- tile table decoder at runtime;
- tileset/autotile plan and supported baseline;
- passability/priority behavior for supported cases;
- camera scrolling;
- player sprite/directional animation;
- transfer spawn positions;
- collision tests.

Acceptance:
- reference maps render and navigate consistently with fixture expectations.

### M13 — Event Interpreter Vertical Slice
**Goal:** Execute the first-support event opcode profile.

Deliver:
- interpreter state machine;
- switches/variables/self switches;
- page condition resolution;
- text/choice UI;
- branches/labels/waits;
- movement routes;
- transfers;
- common events;
- basic inventory/party operations;
- audio/fade hooks.

Acceptance:
- vertical-slice fixture completes a scripted quest sequence with deterministic state.

### M14 — Core UI, Audio, Save/Load & Scene Services
**Goal:** Make the runtime feel like a usable RPG rather than a map viewer.

Deliver:
- message/window framework;
- title/menu baseline where supported;
- audio service;
- save serialization using RPG2X runtime state, not Ruby Marshal;
- slots and version migration policy;
- runtime settings;
- crash-safe save write pattern.

Acceptance:
- save/load restores map, player, switches, variables, inventory and party state for supported profile.

### M15 — Stock Battle-System Baseline
**Goal:** Implement an initial battle path sufficient for basic RMXP projects.

Deliver:
- battle scene/state machine;
- actors/enemies/troops;
- skills/items/states;
- turn/action processing;
- damage formulas mapped for supported stock semantics;
- animations/audio hooks;
- victory/defeat/escape results;
- compatibility flags for custom battle scripts.

Acceptance:
- synthetic stock-battle fixture reaches correct outcomes;
- custom battle-engine overrides are not misrepresented as stock-compatible.

### M16 — RGSS Runtime Shim Catalog v1
**Goal:** Implement the most valuable RGSS abstractions needed by real projects.

Deliver:
- prioritized shim implementations based on analyzed corpus;
- mapping tests from analyzer rule -> shim ID -> runtime behavior;
- Graphics/Input/Audio/resource/window/sprite-style abstractions where feasible;
- explicit unsupported methods/properties;
- performance instrumentation.

Acceptance:
- every advertised shim has both detection and runtime tests;
- missing shim implementations fail visibly.

### M17 — Xbox 360 Input, Storage & Performance Integration
**Goal:** Make the Unity runtime console-ready.

Deliver:
- Xbox controller mapping;
- controller-to-RMXP input action map;
- Xbox-safe persistent save path abstraction;
- memory/GC/per-frame allocation profiling;
- load-time profiling;
- 720p output/presentation configuration while preserving logical gameplay coordinates;
- console diagnostics overlay/build logging as feasible.

Acceptance:
- PC simulation and console mappings share the same logical input actions;
- runtime remains within measured memory/performance budgets established on hardware.

### M18 — First Xbox 360 XEX Smoke Test
**Goal:** Prove end-to-end build and boot on authorized hardware.

Deliver:
- build configuration documentation without bundling XDK assets;
- minimal converted fixture package;
- XEX boot test;
- controller movement/event test;
- save/load smoke test;
- hardware evidence report.

Acceptance:
- converted vertical slice boots and reaches expected interaction on Xbox 360.

### M19 — Complete Basic RMXP Game Vertical Slice
**Goal:** Port a small but complete representative game using supported stock features.

Deliver:
- title -> exploration -> dialogue -> inventory -> battle -> map transfer -> save/load -> ending;
- compatibility report with zero hidden unsupported items;
- performance report;
- conversion/runtime defect backlog.

Acceptance:
- game is completable on console within the declared compatibility profile.

### M20 — Compatibility Hardening & Real-World Corpus
**Goal:** Expand from synthetic fixtures to user-authorized projects and common RGSS patterns.

Deliver:
- anonymized/redistributable regression fixtures where licensing permits;
- shim priority based on frequency;
- analyzer false-positive/false-negative reduction;
- converter recovery from malformed projects;
- compatibility database/versioning.

Acceptance:
- published compatibility claims map to evidence and test fixtures.

### M21 — v2.0 Release Candidate
**Goal:** Package the converter/runtime toolchain for repeatable development use.

Deliver:
- installer/portable host package;
- Unity runtime template/exporter;
- CLI documentation;
- compatibility guide;
- manual RGSS porting guide;
- troubleshooting/diagnostics;
- license/third-party notices;
- regression matrix;
- release checklist.

Acceptance:
- clean-machine host install succeeds;
- fixture conversion and Unity import pass;
- no proprietary RPG Maker/XDK content is bundled;
- known limitations are explicit.

## 15. Deferred work

After RMXP is proven:
- RPG Maker VX / VX Ace importers and RGSS2/RGSS3 adapters;
- RPG Maker MV / MZ importers;
- richer battle systems;
- wider RGSS subset coverage;
- optional AI-assisted explanation/manual-port suggestions, always requiring developer review;
- alternate runtimes if justified.

## 16. Explicit non-goals for v2.0

- binary EXE-to-XEX conversion;
- DRM/security bypass;
- automatic conversion of arbitrary native Windows DLL integrations;
- automatic execution of unknown Ruby for “analysis”;
- 100% compatibility marketing claim;
- bundling RPG Maker RTP/RGSS runtime or Xbox SDK files;
- full Ruby VM on Xbox as a prerequisite for the first playable release.

## 17. Release definition

RPG2X360 v2.0 is successful when the toolchain can truthfully say:

> “For RPG Maker XP projects within the documented supported profile, RPG2X360 can statically analyze the project, explain compatibility, generate a validated RPG2X runtime package, and run a representative complete game on an Xbox 360 through the Unity 5.4.1f runtime.”

Anything outside that profile remains visible as compatibility work, not silently ignored.
