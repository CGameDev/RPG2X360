# RPG2X360 Intermediate Representation (IR) v1

## Purpose

The IR decouples RMXP decoding from the Unity 5.4.1f runtime. It is engine-neutral enough to support future source importers without forcing source-engine quirks into runtime code.

## Package layout

```text
MyGame.rpg2x/
  manifest.json
  system.json
  database/
  maps/
  events/
  scripts/
  shims/
  assets/
  manual-port/
  compatibility/
  hashes.json
```

A single archive container can be added later; use an inspectable directory package first.

## Manifest minimum

```json
{
  "formatVersion": "1.0",
  "generator": "RPG2X360",
  "sourceEngine": "RPG Maker XP",
  "projectName": "Example",
  "sourceFingerprint": "sha256:...",
  "compatibility": {
    "headline": "ManualRgssPortRequired",
    "blockingCount": 0,
    "manualPortCount": 3
  },
  "runtimeMinimum": "0.1.0"
}
```

## Event opcodes

Use stable explicit opcodes and typed arguments. Do not make Unity interpret raw RMXP event parameter arrays directly.

Example:

```json
{
  "opcode": "EVT.ShowText.v1",
  "args": {
    "text": "Hello world",
    "speaker": null
  },
  "source": {
    "mapId": 4,
    "eventId": 7,
    "page": 1,
    "commandIndex": 12
  }
}
```

Unknown commands remain represented as explicit unresolved nodes with original code/parameters and compatibility diagnostics.

## Serialization constraints

Because Unity 5.4.1f is a legacy target:
- keep DTO shapes simple;
- avoid requiring modern .NET serializers on the runtime;
- use deterministic property names/versioning;
- consider generated runtime-specific DTOs/readers;
- validate host output before Unity import.

## Versioning

Breaking changes increment format major version. Runtime refuses unknown major versions. Minor additions must be forward-tolerant where possible.
