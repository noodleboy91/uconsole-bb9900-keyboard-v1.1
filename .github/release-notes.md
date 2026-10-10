# Upstream v1.5.0 features for V1.1 hardware

This port follows [jhob101's original firmware](https://github.com/jhob101/uconsole-bb9900-keyboard)
while retaining the V1.1 key matrix verified against the stock firmware.

## Choose one file

| File | Trackpad press | B in keyboard mode |
| --- | --- | --- |
| `bb9900-v1.1-zmk.uf2` | Left click | Tap for middle click; hold for D-pad mouse |
| `bb9900-v1.1-zmk-middle-click.uf2` | Middle click | Tap for left click; hold for D-pad mouse |
| `bb9900-v1.1-zmk-one-handed.uf2` | Left click | Right click, hold to drag |

One-handed also uses Y for middle click, X to toggle scrolling, and A for F11.
It replaces the D-pad mouse and kitty shortcut in keyboard mode. All variants
retain the separate L/R mouse buttons, hold-Select scrolling and Fn+G gamepad.

## New since the first V1.1 port

- Scrolling follows finger speed, including small/slow movements, at 60% speed.
- Sensor thumb-lift detection reduces pointer jumps; pointer speed is 120%.
- The trackpad light indicates scrolling mode.
- B tap is now a mouse click, paired with the chosen trackpad-click variant.
- Optional scroll-toggle behavior and a ready-built one-handed layout.
- Upstream pointer-overflow fix and sensor diagnostics.

## Validation and flashing

All three variants compiled with Zephyr SDK 0.16.8. UF2 headers, flash bounds,
reset vectors, V1.1 matrix, feature settings and all variant key bindings were
checked. Keymap and settings match upstream v1.5.0; the ZMK source is pinned.

Earlier V1.1 hardware testing was positive and partially completed. **This update
and its new features still need hardware testing**, so it remains a prerelease.

Use Left Alt + Right Alt + Start to enter the custom firmware's bootloader,
then copy one UF2 to the NRF52BOOT drive. Keep your current working firmware
as a backup. See [commands.txt](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/blob/main/commands.txt)
and [V1.1.md](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/blob/main/V1.1.md)
for flashing instructions and SHA-256 checksums.
