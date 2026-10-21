# Wolfie — 60-second idle / 20-minute deep sleep

Adds TWO NEW, ISOLATED build targets to Wolfie-Split-Trackball-ZMK/build.yaml. The previous 30-second/60-minute test entries and all normal builds remain unchanged.

## Values in BOTH new targets
- `CONFIG_ZMK_IDLE_TIMEOUT=60000` — idle after 60,000 milliseconds (1 minute).
- `CONFIG_ZMK_SLEEP=y` — enable deep sleep.
- `CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=1200000` — deep sleep after 1,200,000 milliseconds (20 minutes) of inactivity.

## Resulting UF2 filenames
- RIGHT: `wolfie_split_right_sleep_20m.uf2` — optimized BLE + PMW3610 + animated eye, battery and profile CPI switching (BT1 400 CPI, other profiles 1000 CPI).
- LEFT: `wolfie_split_left_sleep_20m.uf2` — split peripheral, left space display, battery and LINK status.

## Install in GitHub Desktop
1. Open **Wolfie-Split-Trackball-ZMK** (not wolfie-niceview) and select the development branch used for your successful 60-minute builds. Ideally make a backup branch from the working commit.
2. Extract this ZIP. Copy `build.yaml` into the repository root and examine the GitHub Desktop diff. The ONLY intended modification from the previous 60-minute-test version is TWO ADDITIONAL build entries at the end.
3. If your existing `build.yaml` differs, **do not replace other changes**. Instead open `ADDED-BUILD-ENTRIES.txt` and paste its two `- board` blocks at the bottom of the existing `include:` list, preserving indentation (two spaces before each `- board`).
4. Commit `Add 60s idle 20m sleep tests` and push.
5. Run GitHub Actions and verify a successful build. Download the NEW artifact and flash the RIGHT 20m UF2 to the right controller, then the LEFT 20m UF2 to the left controller.
6. **Never flash settings_reset for this test**; Bluetooth pairing should stay as-is.

## Test
1. Confirm both halves, nice!views, eye, left LINK, keymap and trackball still work on battery power.
2. Leave both halves completely untouched for about 70 seconds; confirm keys and trackball respond promptly (normal idle).
3. Leave both halves untouched and unplugged for **25 minutes measured from the last keypress**; wait for BT disconnection. A nice!view can retain its image while sleeping.
4. Press a key on the right to wake and reconnect to the host, then press a key on the left to wake the peripheral; test typing, cursor, CPI profile switching and both displays.
5. If a half fails to wake, try its reset/power switch and restore your existing working firmware if needed.

## Important
- You have **not yet confirmed Stage 3 wake-up** on the 60-minute firmware. A shorter timeout is a preference, not proof that deep-sleep wake works. Perform a full wake test before relying on it overnight.
- A previously scheduled **65-minute reminder** corresponds to the earlier test and may not match the new firmware. Start a new 25-minute countdown after flashing and completing the idle test.
- Neither the LCD's custom animation code nor the PMW3610/BLE timing code is changed; the new builds pass sleep settings in CMake arguments only.
- Files were checked as YAML and source package; **not compiled or tested on hardware**.
