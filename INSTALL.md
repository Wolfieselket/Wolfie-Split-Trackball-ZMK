Wolfie PMW3610 4 ms diagnostic - missing overlay path fix
===========================================================

WHAT FAILED
  The previous build.yaml used:
    -DEXTRA_DTC_OVERLAY_FILE=wolfie_pmw3610_4ms.overlay
  Zephyr couldn't find that relative path, although the file existed under config/.
  The previous build therefore never reached firmware compilation.

WHAT THIS FIX DOES
  Adds a dedicated diagnostic shield, wolfie_pmw3610_4ms, that Zephyr
  automatically loads when listed with the existing wolfie_split_right shield.
  The 4 ms properties are exactly the same as the prior test; the inclusion
  mechanism is the only change. The normal display firmware is not modified.

FILES IN ZIP
  build.yaml
  config/boards/shields/wolfie_pmw3610_4ms/Kconfig.shield
  config/boards/shields/wolfie_pmw3610_4ms/Kconfig.defconfig
  config/boards/shields/wolfie_pmw3610_4ms/wolfie_pmw3610_4ms.overlay

INSTALL WITH GITHUB DESKTOP
  1. Open Wolfie-Split-Trackball-ZMK (not wolfie-niceview).
  2. Make sure you are on the branch with the previous 4 ms diagnostic build.
  3. Unzip and MERGE build.yaml and the config folder into the repository root.
  4. GitHub Desktop should show one changed build.yaml and three new shield files.
  5. Confirm the 4 ms target now says:
       shield: wolfie_split_right wolfie_pmw3610_4ms
     and there is NO -DEXTRA_DTC_OVERLAY_FILE=... argument for that target.
  6. Commit 'Fix 4ms PMW3610 diagnostic overlay' and push.
  7. Run a NEW GitHub Actions workflow.
  8. ONLY if successful, flash RIGHT nice!nano with:
       wolfie_split_right_bt75_sensor4_awake_no_display.uf2
     Keep the left firmware and both display modules unchanged.
  9. Compare MouseTester and tell me whether movement feels smoother.

NOTE
  You may leave the old config/wolfie_pmw3610_4ms.overlay in the repository:
  it will no longer be referenced by the test. Optionally remove it later.
  The normal full-display target and the working 7 ms diagnostic are unmodified.
  Higher sampling may increase active battery consumption.

VALIDATION
  ZIP structure, YAML parsing, target names, and relative shield-file paths
  are checked. A full Zephyr build has NOT been run in this environment.
