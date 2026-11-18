# Changelog — SbePrism SBE ITCH Viewer

## [1.0.0] — 2026-11-18
- **Initial Release (`PSBEPRISM`):**
  - Zero-hijack `<sbe:messageSchema>` XML & `.sbe` wire byte-offset pills, alignment gap / overlap badges, and hover cards showing C++20 / Rust types, standard SBE null sentinels, `mantissa * 10^exponent` decimals, enums, and `<set>` bitmasks.
  - Interactive 2D `64`-bit (`8`-byte CPU word) wire layout & bitset matrix with two-way editor caret sync.
  - Built-in clean-room `Nasdaq TotalView-ITCH 5.0` and `OUCH 5.0` wire catalogs.
  - `[Pro]` Pure-JVM microsecond & hardware-nanosecond `.pcap` / `.pcapng`, `CME MDP 3.0`, `SOFH`, `MoldUDP64`, and `ITCH 5.0` binary wire decoder + searchable (`Ctrl+F`) binary hex editor.
  - `[Pro]` SBE wire-ordering (`Root -> Groups -> VarData`), `blockLength` truncation, alignment gap, and cross-version schema evolution validator + native `Ctrl+D` semantic diff (`Alt+Shift+B`).
  - `[Pro]` 1-click zero-copy `C++20 (.hpp)` & `#[no_std] Rust (.rs)` codegen, unit-test byte-vector generator, and deterministic `HmacSHA256` `ClOrdID`/account/schema alpha scrubber (`Alt+Shift+R`).
  - Air-gap-safe `?` ToolWindow feedback menu and trading-desk-redacted `IDE Fatal Errors` crash handler.
