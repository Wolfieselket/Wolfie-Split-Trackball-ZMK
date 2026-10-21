# Wolfie — Animated eye + optimised Bluetooth test

This ZIP adds **one** new right-central firmware target to the existing `build.yaml`.
It is based on the working `Wolfie-BLE-Notify-Priority-4-Test.zip` build file.
It does not change existing targets or touch nice!view source files, the keymap,
physical sensor overlay, left half, battery screens or pairing settings.

## Install through GitHub Desktop

1. Open **Wolfie-Split-Trackball-ZMK**, NOT **wolfie-niceview**.
2. Extract this ZIP; copy **build.yaml** to the root of the repository.
3. In GitHub Desktop, inspect the diff BEFORE committing. The only intended
   change is the new block `RIGHT full-display performance test` and its
   `wolfie_split_right_bt75_sensor7_bleprio4_display` artifact.
   If your build.yaml has other new edits, add ONLY that one new block manually
   rather than overwriting your newer entries.
4. Commit: `Test animated eye with optimized Bluetooth timing` and push.
5. Run a new GitHub Actions firmware build and wait for green SUCCESS.
6. Download the NEW artifact and flash only the RIGHT nice!nano with:
   **wolfie_split_right_bt75_sensor7_bleprio4_display.uf2**
7. Keep the left keyboard firmware unchanged. Do NOT use settings_reset.

## Exactly what this target enables

- Right shield: `wolfie_split_right wolfie_display_bus wolfie_niceview`.
  This loads the existing working animated eye, combined BT/layer status,
  battery percentage and USB indicator from the wolfie-niceview module.
- Bluetooth notification-worker priority: `CONFIG_ZMK_BLE_THREAD_PRIORITY=4`.
- Preferred BLE host connection interval: min/max=6 (7.5 ms requested).
  Host may negotiate other parameters.
- BLE automatic parameter updates: `CONFIG_BT_GAP_AUTO_UPDATE_CONN_PARAMS=y`.
- PMW3610 minimum report interval: 7 ms.
- Display work priority: 10 (the same priority as the original normal build).
- Trackball CPI: NOT changed by this ZIP. Your overlay presently uses 1200 CPI.
- Normal PMW3610 sampling: NO 4 ms force-awake shield in this target.

## How to compare

1. Confirm eye animates, battery and `WOLFIE R` show correctly, and BT / layer
   status work as they did before.
2. Test continuous Bluetooth cursor movement for 20–30 seconds, then capture
   MouseTester > Interval vs Time and describe whether the cursor remains smooth.
3. If enabling the display makes jitter return, flash the known-good no-display
   target `wolfie_split_right_bt75_sensor7_bleprio4_no_display.uf2` from a
   successful build. That isolates display workload from the BLE settings.
4. If the new full-display firmware performs well, we can consider promoting
   these settings to the normal build in a separate, later change.

## Limitations

The ZIP and YAML are structurally validated here, but ZMK compilation and
behaviour on your hardware have not been verified. The target is a diagnostic;
its speed, animation consistency and power draw require on-device testing.
