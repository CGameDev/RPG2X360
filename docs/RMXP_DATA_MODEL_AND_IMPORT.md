# RMXP Data Model & Import Requirements

## Detection

Primary signals:
- `Game.rxproj`
- `Game.ini`
- `Data/`
- RMXP `.rxdata` database/maps

Detection must combine evidence rather than trusting one filename.

## Game.ini

Parse as configuration data only. Important values include:
- RGSS library filename;
- scripts archive path;
- title;
- RTP dependency names.

Do not load the RGSS library.

## RXDATA

RMXP `.rxdata` contains Ruby Marshal-serialized data. The importer needs a non-executing decoder that preserves object class names and instance variables. Treat decoded data as data, not trusted objects.

### Safety limits
- maximum file size configurable;
- object/reference count limits;
- recursion/depth limit;
- string/binary allocation caps;
- map/table dimension sanity checks;
- cancellation support;
- clear malformed-object diagnostics.

## Script archive

`Scripts.rxdata` is logically an array of script records. Each record contains an identifier, a display name and compressed source bytes. Source bytes are decompressed for static analysis. Preserve order because RGSS script behavior can depend on load order.

Do not assume UTF-8. Detect/record encoding issues and retain raw hashes/bytes in the workspace when permitted.

## Unknown data

When a Marshal object or instance variable is not yet modeled:
- preserve class name;
- preserve readable primitive values;
- preserve raw/opaque data when safe;
- emit a diagnostic;
- never silently skip the object.

## RTP dependencies

RTP references are dependencies. RPG2X360 should tell the user which resources are required and which project assets reference them. The converter does not automatically redistribute RTP content.

## Import result

The RMXP importer produces a host-side `RmxpSourceProject`, not runtime-ready data. It includes provenance/source coordinates so every runtime/compatibility item can link back to its source file, map/event or script.
