# Wolfie left Corne build target — complete fix

This update registers the left Corne firmware target **and** includes its shield definition and the final user-requested keymap. It is intended for `Wolfie-Split-Trackball-ZMK` on `main`.

## Layout, left to right

```
Tab    Q  W  E  R  T
Shift  A  S  D  F  G
Ctrl   Z  X  C  V  B
    None | Win | None | Space
```

The row 0 first two matrix positions are physically unused. The four thumb positions are scan row 0 columns 2–5. There is no layer activation bound to the thumbs.

## Installation (GitHub Desktop)

1. In `Wolfie-Split-Trackball-ZMK`, select branch `main`, not `working-complete-right-half`.
2. Extract this ZIP, copying **the contents** (the `config/` folder and `build.yaml`) into the repository root. Merge folders and replace the existing `build.yaml` and any matching files; never remove `.git`.
3. In GitHub Desktop, verify that `build.yaml` has a new entry for `shield: wolfie_left_corne`; that `config/boards/shields/wolfie_left_corne/` contains its files; and that `config/wolfie_left_corne.keymap` exists.
4. Commit and push, then view GitHub Actions. The run should contain one additional firmware UF2, `wolfie_left_corne_no_layer.uf2`.
5. Flash **only** `wolfie_left_corne_no_layer.uf2` to the LEFT nice!nano. Leave the right-hand firmware untouched.

**Note:** The supplied `build.yaml` is based on the seven-build matrix version currently published on GitHub. If you added build targets locally since then, do not overwrite those additions: append the `wolfie_left_corne` entry instead.

## First firmware test

Connect by USB, keep battery disconnected, and test all 22 switches. The left display remains deliberately disabled for this matrix/keymap test. This is a standalone left-half build, not yet the wireless split configuration.

The files were checked for consistency but have not been compiled here; GitHub Actions provides the first build validation.
