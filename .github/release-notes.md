# Upstream v1.5.2 features for V1.1 hardware

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

Pre-release builds append `_PreR_<version>-v1.1` to these names, so a downloaded
file is self-identifying.

## New since the first V1.1 port

- Scrolling follows finger speed, including small/slow movements, at 60% speed.
  **Horizontal scrolling now goes the right way** (upstream 1.5.2 fix).
- Pointer speed is 120%. Sensor lift detection is disabled for V1.1 — it was
  confirmed incompatible with this hardware, and upstream also removed it in
  release 1.5.1.
- The trackpad light indicates scrolling mode.
- B tap is now a mouse click, paired with the chosen trackpad-click variant.
- Optional scroll-toggle behavior and a ready-built one-handed layout.
- Upstream pointer-overflow fix and sensor diagnostics.

## Validation and flashing

All three variants built cleanly and were checked for UF2 headers, flash bounds,
reset vectors, the V1.1 matrix, feature settings and all variant key bindings.
The keymap matches upstream; settings differ only in disabling sensor
lift-detection programming (`CONFIG_INPUT_A320_OFN_ENGINE=0x00`). The ZMK source
is pinned to the upstream 1.5.2 fork revision (horizontal scroll fix).

The standard and middle-click variants are hardware-tested and work as expected
(tracking, horizontal and vertical scrolling, clicks, and the Y/B D-pad mouse).
The one-handed variant builds cleanly but is not yet hardware-confirmed. The
earlier `v1.1-hardware-port.1` release is kept published as a recovery build.

If upgrading from the initial v1.5.0 build, **fully shut down and power back on
after flashing** to clear the sensor's previous volatile setting.

Enter bootloader with Left Alt + Right Alt + Start, or with the physical reset
button on the underside of the PCB next to the crystal (it may have kapton tape
over it; connect the keyboard to another PC with the USB-to-POGO adapter that
came with the board), then copy one UF2 to the NRF52BOOT drive that appears.
Keep your current working firmware as a backup. See
[V1.1.md](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/blob/main/V1.1.md)
for flashing instructions and SHA-256 checksums.