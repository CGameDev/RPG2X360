# WPF UI / UX Specification

## Design goal

The interface should make compatibility understandable. The user should be able to answer: “What will work, what will be translated, what needs a runtime shim, and what must I port manually?” without reading logs.

## Navigation

```text
Home
Source Project
Import Summary
Project Explorer
RGSS Analyzer
Compatibility
Conversion
Runtime Export
Results
Settings
```

## Source Project screen

Show:
- project path;
- detected engine;
- `Game.ini` title/library/scripts entry;
- project fingerprint;
- source read-only indicator;
- source size/file counts;
- RTP dependencies;
- validation problems.

The read-only badge remains visible through all conversion screens.

## Project Explorer

Tree:

```text
Database
Maps
  Map -> Events -> Pages -> Commands
Common Events
Scripts
Graphics
Audio
Dependencies
Unknown Data
```

Selecting an item shows source provenance and compatibility status.

## RGSS Analyzer

Main panes:
- script load order;
- source viewer;
- symbols/API usage;
- compatibility findings;
- shim matches;
- manual-port task preview.

Filters:
- Tier A-E;
- severity;
- overridden stock class/method;
- native/dynamic-risk usage;
- runtime shim.

## Compatibility dashboard

Do not use a single misleading percentage as the only indicator. Show counts plus the headline state.

Cards:
- Supported
- Auto-translated
- Runtime shims
- Manual port
- Unsupported
- Blocking

## Conversion progress

Each stage is independently visible. Cancellation should leave the last valid workspace state and never corrupt source data.

## Runtime export

Show:
- IR format version;
- target runtime version;
- unresolved runtime requirements;
- missing assets/dependencies;
- manual-port files not implemented;
- final readiness gate.

## Accessibility

- keyboard navigation;
- high DPI;
- screen-reader labels for status icons;
- do not encode compatibility solely by color;
- reduced-motion option for long-running progress effects.
