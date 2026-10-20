# Wolfie: 7.5 ms Bluetooth host connection interval test (no display)

This package changes **only `build.yaml`** in your **Wolfie-Split-Trackball-ZMK** repo.
It adds one separate diagnostic build, without replacing your working normal firmware,
existing no-display baseline, keymap, trackball overlay, or either nice!view module.

## Installation with GitHub Desktop

1. Open **Wolfie-Split-Trackball-ZMK** on the branch containing the no-display diagnostic.
2. Extract the ZIP and copy `build.yaml` into the **repository root**, replacing the file.
3. In GitHub Desktop, verify the ONLY change in `build.yaml` is the new build entry
   `wolfie_split_right_bt75_no_display` immediately after `wolfie_split_right_no_display`.
   If you have since made other changes to `build.yaml`, keep them and merge this new
   entry manually rather than replacing the whole file.
4. Commit as `Add 7.5ms BLE no-display comparison build` and Push.
5. Run GitHub Actions in Wolfie-Split-Trackball-ZMK.
6. Once successful, download the **new** firmware ZIP/artifacts and flash ONLY
   **`wolfie_split_right_bt75_no_display.uf2`** onto the **right** nice!nano.
   Do not flash left or any settings-reset UF2.
7. Unplug USB; leave the left half on; connect Wolfie to Windows over Bluetooth.
   Wait at least 10-15 seconds before recording MouseTester, because Bluetooth
   connection parameter updates can occur shortly after connection.
8. Collect 5-10 seconds of slow continuous trackball movement and send the
   MouseTester `Interval vs. Time` screenshot for comparison.

## Exact test settings

New build variant only:

```
CONFIG_BT_PERIPHERAL_PREF_MIN_INT=6
CONFIG_BT_PERIPHERAL_PREF_MAX_INT=6
CONFIG_BT_GAP_AUTO_UPDATE_CONN_PARAMS=y
```

Bluetooth values are in 1.25 ms units, so 6 = 7.5 ms. Windows may negotiate
another interval; this is a **request**, not a guaranteed actual connection rate.
This is for the right half's Bluetooth connection **to Windows**; it does not
change the radio interval used for the split-left connection.

Your existing `CONFIG_PMW3610_ALT_REPORT_INTERVAL_MIN=12` and 1200 CPI
remain unchanged by this ZIP. Because of the 12 ms movement-batching limit,
**don't expect 133 mouse reports per second** just from this BLE setting;
we're testing whether timing and consistency improve.

If available, check the GitHub Actions `Kconfig file` step on this specific
BT75 job and confirm the resolved values for `CONFIG_BT_PERIPHERAL_PREF_MIN_INT`,
`CONFIG_BT_PERIPHERAL_PREF_MAX_INT`, and `CONFIG_BT_GAP_AUTO_UPDATE_CONN_PARAMS`.

## Returning to known working versions

- Original no-display test: flash `wolfie_split_right_no_display.uf2`.
- Working animated eye + BT/layer status: flash `wolfie_split_right_central.uf2`.

These should be taken from a successful build that contains the tested config.
Neither requires a Bluetooth settings reset. Leave the left firmware untouched.

## Notes

- This is a source-level diagnostic package, **not a precompiled UF2**.
- We validated ZIP integrity and YAML structure, **not a Zephyr compilation**.
- If the BT75 firmware causes pairing issues or worsens responsiveness, return
  to the previous no-display UF2 without clearing bonds.
