# Upstream v1.6.0 features for V1.1 hardware

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

**This is a pre-release**, so the downloaded files carry a `_PreR_1.6.0-v1.1`
suffix (for example `bb9900-v1.1-zmk_PreR_1.6.0-v1.1.uf2`). The stable
[`v1.5.2-v1.1`](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/releases/tag/v1.5.2-v1.1)
release uses the plain names, and
[`v1.1-hardware-port.1`](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/releases/tag/v1.1-hardware-port.1)
is kept as a recovery build.

## New in 1.6.0

- **Trackpad light on/off with Fn + `/`.** The choice is remembered across
  restarts. The light also goes out while the keyboard is locked.
- **One-shot modifiers are easier to leave.** Tap one again to cancel it, and a
  mouse click or a scroll now uses it up, as a key press does, so a stray Shift
  no longer turns every scroll sideways.

Carried over from 1.5.x: scrolling follows finger speed at 60%; horizontal
scrolling goes the right way; pointer speed 120%; sensor lift detection is
disabled for V1.1; the trackpad light shows scroll mode; B tap is a mouse click;
optional scroll toggle and one-handed layout; upstream pointer-overflow fix and
sensor diagnostics.

## Validation and flashing

All three variants build cleanly against the pinned upstream 1.6.0 source. This
pre-release is **not yet hardware-confirmed**; the stable `v1.5.2-v1.1` release's
standard and middle-click variants are hardware-tested and work as expected.

If upgrading from the initial v1.5.0 build, **fully shut down and power back on
after flashing** to clear the sensor's previous volatile setting.

Enter bootloader with Left Alt + Right Alt + Start, or with the physical reset
button on the underside of the PCB next to the crystal (it may have kapton tape
over it; connect the keyboard to another PC with the USB-to-POGO adapter that
came with the board), then copy one UF2 to the NRF52BOOT drive that appears.
Keep your current working firmware as a backup. See
[V1.1.md](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/blob/main/V1.1.md)
for flashing instructions and SHA-256 checksums.
