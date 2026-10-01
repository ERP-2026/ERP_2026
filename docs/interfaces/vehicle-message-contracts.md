# Vehicle message contracts

## Purpose

`src/erp2026_msgs` contains the three ROS1 messages observed on the ERP_2024 real-vehicle command and feedback path. It preserves their field names, primitive types, and order. It is a minimum contract extraction from legacy `Total_msgs`, not a migration of that whole package or an implemented ERP_2026 vehicle interface. The package defines messages only; it has no publisher, subscriber, transport, or vehicle runtime.

Legacy source of truth: `ERP_2024_ANALYSIS/src/Total_msgs/ERP/{ModeCmd,DriveCmd,SerialFeedBack}.msg`. Behavior below comes from `ERP2024/src/Control/Control.cpp` (`PubSerial`) and `erp42_serial/src/erp42_serial.cpp` (`ModeCallback`, `DriveCallback`, `writeUpdate`, `readUpdate`). These paths refer to the separate, read-only legacy repository.

## Legacy command flow

```text
ERP2024 Control / PubSerial
  → ModeCmd + DriveCmd
  → erp42_serial callbacks and TX
  → ERP42

ERP42
  → erp42_serial RX / readUpdate
  → SerialFeedBack
  → ERP2024
```

This is the observed ERP_2024 flow, not an ERP_2026 runtime or a promise of identical vehicle behavior.

## Verification status

| Status | Meaning |
| --- | --- |
| `VERIFIED_FROM_CODE` | Directly present in the legacy message definition or active source code. |
| `INFERRED_FROM_CODE` | Interpretation of code behavior, without independent hardware confirmation. |
| `TBD_OFFICIAL_SPEC` | Vehicle specification or hardware validation still required. |

All ROS field types and ordering below are `VERIFIED_FROM_CODE`. Descriptions of official physical units, ranges, direction, and protocol semantics remain `TBD_OFFICIAL_SPEC` unless explicitly stated otherwise.

## ModeCmd

| Field | ROS type | Legacy usage | Status |
| --- | --- | --- | --- |
| `MorA` | `uint8` | `PubSerial` sets `1`; `ModeCallback` copies it to TX state. The code treats feedback value `1` as auto mode. | `VERIFIED_FROM_CODE`; full mode semantics `TBD_OFFICIAL_SPEC` |
| `EStop` | `uint8` | `PubSerial` copies `u8_EStop`; callback copies it to TX state. | `VERIFIED_FROM_CODE`; safety semantics `TBD_OFFICIAL_SPEC` |
| `Gear` | `uint8` | `PubSerial` assigns `0` or `2` for two observed planning signals; callback copies the field to TX state. This does not establish a complete Gear enum. | `VERIFIED_FROM_CODE`; official mapping `TBD_OFFICIAL_SPEC` |
| `alive` | `uint8` | `PubSerial` sets `1`, but the active `ModeCallback` does not read it. | `VERIFIED_FROM_CODE`; intended semantics `TBD_OFFICIAL_SPEC` |

## DriveCmd

| Field | ROS type | Legacy generation and use | Status |
| --- | --- | --- | --- |
| `KPH` | `uint16` | `PubSerial` computes `(uint16_t)st_Control.f32_ThrottleInput * 10`; the cast occurs **before** multiplication, so the fractional part is discarded first. `DriveCallback` copies this integer without applying its commented-out scale factor. The name does not establish the official physical unit. | `VERIFIED_FROM_CODE`; official unit/range `TBD_OFFICIAL_SPEC` |
| `Deg` | `int16` | `PubSerial` transforms target steering radians → degrees (`Rad2Deg`) → × `74.074` → clamp to `[-2000, 2000]` → sign inversion → `int16`. `DriveCallback` copies this scaled integer to TX state. It is not a plain degree value. | `VERIFIED_FROM_CODE`; official scale/direction `TBD_OFFICIAL_SPEC` |
| `brake` | `uint8` | `PubSerial` clamps its target to `[1, 200]` and casts it. Active `DriveCallback` instead sets the TX brake state to `0`, and `writeUpdate` fixes the TX brake byte to `0`. | `VERIFIED_FROM_CODE`; official brake semantics `TBD_OFFICIAL_SPEC` |

## SerialFeedBack

`readUpdate` reads a received legacy serial frame and publishes these fields. Integer arithmetic is used for the speed and steer divisions before assignment to `float64` message fields; the fractional remainder is therefore lost.

| Field | ROS type | Legacy parser behavior | Status |
| --- | --- | --- | --- |
| `MorA` | `uint8` | Copies received byte 3. | `VERIFIED_FROM_CODE`; official meanings `TBD_OFFICIAL_SPEC` |
| `EStop` | `uint8` | Copies received byte 4. | `VERIFIED_FROM_CODE`; official meanings `TBD_OFFICIAL_SPEC` |
| `Gear` | `uint8` | Copies received byte 5. | `VERIFIED_FROM_CODE`; official mapping `TBD_OFFICIAL_SPEC` |
| `speed` | `float64` | Combines received bytes 6–7 into an integer, then divides by `10` using integer arithmetic. | `VERIFIED_FROM_CODE`; official unit/scale `TBD_OFFICIAL_SPEC` |
| `steer` | `float64` | Combines received bytes 8–9, applies the legacy signed-value adjustment, then divides by `71` using integer arithmetic. | `VERIFIED_FROM_CODE`; official unit/direction `TBD_OFFICIAL_SPEC` |
| `brake` | `int16` | Copies received byte 10. | `VERIFIED_FROM_CODE`; official unit/meaning `TBD_OFFICIAL_SPEC` |
| `encoder` | `int32` | Combines received bytes 11–14 into a raw 32-bit value. | `VERIFIED_FROM_CODE`; official unit/direction `TBD_OFFICIAL_SPEC` |
| `alive` | `uint8` | Copies received byte 15. | `VERIFIED_FROM_CODE`; official sequence semantics `TBD_OFFICIAL_SPEC` |

The byte positions and arithmetic describe legacy code only. They are not a validated hardware protocol specification.

## Known legacy issues

- `DriveCmd.brake` is generated by `PubSerial` but discarded by the active `DriveCallback`; the TX brake byte is fixed to `0`. This is an observed legacy implementation defect/behavior, **not** a requirement for ERP_2026 to ignore brakes.
- `ModeCmd.alive` is ignored by the callback. TX uses a separate `m_AlvCnt++` counter; its declaration and constructor do not initialize it.
- Serial runtime behavior and safety require separate investigation; this package neither carries over nor fixes that runtime.

## Out of scope

Serial transport, TX/RX packet codec, CAN, `Parking.cpp` migration, LiDAR, Camera, Localization, vehicle actuation, and hardware validation are outside this message-only change. No official Gear enum, command ranges, physical units, or brake/alive semantics are declared here.
