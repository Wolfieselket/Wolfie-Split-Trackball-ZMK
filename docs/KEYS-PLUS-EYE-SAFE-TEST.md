# Stage 2: Keyboard + upright eye diagnostic

This is a **patch-only ZIP** for `Wolfieselket/Wolfie-Split-Trackball-ZMK`.
It adds a new **separate build** that combines the real, previously tested 4 x 6
keyboard matrix with your existing *working upright static-eye* display.
It **does not** include the PMW3610 trackball controller or input listener.
It does **not** modify the display module (`wolfie-niceview`).

## What to copy

In GitHub Desktop, select `Wolfie-Split-Trackball-ZMK` on `main`, then
`Repository > Show in Explorer`. Extract this ZIP somewhere else, and copy
its **contents** into the repository root. The `build.yaml` file is the **only
replacement**, and the `config/boards/shields/wolfie_keys_eye_test/` folder
and `config/wolfie_keys_eye_test.keymap` are new.

Do not delete the existing `config` contents or the hidden `.git` directory.
Commit `Add keyboard and upright-eye safe diagnostic` and push `main`.

## Artifacts

GitHub Actions should produce three artifacts/configurations:

- `wolfie_right_keys_trackball`: known-working keys+PMW fallback (no screen)
- `wolfie_display_only_diagnostic`: existing known-working upright eye (no real keys)
- **`wolfie_keys_eye_safe_test`**: real keyboard matrix + upright eye (no sensor)

The static-eye renderer is taken from the **current `wolfie-niceview` main
branch** at build time. Keep its working `working-static-eye` branch unchanged.

## Safe hardware test

1. Verify the GitHub Actions build succeeded. Download **only** the artifact
   named `wolfie_keys_eye_safe_test` and identify its `.uf2`.
2. Use a noncritical computer session. Close important documents, and have the
   USB cable/reset ready. If unwanted keypresses appear, disconnect immediately.
3. Flash the new test image. It uses **no Alt, Ctrl, Win, Enter, Backspace,
   layer-switch, or other shortcut functions**. Its 22 real switches type
   alphabet letters A-X (two unused matrix positions); this is **temporary**.
4. Without touching the switches, watch for unsolicited text in a blank editor
   for at least 30-60 seconds. Confirm the upright eye stays stable.
5. Press every switch and compare outputs:

   Physical top (left-to-right):        X W V U T S
   Physical second row:                R Q P O N M
   Physical third row:                 L K J I H G
   Physical thumbs (left-to-right):    F E D C

6. If all keys work and there are no phantom inputs, tell ChatGPT and we can
   restore the full QWERTY keymap (still without the trackball) in the next test.

**If the screen is black or unwanted typing returns, revert immediately to the
working backup firmware. Don't alter wiring, display rotation or SPI.**

This ZIP was checked for its file structure and GPIO isolation, but **was not
compiled locally**; GitHub Actions is the compile test.
