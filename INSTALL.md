# Wolfie — Sleep / wake-up diagnostic (30-second idle, 60-minute deep sleep)

This is an **uncompiled source-only test**. It adds precisely TWO firmware build targets to the previously proven full-display/smooth-BLE `build.yaml`. No source files, graphics, keymaps, sensor overlay, Bluetooth pairings or existing builds are changed.

## Settings, applied to BOTH new targets only
- CONFIG_ZMK_IDLE_TIMEOUT=30000: regular idle after ~30 seconds of inactivity.
- CONFIG_ZMK_SLEEP=y: enable deep sleep if supported by the board.
- CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=3600000: deep sleep after ~60 minutes of inactivity.

## Added build artifacts
- RIGHT nice!nano (central): `wolfie_split_right_sleep_60m.uf2`
  - Same `wolfie_split_right wolfie_display_bus wolfie_niceview` combination as the known-good smooth firmware.
  - Retains preferred BT 7.5 ms, sensor report min 7 ms, BLE priority 4, display priority 10.
  - Uses the current `wolfie-niceview` module for eye, status and BT1 CPI 400/others 1000. That module must remain on your known-good revision.
- LEFT nice!nano (peripheral): `wolfie_split_left_sleep_60m.uf2`
  - Same `wolfie_split_left wolfie_display_bus wolfie_leftview` combination as the functioning left space scene.

## Install with GitHub Desktop
1. Create a backup branch of **Wolfie-Split-Trackball-ZMK** at your known-good smooth-Bluetooth commit (e.g. `working-profile-cpi-optimized`). Do not edit it.
2. Return to your normal working development branch (usually `main`) in GitHub Desktop. Check you are in **Wolfie-Split-Trackball-ZMK**, NOT **wolfie-niceview**.
3. Extract this ZIP. Copy its **build.yaml** into the repository root and inspect the GitHub Desktop diff BEFORE committing.
   - Intended changes: exactly TWO NEW blocks near the end (one right sleep target, one left sleep target).
   - If you have newer/different build.yaml entries already, DO NOT replace them: copy ONLY those two new blocks from the included `ADDED-BUILD-ENTRIES.txt` into your existing build.yaml, under the `include:` list.
4. Commit `Add isolated 30s idle 60m sleep tests for both halves` and Push.
5. Build in GitHub Actions; do not flash a failed or old artifact.
6. Download the NEW zip with UF2 firmware artifacts. Flash `wolfie_split_right_sleep_60m.uf2` to the RIGHT half and `wolfie_split_left_sleep_60m.uf2` to the LEFT half, one at a time, checking operation in between.
7. Do NOT flash `settings_reset`; keep your BLE bonds intact.

## Hardware test procedure
1. Check both halves type, trackball moves smoothly, both displays and LINK status work, and BT0 vs BT1 (1000 vs 400 CPI) still switches.
2. Disconnect USB and let both halves sit for ~35 seconds without touching anything. Test the next key and ball movement. Normal idle should be responsive, while the screens may hold an old image due to the memory LCD.
3. Leave BOTH halves entirely untouched and on battery for at least 65 minutes. Avoid moving the trackball or touching keys. If Bluetooth disconnects after an hour, that's consistent with deep sleep.
4. Press a RIGHT-side key to wake the central, wait for host BLE to reconnect, then press a LEFT-side key to wake the peripheral and check typing from both halves. Test PMW3610, correct BT1 400-CPI mapping, battery updates and both screens.
5. If a half cannot wake, try its reset button/power-cycle. Restore known-good firmware rather than repeatedly changing parameters. If both halves use on/off switches, charging while testing can change the power-state behavior; test on battery.

## Restore (no bond reset)
- RIGHT: `wolfie_split_right_bt75_sensor7_bleprio4_display.uf2` from the previously successful build.
- LEFT: `wolfie_split_left_display_test.uf2` from its previous successful build.
Keep those firmware files handy BEFORE starting the hour-long test.

## Caution / limitations
- Firmware was NOT compiled here. Zephyr/ZMK configuration support is determined in GitHub Actions.
- A wakeup-source property on keyboard kscan is needed; Wolfie's shared matrix was previously reported to include `wakeup-source;`. On actual hardware, wake from a physical key is safer to test than relying on trackball movement.
- Deep sleep intentionally disconnects BLE and can take several seconds to wake and reconnect. It is not expected to retain live display animation; the nice!view may retain a static image.
- Firmware and hardware versions can cause deep-sleep wake problems, especially on custom split centrals. Do not leave it unattended overnight until a complete wake-and-reconnect test has passed.
- Display and PMW3610 low-power behavior are not guaranteed solely by these options; battery drain requires actual testing after wake reliability is confirmed.
