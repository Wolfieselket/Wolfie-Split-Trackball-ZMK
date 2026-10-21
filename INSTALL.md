# Wolfie: Bluetooth HID Notification Thread Priority 4 Test

This is a NO-DISPLAY diagnostic build for the RIGHT central half ONLY.

## Purpose
The best Bluetooth version so far requested a 7.5 ms BLE host interval and used
PMW3610's 7 ms movement-report minimum. MouseTester showed remaining uneven
arrival times. USB on the SAME sensor7 build felt consistently smooth.

This new build keeps the existing settings and changes only the ZMK Bluetooth
notification worker's priority from default 5 to 4 (higher priority in Zephyr).
It tests a plausible scheduler bottleneck; improvement is NOT guaranteed.

## Install with GitHub Desktop
1. Open Wolfie-Split-Trackball-ZMK, not wolfie-niceview.
2. Make sure your current branch is the one with the previous successful
   wolfie_split_right_bt75_sensor7_no_display diagnostic.
3. Extract this ZIP and copy build.yaml to the ROOT of your repository.
4. Inspect GitHub Desktop's diff BEFORE committing. It should add only the
   new block labeled 'Bluetooth HID notification scheduling test'. If your
   existing build.yaml has diverged, manually merge ONLY the new block rather
   than replacing the whole file. The included build.yaml is based on the
   last 4 ms BUILD-PATH FIX ZIP (which contains the repaired 4 ms shield target).
5. Commit 'Add BLE notify priority 4 diagnostic' and push.
6. Run a fresh GitHub Actions build and wait for SUCCESS.
7. Download fresh artifacts. Flash ONLY the right nice!nano with:
     wolfie_split_right_bt75_sensor7_bleprio4_no_display.uf2
   Do not flash the left or a settings-reset firmware.

## Recording and comparing
- Unplug USB and test Windows over Bluetooth; keep the left half ON.
- Wait 15 seconds after connecting, then test continuous movement for 20-30 s.
- Capture MouseTester 'Interval vs. Time' and note subjective smoothness.
- The best prior version is available as:
     wolfie_split_right_bt75_sensor7_no_display.uf2
- Compare these under similar conditions. Look especially at 99th-percentile
  gaps and the frequency of back-to-back (~1 ms) reports.

## Settings in this one new target
- PMW3610 CPI: unchanged by ZIP (your sensor overlay currently has 1200).
- Sensor report minimum: CONFIG_PMW3610_ALT_REPORT_INTERVAL_MIN=7.
- BLE preferred host interval 7.5ms: MIN_INT=6 MAX_INT=6.
- Automatic BLE connection parameter update: y.
- BLE notification worker thread priority: CONFIG_ZMK_BLE_THREAD_PRIORITY=4.
- No nice!view shield, so eye animation does NOT run.
- No modifications to normal full-display firmware or the keymap.

## Caveats
- The OS ultimately negotiates BLE connection parameters, and priority 4
  does not guarantee a specific polling rate. It might not help or might hurt.
- Previous 4 ms sensor 'force awake' test made jitter worse; this build DOES NOT
  enable the 4 ms override shield.
- Source/YAML/ZIP checked, NOT compiled by GitHub Actions here.
- If compile output warns about CONFIG_ZMK_BLE_THREAD_PRIORITY, send the first
  actual error message before making more modifications.
