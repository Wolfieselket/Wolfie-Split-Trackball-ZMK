# Wolfie right half — Bluetooth-lag diagnostic without nice!view

## IMPORTANT

Extract this ZIP into your **Wolfie-Split-Trackball-ZMK** repository root,
**NOT** wolfie-niceview. Merge/replace the files. It changes two files only:

- `build.yaml` (adds a no-display right-central build, preserves all prior builds)
- `config/boards/shields/wolfie_split/wolfie_split_right.conf`
  (moves display thread priority 10 out of shared .conf and into the normal
  display-enabled build's `cmake-args`; keeps 12 ms PMW3610 report interval)

Your custom nice!view module, display assets, keymap and other firmware files
are not modified. The standard `wolfie_split_right_central` build still has
priority 10, and the no-display diagnostic won't apply this display-only setting.

## Installation using GitHub Desktop

1. Open **Wolfie-Split-Trackball-ZMK**, branch `main` (or the active branch).
2. Extract the ZIP. Copy `build.yaml` and the `config` folder into the root
   of the repository, merging folders and replacing only the two files above.
3. Verify GitHub Desktop shows only these two modified files.
4. Commit as `Add right-half no-display Bluetooth latency diagnostic` and Push.
5. Wait for GitHub Actions to complete successfully.
6. Download the NEW artifact `wolfie_split_right_no_display` and flash ONLY
   `wolfie_split_right_no_display.uf2` to your RIGHT nice!nano. Do not flash
   left and do not use any `settings_reset` firmware.
7. Unplug USB; reconnect over Bluetooth. Test slow circular trackball motion
   and typing using the right-hand keys. Also try turning the left half back on.
   The right nice!view may retain a static image (memory LCD) but must not
   animate or refresh during the test.
8. Compare with your normal `wolfie_split_right_central.uf2` firmware.
   To restore the full animated right display, flash that normal UF2 back
   onto the right half, again without resetting Bluetooth settings.

## Diagnostic interpretation

- Smooth Bluetooth input without the display => display/UI activity is implicated.
- Jerky input even without the display => focus on BLE HID connection timing,
  radio coexistence or sensor/report handling rather than animation.

## Notes

- The test firmware is **NOT** precompiled. GitHub Actions will compile it.
- Bluetooth pairing data should remain intact (no settings reset), although
  reconnection behaviour depends on your existing firmware and host.
- CPI remains 800 in the pre-existing overlay; the 12 ms sensor interval stays.
- This preserves the normal working build entry and does not alter display
  source code, left screen, battery, keymap, or PMW3610 pin assignments.
- If any other `build.yaml` edits happened since this package was prepared,
  review differences before replacing it to avoid overwriting newer entries.
