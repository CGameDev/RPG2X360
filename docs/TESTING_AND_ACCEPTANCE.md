# Testing & Acceptance Strategy

## Test layers

### Unit
Marshal primitives, Game.ini, model mapping, event compiler, RGSS rules, shims, hashing and versioning.

### Golden synthetic fixtures
Hand-authored non-proprietary RMXP-like serialized fixtures with known expected objects/events/scripts.

### Malformed input
- truncated Marshal streams;
- invalid references;
- excessive nesting;
- huge declared sizes;
- invalid zlib script payload;
- broken encodings;
- missing MapInfos/maps;
- missing assets;
- inconsistent map/event IDs.

### Security/safety
- verify no source writes;
- verify converter never launches executables;
- verify analyzer never evaluates script source;
- path traversal and output-directory escape tests;
- zip/package extraction safety if archive packaging is introduced.

### Determinism
Run identical source/settings twice and compare normalized IR/hashes excluding intentionally variable metadata.

### Runtime
- Unity editor tests where possible;
- deterministic event-interpreter fixtures;
- save/load round trip;
- asset resolution;
- shim behavior tests.

### Hardware
Xbox 360 validation is a separate evidence layer. Never label a feature hardware-verified merely because it works in Windows/Unity editor.

## Fixture licensing

Do not commit commercial/community RPG Maker games or RTP. Use synthetic fixtures. User-authorized local projects can be tested from ignored paths. Convert observations into small synthetic reproductions when possible.

## Compatibility claim policy

A capability may be marked `Supported` only when:
1. importer behavior is tested;
2. IR output is tested;
3. runtime behavior is tested if runtime-relevant;
4. limitations are documented.

A shim is not supported until analyzer detection and runtime implementation have matching tests.

## Release gates

No release candidate with:
- hidden dropped source data;
- Critical/Blocking items mislabeled as Ready;
- source mutation bug;
- arbitrary script execution path;
- proprietary bundled content;
- unknown IR/runtime major-version mismatch.
