# Display integration test

Keep your `working-keyboard-trackball` branch unchanged. Apply this package to **main** only.

## Build targets
- `wolfie_right_keys_trackball` — original verified right-half firmware (no display).
- `wolfie_right_keys_trackball_eye` — same firmware plus custom static eye on nice!view.

The eye build adds the external `wolfie-niceview` module in `config/west.yml`. The local `wolfie_display_bus` shield supplies the `nice_view_spi` SPI1 device label required by the custom display, independently of the PMW3610 on SPI0.

Pinout: nice!view CS=P0.06, SCK=P0.20, MOSI=P0.17; power=VCC and GND. All physical wiring is unchanged.

First run GitHub Actions and verify **both** builds succeed. Flash only `wolfie_right_keys_trackball_eye` after success. Test 22 switches, AltGr+U, trackball, and eye graphic. If integration fails, use original firmware artifact or return to the backup branch.

No animations are modified. This integration has not been compiled locally; if GitHub Actions fails, share the **first error** in the failing build job.
