Wolfie Bluetooth smoothness: 7.5 ms BLE + 7 ms sensor-report test
===================================================================

This ZIP is for the Wolfie-Split-Trackball-ZMK repository (NOT wolfie-niceview).

INSTALL
1. In GitHub Desktop, choose the Wolfie-Split-Trackball-ZMK repository and the branch
   you used for your previous successful 7.5ms Bluetooth diagnostic build.
2. Unzip this archive; copy build.yaml into the ROOT of that repository, replacing
   the existing file. The ZIP contains a complete build.yaml based on the previously
   supplied 7.5ms diagnostic build; compare in GitHub Desktop before committing,
   especially if you have added any other build jobs since then.
3. GitHub Desktop should show ONE modified file: build.yaml.
4. Commit 'Add 7ms sensor report interval Bluetooth diagnostic' and push.
5. Run the ZMK GitHub Actions workflow.
6. Download the freshly built artifact and flash ONLY the right nice!nano:
      wolfie_split_right_bt75_sensor7_no_display.uf2
7. Disconnect USB and collect a new MouseTester Interval vs Time graph for Wolfie
   over Bluetooth. Compare to the previous 7.5ms BLE / 12ms sensor test.

NEW JOB (no-display diagnostic, not a replacement of existing targets):
  Bluetooth peripheral preferred minimum interval = 6 * 1.25ms = 7.5ms
  Bluetooth peripheral preferred maximum interval = 6 * 1.25ms = 7.5ms
  Bluetooth automatic connection parameter update request = enabled
  PMW3610 minimum report interval = 7ms
  PMW3610 CPI = existing overlay setting (you previously changed to 1200)
  Right screen = not enabled in this target; left half unchanged.

IMPORTANT:
- The keyboard's host (Windows) controls the final negotiated BLE interval.
- The PMW3610 normally samples at ~8ms; a 7ms report ceiling does NOT guarantee
  7.5ms mouse reports. If this doesn't improve, the separate force-awake 4ms
  sampling feature may be an option for later testing, with higher power use.
- This archive has NOT been compiled in GitHub Actions. Verify the new job builds.
- Other firmware targets in build.yaml were intentionally left unchanged.
- To revert, flash the previously working right UF2 from your earlier build.
  For the original display and eye, flash wolfie_split_right_central.uf2.
- Keep both display repositories and existing paired Bluetooth settings intact.

If the job fails, send the FIRST build error and the generated command line.
