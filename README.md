# Wolfie Split Trackball ZMK

Right-half integration baseline: 22 working keys and PMW3610 sensor, with no display enabled. Built from the previously tested right-half firmware and current AltGr keymap.

## Build

GitHub Actions builds `wolfie_right_keys_trackball` for `nice_nano/nrf52840/zmk` with the `right_half_test` shield.

## Default physical layout (right half, left to right)

```
Y  U  I  O  P  Backspace
H  J  K  L  ;  '
N  M  ,  .  /  Escape
Enter  Layer 2 (hold)  AltGr  Disabled
```

The internal matrix indices are intentionally reversed from the physical layout based on the verified A–X switch test. The layer hold binding is `&mo 1` (layer index 1 is the second layer).

## Hardware

- Right-hand nice!nano; six columns, four rows; 1N4148 diodes oriented cathode toward row, col2row
- PMW3610 pins: SDIO P0.08, SCK P0.22, CS P0.02, MOT P1.15; 600 CPI with current axes preserved
- The nice!view is **deliberately disabled** in this baseline; it will be integrated after the keyboard + trackball build is validated.

## Important

This ZIP was assembled from the user's previously tested repository, but has not been compiled. The GitHub Actions result is the first check. Keep the old repository as rollback reference.
