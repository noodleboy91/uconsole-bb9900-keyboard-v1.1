# uConsole trackpad keyboard firmware

## V1.1 hardware port

This repository ports [jhob101's firmware](https://github.com/jhob101/uconsole-bb9900-keyboard)
to the V1.1 keyboard's matrix wiring while preserving its features and logical keymap.

**Original custom firmware creator:** [jhob101](https://github.com/jhob101).
See the [original firmware repository](https://github.com/jhob101/uconsole-bb9900-keyboard)
for the features this port builds on. Earlier contributors and the vendor are
credited in [Credits and licence](#credits-and-licence).

Download the compiled firmware from [this repository's releases](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/releases):

- **`bb9900-v1.1-zmk.uf2`**: trackpad press is left mouse click.
- **`bb9900-v1.1-zmk-middle-click.uf2`**: trackpad press is middle mouse click.
- **`bb9900-v1.1-zmk-one-handed.uf2`**: trackpad left click; Y middle click,
  B right click, X scroll toggle, A F11.

This version follows **upstream v1.5.0**. **V1.1 hardware testing is partially
completed for the earlier release**, with positive user feedback. The new
v1.5.0 features and one-handed variant need hardware testing. See
[V1.1 details](V1.1.md) and [flashing commands](commands.txt).

### New in this update

- Scrolling follows finger speed, with overall scroll speed set to 60%.
- Pointer speed is now 120%. The new sensor lift detection is **disabled on
  V1.1** after a report of missed movement, pointer jumps and stopping with
  the initial v1.5.0 build. Rolling back restored operation; this revised
  configuration still needs hardware verification.
- The trackpad light indicates scroll mode.
- B tap is middle click in the standard firmware, or left click in the
  middle-click-trackpad firmware. Holding B still enables D-pad mouse movement.
- An optional one-handed layout adds a scroll toggle and face-button clicks.

## Main additions

These features from jhob101's firmware are preserved in this V1.1 port:

- **Hold Select and move the trackpad to scroll.** Hold, not a toggle.
- **Gamepad mode on Fn+G.** The keyboard shows up as a real USB joystick,
  with the D-pad on the axes and A/B/X/Y/Select/Start as buttons.
- **Trackpad press as a mouse click.** Choose left click or middle click,
  or the separate one-handed layout.
- **Start is Super**, and holding **Y or B** turns the D-pad into a mouse
  in the standard and middle-click layouts.
- **Keyboard lock on Fn+Esc.** It ignores the keys and trackpad and turns
  the backlight off, which is handy for carrying it around.
- **A bootloader shortcut that's hard to hit by accident.** Hold Left Alt
  and Right Alt, then press Start. This replaces the easier-to-trigger stock
  shortcut. The fallback is to hold Left Fn, then Right Fn, then press `\`.

## Other changes / stuff to be aware of

A few choices inherited from the upstream author's setup:

- **Bluetooth is switched off**; the keyboard connects over USB.
- **Shift/Ctrl/Alt are one-shot modifiers**: tap once and the modifier applies
  to the next key. Holding a modifier works normally. In gamepad mode, these
  are plain modifiers.
- **The A button is a Ctrl+Shift shortcut key for kitty terminal** in keyboard
  mode (F11 in the one-handed layout); in gamepad mode, it is a joystick button.

All of these can be changed if you fork the repository and build your own firmware.

---

Custom firmware for the [hack2you uConsole trackpad keyboard kit](https://hack2you.tech/products/uconsole-trackpad-keyboard):
the replacement uConsole keyboard with an optical trackpad in place of the
trackball.

**The original build is for the V2.1 keyboard.** This port also includes a
V1.1 build using the vendor's older matrix wiring. V1.1 hardware testing is
partially completed, with positive results so far; see [V1.1 port](V1.1.md)
for the correct file and build instructions.

Back up your current firmware first (see [Flashing](#flashing)).

## Contents

- [What it does](#what-it-does)
- [Changes in this fork](#changes-in-this-fork)
- [Building](#building)
- [Flashing](#flashing)
- [Desktop setup](#desktop-setup)
- [Customising](#customising)
- [Known limitations](#known-limitations)
- [Credits and licence](#credits-and-licence)

## What it does

USB only: Bluetooth is switched off.

### Keyboard mode

| Control | What it does |
| --- | --- |
| Trackpad | Moves the pointer. Pressing it is a left click. |
| L / R buttons | Left click / right click. |
| D-pad | Arrow keys. |
| Select | Hold and move the trackpad to scroll. The trackpad light goes out while scrolling. A tap sends the Select key (`KEY_FRONT` on Linux). |
| Start | Super. |
| A | The [kitty key](#kitty-key). |
| Y, B | Hold either and the D-pad moves the pointer. |
| Y tap | F21. |
| B tap | Middle click, sent on release, so it cannot drag. |
| X | F22. |
| Speaker | Volume down. With Shift, volume up. With Fn, mute. |
| Shift, Ctrl, Alt, AltGr | [One-shot modifiers](#one-shot-modifiers). |

The [middle-click firmware](#which-file) swaps two of these: pressing the
trackpad is a middle click and a tap on B is a left click.

### One-handed layout

A third firmware puts the mouse buttons and a scroll switch on the face
buttons, for right-hand use:

| Control | What it does |
| --- | --- |
| Y | Middle click |
| B | Right click |
| X | Scroll switch: press to scroll with the trackpad, press again to stop. The trackpad light is out while it is on. |
| A | F11 |

Y and B click on press and can be held to drag. Everything else is as above,
except that there is no D-pad pointer and no kitty key.

### One-shot modifiers

Tap Shift, Ctrl, Alt or AltGr and it applies to the next key only. Holding
works as normal. An unused tap expires after 60 seconds.

### Kitty key

A is a one-shot Ctrl+Shift for the [kitty](https://sw.kovidgoyal.net/kitty/)
terminal: tap A, then C, for Ctrl+Shift+C. Holding A works too. It covers:

- Letters `Q W E R T P S F G H J K L X C V B N M`
- `Enter`, the arrow keys, `[ ] / - = , .`
- `1` to `9`, sent as Ctrl+Shift+F1 to F9

Any other key is sent unchanged.

### Fn keys

Either Fn key works unless a row says otherwise.

| Keys | What it does |
| --- | --- |
| Fn + `1` to `0` | F1 to F10 |
| Fn + `-`, Fn + `=` | F11, F12 |
| Fn + Backspace | Delete |
| Fn + Tab | Caps Lock (no indicator light) |
| Fn + U, Fn + K | Page Up, Page Down |
| Fn + H, Fn + J | Home, End |
| Fn + I | Insert |
| Fn + `,`, Fn + `.` | Screen brightness down, up |
| Fn + Space | Key backlight on/off. It turns itself off after 30 seconds idle. |
| Fn + Speaker | Mute |
| Fn + Select | Print |
| Fn + Start | Media pause |
| Fn + G | [Gamepad mode](#gamepad-mode) on/off |
| Fn + Esc | [Lock](#lock) / unlock |
| Left Fn + D-pad up / down | Volume up / down |
| Left Fn + Left Alt | Super |
| Right Fn, then Left Fn | Restart the keyboard firmware |

### Gamepad mode

Fn + G toggles it. The keyboard also appears as a USB joystick
(`/dev/input/js0` on Linux):

| Control | Joystick |
| --- | --- |
| D-pad | X and Y axes |
| A, B, X, Y | Buttons 1 to 4 |
| Select | Button 5. Holding it still scrolls. |
| Start | Button 6 |

Modifiers are not one-shot in this mode. Everything else works as usual.

To test it with [sdl-jstest](https://github.com/Grumbel/sdl-jstest):

```sh
git clone https://github.com/Grumbel/sdl-jstest.git
cd sdl-jstest && mkdir build && cd build
cmake .. -DBUILD_SDL_JSTEST=OFF -DBUILD_SDL3_JSTEST=OFF && make
./sdl2-jstest --list && ./sdl2-jstest --test 0
```

### Lock

Fn + Esc locks the keyboard for carrying the uConsole around: keys, mouse
buttons and trackpad are ignored and the backlight goes off. Release Fn, then
Fn + Esc again to unlock. It does not blank the screen, and the keyboard always
starts unlocked.

### Bootloader

Two ways into the bootloader for [flashing](#flashing):

- **Left Alt + Right Alt + Start.** Not available in gamepad mode.
- **Hold Left Fn, then Right Fn, then press `\`.** The order matters.

Neither works while the keyboard is locked.

## Changes in this fork

A fork of [thoughtfix/fix9900](https://github.com/thoughtfix/fix9900), itself a
fork of the vendor's [Bill-lulu/uc9900](https://github.com/Bill-lulu/uc9900).
Added here:

- Gamepad mode, as a real USB joystick.
- Hold Select to scroll, replacing fix9900's click-the-trackpad toggle. An
  optional [scroll switch](#scroll-switch) is there for those who prefer one.
- Trackpad press as left click, middle click on B, and a second firmware with
  the two swapped.
- A one-handed firmware, with the mouse buttons on the face buttons.
- Start as Super, the kitty key and one-shot modifiers, replacing a
  [keyd](https://github.com/rvaiya/keyd) config.
- Y or B plus the D-pad as a mouse.
- The keyboard lock.
- Pointer overflow fix and pointer speed up 20%. Upstream sensor lift detection
  is disabled in this V1.1 port while investigating a trackpad regression.
- Gentler scrolling that follows your finger speed: slow movement scrolls
  slowly.
- The trackpad light shows scroll mode.
- Bluetooth off.
- Fixes: Shift + Speaker no longer leaves Shift stuck, and the bootloader can
  no longer be entered by accident with `\`.

thoughtfix's own fixes are kept: see [`config/NOTES.md`](config/NOTES.md).
The changes to ZMK itself are in [jhob101/zmk](https://github.com/jhob101/zmk).

## Building

**You do not need to build anything.** Download a file from the
[releases page](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/releases)
and go to [Flashing](#flashing).

### Which file

| File | What is different |
| --- | --- |
| `bb9900-v1.1-zmk.uf2` | Trackpad left click, B tap middle click. |
| `bb9900-v1.1-zmk-middle-click.uf2` | Trackpad middle click, B tap left click. |
| `bb9900-v1.1-zmk-one-handed.uf2` | The [one-handed layout](#one-handed-layout). |

All three use V1.1 wiring. The separate L/R buttons remain left/right click.

Build your own only to change the keymap or settings.

### On GitHub

1. Fork this repo and enable Actions on your fork.
2. Push a commit, or run the workflow from the **Actions** tab.
3. Download the `firmware` artifact from the finished run. It holds all three
   files.

### Locally

Needs Python 3, `cmake`, `ninja` and `git`. Tested on Linux x86-64.

```sh
# 1. west, the Zephyr build tool. If pip refuses to install system-wide:
#    python3 -m venv ~/zmk-venv && . ~/zmk-venv/bin/activate
pip install west

# 2. this repo, then the firmware source it builds against
git clone -b main https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1.git
cd uconsole-bb9900-keyboard-v1.1
west init -l config
west update
west zephyr-export
pip install -r zephyr/scripts/requirements-base.txt

# 3. the ARM toolchain (Zephyr SDK 0.16.8)
cd ~
wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.8/zephyr-sdk-0.16.8_linux-x86_64_minimal.tar.xz
wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.8/toolchain_linux-x86_64_arm-zephyr-eabi.tar.xz
tar xf zephyr-sdk-0.16.8_linux-x86_64_minimal.tar.xz
tar xf toolchain_linux-x86_64_arm-zephyr-eabi.tar.xz -C zephyr-sdk-0.16.8

# 4. build
cd ~/uconsole-bb9900-keyboard-v1.1
west build -s zmk/app -d build -b bb9900 -- \
  -DZMK_CONFIG="$(pwd)/config" \
  -DDTS_EXTRA_CPPFLAGS=-DBB9900_V1_1 \
  -DZEPHYR_SDK_INSTALL_DIR="$HOME/zephyr-sdk-0.16.8"
```

The firmware is `build/zephyr/zmk.uf2`. Add `-p always` to `west build` after
changing a `.conf` file or `west.yml`.

### Publishing a release

Change the number in the [`VERSION`](VERSION) file on `main`, for example from
`1.0.0` to `1.0.1`. You can edit it straight on GitHub. That builds the
firmware and publishes a GitHub prerelease named `v1.0.1-v1.1` with the `.uf2`
attached.

The release is built against the `jhob101/zmk` revision pinned in `config/west.yml`.

The text on the release page comes from
[`.github/release-notes.md`](.github/release-notes.md). Editing that file on
its own updates the text of the current release without touching its firmware
file.

## Flashing

The built-in keyboard stops typing in bootloader mode, but the uConsole keeps
running. Connect over SSH **before** entering the bootloader, or use an external
USB keyboard. Flash one UF2 at a time.

### 1. Enable SSH on the uConsole

If SSH is not already enabled, run these on the **uConsole** while its keyboard
still works:

```sh
sudo systemctl enable --now ssh
hostname -I
```

Both computers should be on the same network. The examples below use
`matt@clockworkpi`, as on the tested uConsole. Use your own username and the IP
address printed by `hostname -I` if that hostname does not resolve.

### 2. Copy the firmware from your other computer

Download your chosen V1.1 UF2 from the [releases page](https://github.com/noodleboy91/uconsole-bb9900-keyboard-v1.1/releases)
into your other computer's `Downloads` folder. In a terminal on **that computer**,
run:

```sh
ssh matt@clockworkpi 'mkdir -p ~/Downloads'
scp ~/Downloads/bb9900-v1.1-zmk.uf2 matt@clockworkpi:Downloads/
ssh matt@clockworkpi
```

For middle-click or one-handed firmware, replace the filename in the `scp`
command with `bb9900-v1.1-zmk-middle-click.uf2` or
`bb9900-v1.1-zmk-one-handed.uf2`. If the file is already on the uConsole, skip
the copy and just connect with SSH.

**Run the remaining shell commands inside this SSH session on the uConsole.**

### 3. Enter bootloader mode using the physical keyboard

| Firmware currently installed | Shortcut |
| --- | --- |
| Vendor stock V1.1 firmware | Hold **Right Fn**, then press **`\`** (backslash/pipe). |
| This custom firmware | Hold **Left Alt + Right Alt**, then press **Start**. |
| Custom firmware fallback | Hold **Left Fn**, then also **Right Fn**, then press **`\`**. The order matters. |

On the custom firmware, unlock with Fn+Esc first if locked. The Alt+Start route
does not work in gamepad mode; use the Fn fallback or leave gamepad mode first.
These shortcuts are pressed on the built-in keyboard, not typed over SSH.

### 4. Find and mount the bootloader drive

```sh
lsblk -o NAME,LABEL,SIZE,FSTYPE,MOUNTPOINTS
```

Look for a roughly 32 MB FAT drive labelled **NRF52BOOT** (or **ADM840BOOT**).
It may be unmounted, so it will not appear in `df`. On the tested V1.1 it was
`/dev/sda`, with no partition number:

```text
sda  NRF52BOOT  32.1M  vfat
```

If your output matches, mount it with:

```sh
sudo mkdir -p /mnt/keyboard
sudo mount /dev/sda /mnt/keyboard
ls /mnt/keyboard
```

Use the device shown by your own `lsblk` output if it differs. If it is already
mounted, use that mount point in the commands below instead of mounting it again.

### 5. Back up and flash

Save the current firmware under a dated name:

```sh
cp /mnt/keyboard/CURRENT.UF2 "$HOME/keyboard-backup-$(date +%Y%m%d-%H%M%S).uf2"
```

Then copy **your chosen firmware** to the bootloader drive. For left click:

```sh
sudo cp ~/Downloads/bb9900-v1.1-zmk.uf2 /mnt/keyboard/
sync
```

Or, **instead**, for middle click:

```sh
sudo cp ~/Downloads/bb9900-v1.1-zmk-middle-click.uf2 /mnt/keyboard/
sync
```

Or, **instead**, for the one-handed layout:

```sh
sudo cp ~/Downloads/bb9900-v1.1-zmk-one-handed.uf2 /mnt/keyboard/
sync
```

The drive should disappear and the keyboard should restart automatically.
Your SSH session should remain connected. Test typing, Enter, the bottom-row
modifiers, trackpad clicks and scrolling after it restarts.

**If you flashed the initial v1.5.0 build with lift detection enabled**, save
your work, run `sudo poweroff`, wait for shutdown, then turn the uConsole back
on. Disabling the option stops programming the sensor but does not clear a
previous setting until the keyboard loses power.

### Recovery

Keep your backup and known-working stock `KB1V1-1V31.uf2`. To restore stock,
enter the bootloader again, identify and mount its drive as above, then run
this over SSH if the stock file is in Downloads:

```sh
sudo cp ~/Downloads/KB1V1-1V31.uf2 /mnt/keyboard/
sync
```

You can also restore a saved `CURRENT.UF2` backup by copying that file to the
drive. If the firmware cannot process keyboard shortcuts, entering the
bootloader may require the board's hardware reset procedure.

## Desktop setup

Nothing to install. If you ran keyd for one-shot modifiers or a kitty key,
stop it, or the two will stack:

```sh
sudo systemctl disable --now keyd
```

## Customising

| File | What it holds |
| --- | --- |
| [`config/bb9900.keymap`](config/bb9900.keymap) | Every key on every layer, with comments. Start here. |
| [`config/bb9900.conf`](config/bb9900.conf) | Settings, each with a comment. |
| [`config/west.yml`](config/west.yml) | Which ZMK source to build against. |

Common changes:

| What | Where |
| --- | --- |
| Trackpad press and B tap | `TRACKPAD_PRESS` and `B_CLICK` at the top of the keymap: `LCLK`, `MCLK` or `RCLK` |
| Y, X, B and A | `Y_KEY`, `X_KEY`, `B_KEY` and `A_KEY` at the top of the keymap |
| Pointer speed | `CONFIG_TRACKPAD_SPEEDMULTIPLIER_HORIZONTAL` and `_VERTICAL`, in percent |
| Scroll speed | `CONFIG_TRACKPAD_SCROLL_SPEED`, in percent |
| Lift detection | `CONFIG_INPUT_A320_OFN_ENGINE=0x00` on V1.1 (sensor programming disabled); upstream uses `0xA0`. Power-cycle after changing from `0xA0`. |
| Trackpad light | `CONFIG_ZMK_TRACKPAD_SCROLL_LIGHT`: `n` leaves it alone |
| Backlight auto-off delay | `CONFIG_ZMK_IDLE_TIMEOUT`, in milliseconds |
| One-shot timeout | `release-after-ms` on `osm` and `osl` in the keymap |
| Kitty key list | `kitty_layer` in the keymap |

Avoid ZMK's `combos` feature: on this board it stops the Enter key working.

To see what a key sends: `sudo libinput debug-events --show-keycodes`

### Scroll switch

For a key that turns scrolling on and off, as on the stock firmware, bind it to
`&scroll_toggle`. For X on the standard layout, set `X_KEY` to
`&scroll_toggle`. Holding Select still scrolls, and the trackpad light is out
while the switch is on.

### Variants

<a name="example-a-one-handed-layout"></a>
The middle-click and one-handed firmware are the same keymap built with
`MIDDLE_CLICK_TRACKPAD` or `ONE_HANDED` defined, from
[`build.yaml`](build.yaml). The one-handed layout is the four `_KEY` values
under `#ifdef ONE_HANDED`, a worked example to copy for a layout of your own.

## Known limitations

- **V1.1 hardware testing is partially completed.** Initial user feedback is
  positive for the earlier release; the v1.5.0 update needs hardware testing.
  The original upstream firmware was tested on V2.1.
- **DO NOT ENABLE SENSOR LIFT DETECTION ON V1.1.** Setting `CONFIG_INPUT_A320_OFN_ENGINE=0xA0`
  is incompatible with V1.1 hardware and causes trackpad malfunctions including
  missed movements, pointer jumps, and unresponsive trackpad. This has been tested
  and confirmed broken. The setting must remain at `0x00` for all V1.1 builds.
  This is a permanent hardware incompatibility, not a configuration issue.
- **Left Shift after Fn does nothing.** Press Shift before Fn, or use Right
  Shift.
- **Unlocking needs Fn released first.**
- **The Alt keys are one-shot**, so tapping both and then pressing Start also
  enters the bootloader.
- **The D-pad pointer and the trackpad share one mouse report**, so using both
  at once can feel uneven.
- **Based on a December 2023 ZMK.** Current ZMK documentation may not apply.

## Credits and licence

- [ZMK](https://github.com/zmkfirmware/zmk), the firmware underneath.
- [ZitaoTech](https://github.com/ZitaoTech/zmk), for the BB9900 keyboard and
  trackpad support.
- [Bill-lulu](https://github.com/Bill-lulu/uc9900), for the uConsole port.
- [Daniel Gentleman (thoughtfix)](https://github.com/thoughtfix/fix9900), for
  the fixes this fork starts from.
- [j1n6's qmk-uconsole](https://github.com/j1n6/qmk-uconsole), the model for
  gamepad mode, Select scrolling and the keyboard lock.

MIT licensed, the same as upstream. See [`LICENSE.txt`](LICENSE.txt).

The changes in this fork were written with the help of Claude (Anthropic).
