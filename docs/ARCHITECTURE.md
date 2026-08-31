# Architecture

## Layered design

```text
WPF / CLI
   |
RPG2X360.Core orchestration
   |
+----------------------+---------------------+
| RMXP Source Import   | RGSS Static Analyze |
+----------------------+---------------------+
            |
     Source Model
            |
 Compatibility + Conversion
            |
      RPG2X360 IR v1
            |
+----------------------+---------------------+
| Reports/Diagnostics  | Runtime Export      |
+----------------------+---------------------+
                                  |
                         Unity 5.4.1f Runtime
                                  |
                            Xbox 360 Build
```

## Host-side project responsibilities

### RPG2X360.Desktop
Pure presentation, navigation, ViewModels and user interaction. No parsing logic.

### RPG2X360.Core
Workspaces, pipeline orchestration, cancellation, configuration, progress and service composition.

### RPG2X360.Models
Shared host-side source, compatibility, IR and diagnostic models. Runtime serialization contracts should be isolated/versioned.

### RPG2X360.Rmxp
Project detection, Game.ini, Marshal/RXDATA decoding, database/maps/events/scripts archive reading.

### RPG2X360.Rgss
Script archive model, static parser adapter, RGSS symbol/API metadata and source-analysis primitives.

### RPG2X360.Analysis
Compatibility rules, RGSS tier engine, project readiness, dependency analysis and shim matching.

### RPG2X360.Conversion
Source model -> RPG2X360 IR, event compiler, RGSS bounded-subset translation and manual-port workspace generation.

### RPG2X360.Export
IR package serialization, schema validation, assets, reports and Unity target package.

### RPG2X360.Diagnostics
Structured logs, stable diagnostic IDs, redaction rules and support bundles.

### RPG2X360.Cli
Automation surface; must call the same application services as WPF.

## Runtime boundary

The Unity 5.4.1f runtime consumes serialized IR and assets. It must not reference modern .NET host projects. Create explicit DTOs/readers compatible with Unity 5.4.1f.

## Workspaces

A conversion uses:

```text
<workspace>/
  source.snapshot.json
  cache/
  analysis/
  manual-port/
  output/
  reports/
  logs/
```

Only the workspace/output is mutable by RPG2X360. The source path remains read-only.

## Stable identifiers

Diagnostics, shims, event opcodes and compatibility rules use stable string IDs, e.g.:

```text
RMXP001_MISSING_GAME_INI
RXD014_MAX_DEPTH_EXCEEDED
RGSS042_DYNAMIC_EVAL
SHIM.Graphics.Transition.v1
EVT.ShowText.v1
```

This allows compatibility reports to remain understandable across versions.
