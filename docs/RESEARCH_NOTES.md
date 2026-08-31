# Research Notes Used for v2.0 Architecture

These are implementation-oriented facts Codex should validate against fixtures and current documentation rather than treating as magic constants.

- RPG Maker XP projects conventionally use `Game.rxproj`, `Game.ini`, a `Data` directory and `.rxdata` files.
- RMXP data is serialized using Ruby Marshal conventions associated with RGSS1/Ruby 1.8-era runtime behavior.
- `Game.ini` identifies the RGSS library, scripts archive and RTP dependencies; the converter must parse this configuration but never load the RGSS library.
- `Scripts.rxdata` is a serialized array whose script records contain an ID/name and zlib-compressed source payload. Script order matters.
- RMXP's stock display space is 640x480, so the first compatibility runtime should preserve that logical coordinate system rather than stretching assumptions into 16:9.
- Modern .NET host tooling and the legacy Unity 5.4.1f runtime should communicate through serialized IR, not shared modern binaries.

Suggested external references for engineering research:
- RPG Maker XP / RGSS reference documentation for `Game.ini` and RGSS1 behavior.
- Ruby Marshal format documentation / test vectors.
- Independent open-source RMXP tooling can be used as corroborating research after license review, but source code should not be copied blindly.

All third-party dependencies selected by Codex require a license/security review and should be recorded in third-party notices.
