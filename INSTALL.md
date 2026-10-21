Wolfie Split Trackball - 4 ms Sensor Sampling Diagnostic
====================================================

PURPOSE
  The successful no-display BLE test requested a 7.5 ms host connection
  interval and used CONFIG_PMW3610_ALT_REPORT_INTERVAL_MIN=7. MouseTester
  improved to ~8 ms average but reports still arrived unevenly.

  This *additional* experimental build keeps those two settings and changes
  ONLY the PMW3610 active sampling mode via a second overlay:
    - force-awake;              (keeps PMW3610 in active/run state while ZMK is active)
    - force-awake-4ms-mode;     (requests a 4 ms sampling period in run mode)

  The driver documents the normal run sampling cadence as 8 ms.
  Faster sampling can increase battery consumption. A smoother graph is
  NOT guaranteed, because Bluetooth/HID timing still affects host arrivals.

INSTALL USING GITHUB DESKTOP
  1. Select your Wolfie-Split-Trackball-ZMK repository (NOT wolfie-niceview).
  2. Make sure you are on the branch with the previously working 7.5 ms BLE +
     7 ms sensor-report diagnostic. Save a backup branch if you prefer.
  3. Unzip and copy the INCLUDED build.yaml to the repository root,
     and INCLUDED config/wolfie_pmw3610_4ms.overlay into the config folder.
     Compare the build.yaml diff before committing, especially if you have
     added any new targets since the previous test.
  4. GitHub Desktop should show an updated build.yaml and a NEW file:
       config/wolfie_pmw3610_4ms.overlay
     No source-file modifications are required to either display module.
  5. Commit 'Add 4ms PMW3610 sampling diagnostic' and push.
  6. Trigger a NEW GitHub Actions firmware build.
  7. Download the new artifact and flash ONLY onto the RIGHT nice!nano:
       wolfie_split_right_bt75_sensor4_awake_no_display.uf2
     (do not flash the left half; do not clear Bluetooth pairings).
  8. Disconnect USB; allow BLE connection to settle; record 5-10 seconds of
     steady trackball circles in MouseTester, Interval vs. Time.
  9. Send the graph and say whether movement feels better, similar, or worse.

COMPARISON / ROLLBACK
  Previous known useful build:
    wolfie_split_right_bt75_sensor7_no_display.uf2
  Full display/animated eye firmware:
    wolfie_split_right_central.uf2
  You can reflash either previously working UF2 to return to it.

NOTES / LIMITATIONS
  - Host ultimately determines the BLE interval; these are *requests*.
  - The sensor report minimum is still 7ms. The driver reports on motion
    events, not on a synchronized 7.5ms timer, so even 4ms sampling may not
    produce evenly spaced Bluetooth reports.
  - This test may increase power draw during ZMK ACTIVE state and is not
    recommended as permanent firmware until power usage is checked.
  - Package structure and YAML/overlay syntax inspected. NOT compiled or
    hardware tested. If CI fails, share the FIRST error from the build log.
