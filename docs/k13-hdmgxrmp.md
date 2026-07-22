# K13 (`hdmgxrmp`) experimental support

Product category: `jtmspro`  
Product ID: `hdmgxrmp`  
Marketing name observed: **K13** / Big Kids Room lock

## devices.json example

Create `/config/tuya_local_ble/devices.json`:

```json
{
  "DC:23:53:0C:4A:7A": {
    "address": "DC:23:53:0C:4A:7A",
    "uuid": "4f1b76766a8498d8",
    "local_key": "<local_key>",
    "device_id": "ebc3e5nip6bjkjre",
    "category": "jtmspro",
    "product_id": "hdmgxrmp",
    "device_name": "Big Kids Room Lock",
    "product_model": "K13",
    "product_name": "K13"
  }
}
```

Replace `<local_key>` with the key from TinyTuya / Tuya IoT.

## Mapped DPs (from Tuya cloud diagnostics)

| DP | Code | Role in this fork |
|----|------|-------------------|
| 8 | residual_electricity | Battery % sensor |
| 9 | battery_state | Battery enum sensor |
| 10 | child_lock | Config switch |
| 11 | anti_lock_outside | Config switch |
| 31 | beep_volume | Select (mute/normal) |
| 32 | reverse_lock | Config switch |
| 46 | manual_lock | Lock command |
| 47 | lock_motor_state | Lock state |
| 62 / 71 | unlock + ble_unlock_check | Remote unlock (challenge) |

## Auto-lock

This product's cloud DP schema has **no** `automatic_lock` / delay datapoint. That is why Smart Life shows auto-lock but will not let you change it — the firmware does not expose a setting. Home Assistant cannot add a real control for a DP that does not exist.

## Expectations

- Requires BLE range (Pi adapter or Bluetooth proxy near the lock).
- After re-pairing in Smart Life, refresh `local_key` and `ble_unlock_check` in `devices.json` (they rotate).
- Remote unlock needs a current `ble_unlock_check` value from Tuya cloud status.
