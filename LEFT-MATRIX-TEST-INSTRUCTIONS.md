# Wolfie left-half matrix test

## What this changes
Adds a single seventh build target (`wolfie_left_matrix_test`) to `build.yaml`; does NOT alter the six right/display test targets.

It adds a new `config/boards/shields/wolfie_left_matrix_test/` shield and `config/wolfie_left_matrix_test.keymap`.

## Confirmed GPIO wiring
Rows: black P0.29, red P1.13, blue P1.11, green P0.09.
Columns: green P0.24, yellow P1.00, white P0.11, blue P1.04, red P1.06, black P0.31.
Diodes: black cathode bands toward rows (`col2row`).

The left screen can remain physically wired at CS P0.06, SCK P0.20, MOSI P0.17, but it will be blank in this diagnostic. No battery required.

## Upload via GitHub Desktop
1. Use `main` in `Wolfie-Split-Trackball-ZMK`, leaving backup branches untouched.
2. Extract the ZIP and merge the contents into the local repository root (never overwrite `.git`).
3. Inspect changes (one modified `build.yaml`, six? new files in left shield+keymap+readme).
4. Commit `Add left-only matrix test` and Push origin.
5. After GitHub Actions succeeds, download `firmware` and flash ONLY `wolfie_left_matrix_test` UF2 to the LEFT nice!nano.

## Test
- Disconnect the left battery (if fitted); connect via USB.
- On computer, ideally disable Bluetooth pairing temporarily so it is obvious which board is testing.
- Open Notepad and first wait 30 seconds with no keys pressed. If spontaneous typing occurs, unplug and check wiring.
- Press each physical row from left to right; note six key outputs per main row and four thumb outputs. Actual order may be reversed by the mirrored harness.
- Every switch should produce one letter. Record 22 outputs in physical order so we can build the actual left keymap.
- Use double-press of reset to enter UF2 bootloader if needed.

## Notes
This is a standalone left USB diagnostic, NOT split peripheral firmware and NOT a final QWERTY left layer. It has not yet been compiled; successful CI and hardware test required.
