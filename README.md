# Wolfie Split: right-central Bluetooth name build fix

The right-hand shield configured `Wolfie Trackball` (16 characters), which exceeds the strict Bluetooth device-name limit for this ZMK configuration.

This one-line change shortens the central's name to `Wolfie Track` (12 characters). The left peripheral, trackball, matrix and display firmware are unchanged.

Copy the `config` directory's contents into the root of `Wolfie-Split-Trackball-ZMK` on the `main` branch. Replace the matching file, commit and push, then confirm both new split builds succeed in GitHub Actions before flashing either board.

This has not been compiled locally; GitHub Actions is the validation step.
