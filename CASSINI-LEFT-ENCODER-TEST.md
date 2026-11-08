# Cassini ZMK — optional left EC11 rotary scrolling test

**Development-only**. This branch is separate from the working
`cassini-right-orientation` build. Do not flash the encoder-test firmware until
the EC11 connections are confirmed.

## Intended wiring

The firmware currently PROPOSES these unused left nice!nano v2 pins:

| EC11 function | nice!nano GPIO | Pro Micro pad |
| --- | --- | --- |
| A | P0.02 | F6 |
| B | P1.15 | F7 |
| Common/C | GND | GND |
| Push switch (2 pins) | Not connected | Not used |

The two GPIOs are not occupied by the left key matrix or nice!view
in the existing firmware. **These are proposed pins, not verified actual
Saturn-XR-02 wiring.** If A or B was wired elsewhere, change
`config/boards/shields/wolfie_split/wolfie_split.dtsi` to match before
building or flashing.

## Feature

- Left EC11 peripheral sends sensor turns to right central over split BLE.
- Clockwise: HID vertical wheel down; counter-clockwise: up.
- Push-button: unassigned.
- Defaults: 80 encoder steps / 20 scroll triggers per revolution,
  120 ms mouse-scroll behavior hold, no acceleration specific to the encoder.
- The right-half PMW3610 sensor and its Y inversion are untouched.

## Builds

Use the GitHub Actions **Run workflow** option and select
`cassini-left-encoder-dev`. Download and flash:

1. **Right:** `wolfie_split_right_stable_candidate`. The right central owns the
   keymap and encoder scroll behavior.
2. **Left:** `cassini_split_left_encoder_test`. The left BLE peripheral
   enables the EC11 on its proposed GPIOs.

The standard `wolfie_split_left_stable_candidate` target remains present
and does not enable the encoder. Keep a previous known-good UF2 available.

## Verify

1. Verify both halves LINK and type normally.
2. With the keyboard face up, left thumbs from left to right:
   Alt, Windows, Space, momentary Symbols/Numbers/Arrows (Layer 1).
3. Verify right PMW3610 X and Y directions and Functions thumb are unchanged.
4. In a scrollable page, rotate the EC11 one detent clockwise — down;
   one detent counterclockwise — up.
5. Check for missed or duplicated detents; tune `steps`,
   `triggers-per-rotation` or `tap-ms` only after hardware tests.
6. Verify idle, wake, and connection stability.

If scrolling is reversed, exchange A/B pin designations in the overlay;
avoid changing the matrix, display, or trackball configuration.
