# Stage 5 — QWERTY + eye + PMW3610

This update only adds a **new shield and keymap** plus an extra entry in `build.yaml`.
It does not change earlier shields, the display module, or any of the working test builds.

## Build

`wolfie_keys_eye_pmw_qwerty`

The shield is an exact copy of the **working Stage 4** `wolfie_keys_eye_pmw_safe` hardware overlay and `.conf`, with a new shield name. This keeps:

- Keyboard matrix on the same 10 GPIO pins, with `col2row` diodes.
- PMW3610 on SPI0, 600 CPI, and the existing axis options.
- Upright static nice!view eye from the separate `wolfie_display_bus` and `wolfie_niceview` shields.

The keymap is an exact copy of the **working Stage 3** `wolfie_keys_eye_qwerty.keymap`, retaining:

- Y U I O P Backspace
- H J K L ; '
- N M , . / Escape
- Enter / Layer 2 / AltGr / None

## Update instructions

In GitHub Desktop, select `main` in `Wolfie-Split-Trackball-ZMK`; keep `working-keyboard-trackball` unchanged. Extract this update ZIP; copy the **contents** of the extracted folder into the local repo root, keeping paths intact. Check that only the new shield, new keymap, `build.yaml`, and this document appear in Changes. Commit and push.

After a successful GitHub Actions run, extract the `firmware` artifact and select the UF2 for **`wolfie_keys_eye_pmw_qwerty`**. Do not flash a different target.

Because an earlier experiment caused phantom keypresses, start with a safe test session. If unsolicited input reappears, disconnect at once and flash a known-working Stage 4 or `right_half_test` image. Keep backup branches intact.

This patch was checked for file consistency but has **not** been compiled locally.
