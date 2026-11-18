# FIX SBE Wire Byte-Offset Calculation, Alignment Padding Gaps & Safe `sinceVersion` Evolution

## 1. Standard 8-Byte `messageHeader` + Root `blockLength` Framing

In **FIX Simple Binary Encoding (`SBE`)** (`littleEndian` by default on x86_64 exchanges such as CME `MDP 3.0` / `iLink 3` and `Binance Spot SBE`), every standalone message begins with an 8-byte `messageHeader` composite followed immediately by the fixed-width root block:

| Wire Bytes | Field | Type | Purpose |
| :--- | :--- | :--- | :--- |
| `B0..B1` | `blockLength` | `uint16` | Acting byte length of the **root fixed block** on the wire (excludes header, repeating groups, and varData) |
| `B2..B3` | `templateId` | `uint16` | Identifies `<sbe:message id="...">` |
| `B4..B5` | `schemaId` | `uint16` | Identifies `<sbe:messageSchema id="...">` |
| `B6..B7` | `version` | `uint16` | Acting schema version (`actingVersion`) used by the encoder |

## 2. Explicit `offset="..."` Alignment Gaps vs. Silent Stack Leaks

- When `<field>` omits `offset`, its root offset equals the previous wire field's `offset + byteLength`.
- Fields with `presence="constant"` (such as the `exponent` inside a fixed-point `Price9` composite) occupy **`0` bytes on the wire**.
- When an explicit `offset` jumps ahead of the running cursor (e.g., aligning a `uint64` field to an 8-byte boundary), the skipped bytes form an **unmapped wire padding hole**—encoders must zero-initialize the buffer so stack/heap memory never leaks onto the wire.

## 3. Strict Wire Section Ordering & Backward-Compatible Evolution

1. **Mandatory Section Order:** `messageHeader` -> `Root Fixed Fields` -> `Repeating <group>s` -> `Variable-Length <data>`. Declaring `<data>` before `<group>` is illegal because variable-length tails have no fixed offset stride.
2. **Non-Breaking Schema Evolution:** Append new root fields strictly at `offset >= oldBlockLength` with `sinceVersion="N"` (`N > oldVersion`) and increase `blockLength`. Older messages on disk or `.pcap` captures carry the smaller `actingBlockLength` in `B0..B1`, allowing newer decoders to skip extension fields cleanly without corrupting repeating group cursors.
