# qmk-workspace

This repo stores my Lily58 QMK keymap and config. It is meant to be used as an external QMK userspace overlay for a `lily58/rev1` with `Pro Micro`, using a Portuguese (Portugal) host layout.

## Flashing Firmware On A New Machine

First install the QMK CLI by following the official guide: <https://docs.qmk.fm/newbs_getting_started>. By following this you should get

- a working `qmk` command
- a local `qmk_firmware` checkout created by `qmk setup`

After that, clone this repo :

```bash
git clone  git@github.com:jvsonica/qmk-workspace.git jvsonica-workspace
cd jvsonica-workspace
```

Tell QMK to use this repo as the external userspace overlay:

```bash
qmk config user.overlay_dir="$(pwd)"
qmk userspace-doctor
```

`qmk userspace-doctor` should report that this repo is a valid userspace.

Then go to your existing `qmk_firmware` checkout from `qmk setup` and build the firmware:

```bash
cd /path/to/qmk_firmware
qmk compile -kb lily58/rev1 -km jvsonica
```

Then flash the left half:

```bash
qmk flash -kb lily58/rev1 -km jvsonica -bl avrdude-split-left
```

When QMK is waiting for the bootloader, press reset on the left half.

Then flash the right half:

```bash
qmk flash -kb lily58/rev1 -km jvsonica -bl avrdude-split-right
```

When QMK is waiting for the bootloader, press reset on the right half.

Flash notes:

- Disconnect the TRRS cable before flashing
- Flash one half at a time
- Build once before flashing
- Reconnect TRRS only after both halves are flashed
