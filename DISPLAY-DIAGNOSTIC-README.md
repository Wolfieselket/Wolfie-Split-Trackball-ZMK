# Display-only test: nice!view + custom eye

This package is based on the previous display-integration source, but **the experimental full-combination build is deliberately removed from `build.yaml`**. The working `right_half_test` target remains as a fallback.

## Safety
- **The diagnostic has no physical keyboard matrix and no PMW3610 listener**. A `zmk,kscan-mock` node emits no switch events, and its only key binding is `&none`. It cannot emit ordinary key presses from the real keyboard wiring.
- It may still advertise itself to Bluetooth hosts because it is a ZMK build. Remove or disconnect the old pairing first if you want to prevent connection surprises.
- Use only the `wolfie_display_only_diagnostic` artifact for this test, **not** the old combined `_eye` build.
- The old proven firmware remains on `working-keyboard-trackball`; it is also built as `wolfie_right_keys_trackball`.

## Installation with GitHub Desktop
1. Select your `main` branch in `Wolfie-Split-Trackball-ZMK`.
2. Unzip and copy the **contents** over the repo root, preserving `.github/` and `config/` structure. Do not replace `.git/`.
3. Inspect changed files, commit, push. The extra old experimental integration shield may remain as an unused file; this ZIP does **not** include a build for it.
4. Open GitHub Actions and check that the diagnostic build succeeded.
5. Download its artifact, flash to nice!nano using reset bootloader mode, and inspect screen. This diagnostic does not type or move the cursor.
6. If the static eye and `WOLFIE R` appear, we have verified screen wiring, SPI1, display driver and basic rendering. If only text appears, inspect image assets. If blank, inspect driver, controller routing, power and the *actual* remote module revision used by west.
7. After the test, flash `wolfie_right_keys_trackball` to restore typing and pointing; screen will be blank.

## Hardware signals
nice!view SPI1: P0.20 SCK, P0.17 MOSI, P0.06 CS. Sensor SPI0 not enabled in diagnostic. Display power VCC and GND must remain connected.

## Notes
The module `wolfie-niceview` is still fetched via `config/west.yml` at revision `main`. The remote repo should contain the previously restored static-eye renderer; verify that it does. The custom renderer from that remote repo is not bundled into this ZIP.

**Not locally compiled.** GitHub Actions is the build validation step.
