# Unity 5.4.1f / Xbox 360 Runtime Specification

## Runtime role

The Unity project is a compatibility runtime for RPG2X360 IR, not an RPG Maker emulator and not a Ruby host.

## Required systems

1. IR/package loader
2. asset resolver/cache
3. logical display/presentation layer
4. map/tile renderer
5. player/camera/collision
6. event interpreter
7. switches/variables/self-switches
8. text/choice/window UI
9. audio service
10. inventory/party/database services
11. save/load
12. battle baseline
13. RGSS shim registry
14. input abstraction
15. diagnostics/performance counters

## Logical display

Preserve the RMXP default 640x480 logical space initially. Xbox output can be 1280x720 while the compatibility layer controls scaling/pillarboxing. Widescreen expansion must be opt-in and compatibility-tested; it cannot simply alter coordinates assumed by source scripts.

## Input

Define logical actions rather than hard-coding controller buttons throughout gameplay code:

```text
Confirm
Cancel
Menu
Up/Down/Left/Right
PageLeft/PageRight
Debug/Developer actions (development builds only)
```

Map PC keyboard and Xbox controller to the same action layer.

## Save data

Do not reproduce Ruby Marshal save files on Xbox unless a later compatibility milestone explicitly requires import/export. Runtime saves use a versioned RPG2X state format and include migration metadata.

## Performance

Avoid per-frame LINQ/reflection-heavy paths, uncontrolled allocations and repeated asset loads. Profile before accepting performance claims. Provide low-cost counters in development builds.

## Build boundary

The repository may contain build scripts/config templates, but not proprietary XDK binaries/keys. Local configuration locates the user's authorized tools.
