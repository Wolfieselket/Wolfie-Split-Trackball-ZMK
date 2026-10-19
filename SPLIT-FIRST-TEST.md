# Wolfie: first wireless split firmware test

## Purpose
- Right nice!nano: **central** Bluetooth keyboard with the proven PMW3610 SPI0 sensor and wolfie-niceview SPI1 animated eye.
- Left nice!nano: **peripheral** sends all 22 left key positions over BLE to the right.
- 44-key shared `config/wolfie_split.keymap`: left default matches Corne + None/Win/None/Space thumbs; right default matches proven QWERTY/AltGr; right thumb 2 holds Function with right H/J/K/L arrows.
- Left display is wired but disabled until we validate split operation.
- Existing standalone targets and `settings_reset` remain in `build.yaml` as recovery builds.

## Install
1. Make sure `working-complete-right-half` and your left standalone firmware remain saved.
2. In GitHub Desktop, use `main` branch. Copy this ZIP's **contents** into the local Wolfie-Split-Trackball-ZMK repository, merging `config`, replacing only `build.yaml`.
3. Commit and push, then inspect the GitHub Actions result. **Do not flash if either split build fails.**
4. Download the `firmware` artifact and identify `wolfie_split_right_central.uf2` and `wolfie_split_left_peripheral.uf2`.

## Before flashing
- The left half will no longer act as a standalone keyboard. It won't appear as a pairable keyboard in Windows.
- Flashing `settings_reset` on both controllers is recommended when changing from standalone to split roles, to clear old host and split bonds. This will require Windows to forget and re-pair the right central keyboard. Follow the ZMK split reset procedure.
- If you flash via USB, unplug **battery** from that half first, then reconnect it for wireless testing afterward.
- Use right USB only initially; avoid sending unintended shortcuts to the Windows machine while testing.

## Typical transition order after BOTH split builds succeed
1. Remove old host Bluetooth bond in Windows.
2. Flash `wolfie_settings_reset.uf2` to BOTH nice!nanos (one after another), then flash the proper new right/left split UF2 to each.
3. Power both halves, press reset on both nearly simultaneously to initiate pairing if required.
4. The right central should advertise to Windows, pair it. Left peripheral itself should never pair directly with Windows.
5. Test left letters and thumbs, right keys and PMW3610, eye and battery reading. Keep the known-good right UF2 available for recovery.

## Caveats
This source package is **not compiled locally**. GitHub Actions is the compilation validation. It reuses the proven GPIO maps, but has not yet been tested on both halves together. ZMK version and external shield modules may have different requirements. If you see a failure, send the **first actual error** from the split build's log and do not flash.
