# QWERTY + static eye integration test

Use this update **on top of** the existing, successful `Wolfie-keys-plus-upright-eye-safe-test` configuration. Extract the ZIP and copy its contents into the **main** branch checkout of `Wolfie-Split-Trackball-ZMK`, preserving folder hierarchy. Do not delete the existing `config` folder or `working-keyboard-trackball` branch. Review and commit using GitHub Desktop, then Push origin.

This ZIP modifies `build.yaml` and **adds** a QWERTY test shield and keymap. The existing keyboard+trackball fallback, display-only diagnostic and letter-only keyboard+eye test remain available and are not changed.

## Firmware selection

The target artifact for this stage is `wolfie_keys_eye_qwerty`. Build with the shield combination `wolfie_keys_eye_qwerty wolfie_display_bus wolfie_niceview`.

- Matrix: **same** 4x6 scan GPIO configuration and COL2ROW diode direction as the successful letter-only test.
- Keymap: copied without modification from the original working `right_half_test.keymap`; thumb keys are Enter, momentary layer (`&mo 1`), Right Alt/AltGr, and None, in physical left-to-right order.
- nice!view: uses **the currently working** upright static-eye module from `wolfie-niceview` (no asset or display-bus change).
- PMW3610: **disabled in this test**, no PMW3610 initialization or input listener.

## Important safety check

The earlier combined firmware generated unintended shortcuts. For the first test, disable the keyboard's Bluetooth connection to your normal Windows PC, connect to a safe test machine/session if available, and be prepared to unplug immediately. Watch for spontaneous inputs before pressing any keys. Only then verify key mapping, Enter, AltGr+U, and the layer key. If phantom input occurs, unplug and return to the known-working firmware.

This patch was checked for path consistency and pin-map preservation, but **not locally compiled**. GitHub Actions is the compilation check. Do not flash until the new target builds successfully.
