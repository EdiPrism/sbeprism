# Decoding `Nasdaq TotalView-ITCH 5.0` & `MoldUDP64` Packets Offline

## 1. `MoldUDP64` Multicast Framing (`20`-Byte Header)

Market-data `.pcap` captures from Nasdaq multicast feeds encapsulate one or more `TotalView-ITCH 5.0` messages inside a **Big-Endian `MoldUDP64` UDP payload**:

- **`B0..B9` (`10` Bytes ASCII):** Session identifier.
- **`B10..B17` (`uint64 BE`):** Sequence number of the first message in the packet.
- **`B18..B19` (`uint16 BE`):** Message count (`0` = heartbeat, `0xFFFF` = end-of-session).
- **`B20+` Repeating Blocks:** `[messageLength: uint16 BE][ITCH message bytes]`.

## 2. `48`-Bit Midnight Nanosecond Timestamp (`uint48 BE`)

Every `ITCH 5.0` message stores `timestampNs` at root bytes `4..9` (wire bytes `B5..B10` including the 1-byte `msgType` tag) as a **6-byte unsigned Big-Endian integer** representing nanoseconds elapsed since `00:00:00.000000000` Eastern Time:

```c
uint64_t ns = ((uint64_t)p[5] << 40) | ((uint64_t)p[6] << 32) |
              ((uint64_t)p[7] << 24) | ((uint64_t)p[8] << 16) |
              ((uint64_t)p[9] << 8)  |  (uint64_t)p[10];
// Example: 34,200,500,000,000 ns -> 09:30:00.500000000 (Market Open + 500ms)
```

## 3. Fixed-Point `Price4` (`* 10^-4`) vs. `Price8` (`* 10^-8`)

Standard `ITCH 5.0` equity order messages (`AddOrder 'A'` `36B`, `AddOrderMPID 'F'` `40B`, `OrderReplace 'U'` `35B`) encode prices as 4-byte unsigned Big-Endian integers with `4` implied decimal places (`1852500 -> $185.2500`), whereas `OUCH 5.0` order-entry frames use 8-byte fixed-point integers scaled by `10^-8` (or `10^-4` depending on venue port configuration).
