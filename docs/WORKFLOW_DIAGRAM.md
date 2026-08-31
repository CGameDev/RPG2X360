# RPG2X360 RMXP-First Workflow Diagram

## Mermaid

```mermaid
flowchart TD
    A[Select RPG Maker XP Project] --> B[Read-Only Validation & Fingerprint]
    B --> C[Parse Game.ini / Dependencies]
    C --> D[Decode Data/*.rxdata via Static Marshal Reader]
    D --> E[Import Database Maps Events Assets]
    C --> F[Locate Scripts.rxdata]
    F --> G[Extract + Zlib Decompress RGSS Source]
    G --> H[Static RGSS Parser / Symbol & API Analysis]
    E --> I[Compatibility Engine]
    H --> I
    I --> J{Compatibility Tier}
    J -->|A| K[Native RPG2X IR]
    J -->|B| L[Auto-Translate Bounded RGSS Subset]
    J -->|C| M[Bind Runtime Shim]
    J -->|D| N[Generate Manual C# Port Stub]
    J -->|E| O[Unsupported / Blocking Diagnostic]
    K --> P[Versioned RPG2X360 IR Package]
    L --> P
    M --> P
    N --> P
    O --> P
    E --> Q[Asset + RTP Dependency Pipeline]
    Q --> P
    P --> R[Validate Package / Compatibility Report]
    R --> S[Unity 5.4.1f RPG2X Runtime]
    S --> T[PC Simulation / Runtime Tests]
    T --> U[Xbox 360 Build via Authorized Local Toolchain]
    U --> V[RGH/JTAG Xbox 360 Hardware Validation]
```

## ASCII

```text
                       RPG MAKER XP PROJECT
                               |
                  +------------+-------------+
                  |                          |
          Game.ini / Data                Scripts.rxdata
                  |                          |
       Static Marshal Decode         Decompress Ruby Source
                  |                          |
      DB / Maps / Events / Assets     Static RGSS Analysis
                  |                          |
                  +------------+-------------+
                               |
                    COMPATIBILITY ENGINE
                               |
         +----------+----------+----------+----------+
         |          |          |          |          |
       Tier A     Tier B     Tier C     Tier D     Tier E
     Native IR   Auto RGSS   Runtime    Manual    Unsupported
                 Subset      Shim       C# Stub   / Blocking
         |          |          |          |          |
         +----------+----------+----------+----------+
                               |
                    RPG2X360 IR PACKAGE
                               |
                    Unity 5.4.1f Runtime
                               |
                       Runtime Testing
                               |
                       Xbox 360 Build
                               |
                      Hardware Validation
```
