# Wolfie three-component safety test

This ZIP is an *additive patch* for `Wolfie-Split-Trackball-ZMK` on `main`.
It adds the `wolfie_keys_eye_pmw_safe` build alongside all previous targets.

## Included components
- Real 4×6 matrix with the tested GPIO assignments and `col2row` diodes.
- Letter-only test keymap (A–X matrix, 22 physically populated positions).
- PMW3610 on SPI0 using its known-working pins and 600 CPI settings.
- nice!view on the already-working dedicated SPI1 bus with the upright
  `wolfie_niceview` static-eye renderer. No new display module changes.

## Upload
Copy the **contents** of this ZIP into the root of the local
`Wolfie-Split-Trackball-ZMK` repository on `main`, replacing `build.yaml`.
Commit, push and let GitHub Actions compile.
Download the resulting `firmware` artifact and flash **only** the UF2 for
`wolfie_keys_eye_pmw_safe`.

## Safe hardware check
1. Keep the known-working `.uf2` ready for immediate recovery.
2. Disconnect unused Bluetooth hosts, and close sensitive applications.
3. Observe for 30–60 seconds without touching any keys or the ball.
4. Check no unsolicited letters or mouse movement occurs.
5. Check the static eye remains upright and stable.
6. Check trackball movement and then type a few letters.
7. Disconnect immediately if unwanted input occurs.

This build is intentionally **not** QWERTY. The keymap contains no Alt,
Ctrl, GUI, or function shortcuts. If it passes, a later patch can restore
QWERTY/AltGr to the combined build.

Build and hardware operation are **not yet verified**. The backed-up
`working-keyboard-trackball` and `working-static-eye` branches are unchanged.
