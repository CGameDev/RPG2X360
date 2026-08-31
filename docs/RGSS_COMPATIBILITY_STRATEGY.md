# RGSS1 Compatibility Strategy

## Guiding decision

RPG2X360 v2.0 does **not** equate “Ruby parsed successfully” with “script can run on Xbox.” The goal is behavior preservation with evidence.

## Pipeline

```text
Scripts.rxdata
 -> decode record array
 -> zlib decompress each source
 -> normalize only for analysis (preserve raw)
 -> tokenize/parse safely
 -> symbol/API inventory
 -> patch/dynamic-risk analysis
 -> shim matching
 -> compatibility tier
 -> optional bounded translation or manual-port stub
```

## Tier A — Native IR

Use when behavior belongs in data/events rather than custom script execution. Example: event commands already represented by the event compiler.

## Tier B — Auto-translatable subset

A deliberately small Ruby subset may be translated only when semantics are defined and tested. Candidate constructs:
- literals;
- local variables;
- basic arithmetic/boolean expressions;
- arrays/hashes within supported forms;
- simple conditionals;
- bounded loops where supported;
- method calls to allowlisted APIs;
- simple method definitions where types/targets are understood.

Disallow/route to higher tiers when semantics are uncertain.

## Tier C — Runtime shim

Calls to known RGSS/RPG APIs can map to runtime services. A shim record includes:
- stable ID;
- source pattern/API signature;
- supported argument semantics;
- runtime implementation type/method;
- limitations;
- tests;
- minimum runtime version.

## Tier D — Manual port required

Generate a C# scaffold in a protected developer workspace. Include:
- original script name and hash;
- source line/method if known;
- dependency list;
- referenced RGSS APIs;
- reason auto-translation stopped;
- suggested runtime service;
- TODOs;
- generated-region boundary so regeneration does not overwrite hand-written code.

## Tier E — Unsupported/blocking

Examples that default to Tier E until specifically supported:
- arbitrary `eval`/runtime code generation;
- native/Win32 API or custom DLL dependency;
- unknown binary extensions;
- scripts that require Windows-only UI/network/file assumptions with no shim;
- deep replacement of engine internals for which runtime equivalence is not implemented.

## Monkey patch and load-order awareness

RGSS commonly permits reopening classes/modules. The analyzer must record:
- class/module definitions in source order;
- methods introduced/overridden;
- aliases when parseable;
- known stock-class overrides;
- collisions among custom scripts.

A script that overrides a stock method is not compatible merely because the method body uses simple Ruby.

## Script-event commands

RMXP event commands that execute script snippets are sent through the same static analyzer. The Windows converter never runs them.

## Compatibility evidence

Every script result includes:
- script ID/name/hash;
- line/span when possible;
- tier/status;
- severity;
- matched rules;
- shim IDs;
- dependencies;
- manual-port task ID;
- blocking reason.

## Future option

A Ruby interpreter or broader transpiler may be researched later, but it is not a dependency for v2.0 and must not weaken the static safety boundary.
