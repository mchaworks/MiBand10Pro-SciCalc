# SciCalc for Xiaomi Smart Band 10 Pro

**SciCalc is a native scientific calculator for the Xiaomi Smart Band 10 Pro.**
It is not a phone app. The downloadable `.bin` files are watchface packages
that install and launch a native C/LVGL application on the band.

This repository contains three installable releases, the latest calculator
source, and a build script. The installer currently targets Smart Band 10 Pro
firmware `3.201.016`; other firmware versions are not verified.

## Releases

| Release | File | Summary |
| --- | --- | --- |
| v1 | [GitHub release v1](https://github.com/nopiasminar111-stack/MiBand10Pro-SciCalc/releases/tag/v1) | Initial Apple-inspired calculator UI release. |
| v2 | [GitHub release v2](https://github.com/nopiasminar111-stack/MiBand10Pro-SciCalc/releases/tag/v2) | Restored v2 calculator package; original binary preserved. |
| v3 | [GitHub release v3](https://github.com/nopiasminar111-stack/MiBand10Pro-SciCalc/releases/tag/v3) | Latest source/build: main and scientific keypads, wide zero, page navigation, and result backspace. |

Use the v3 package unless you specifically need an earlier version. Each
package is a fixed-size watchface resource container; do not rename it to an
ELF file or try to load the `.bin` directly with `insmod`.

## Feature overview

- Calculator keypad with clear, sign toggle, postfix percent, decimal point,
  arithmetic operators, and equals.
- Scientific functions on additional pages.
- Tap the `SciCalc` pill to switch between the main and scientific keypad.
- `>` and `<` move between scientific pages.
- Scientific-page equals key spans the bottom three rows of the right column.
- Backspace button beside the result removes the last input.
- Expression and result are shown in the display area.

## Installation

1. Confirm that the band is a **Xiaomi Smart Band 10 Pro** running firmware
   `3.201.016`.
2. Download `SciCalc_v3.bin` from the v3 release assets.
3. Install the package using a watchface installation method that supports
   custom Xiaomi Smart Band 10 Pro resource/watchface packages. Follow that
   installer's on-screen steps and let it apply the package to the band.
4. Open the installed SciCalc watchface/installer on the band and tap
   **Install** if prompted. The installer copies the matching native ELF and
   logo, loads the native module, registers the calculator, and then closes.
5. Open **SciCalc** from the watch's app launcher.

The watchface package and the native calculator are distinct stages: applying
the `.bin` installs the small launcher; launching it loads the native app.
The installer removes itself after the native module is loaded. On the first
use, allow the installer to finish and return to the watch launcher before
opening SciCalc.

If installation reports an unsupported firmware, stop there. Do not force an
ELF built for a different firmware onto the device.

## Using the calculator

### Main keypad

- Tap digits and operators to enter an expression.
- `C` clears the current input.
- `±` toggles the sign of the current number or term.
- `%` applies postfix percent.
- `.` enters a decimal point.
- `=` evaluates the expression.
- Tap the arrow beside the large result to delete the last input.
- The wide `0` key occupies two keypad columns.

### Scientific keypads

- Tap the `SciCalc` title pill to enter the scientific pages; tap it again to
  return to the main keypad.
- Tap `>` or `<` to move between the scientific pages, or tap a page dot.
- Scientific functions include trigonometric and inverse functions,
  hyperbolic functions, logarithms, roots, powers, factorial, constants, and
  memory controls. The exact keys are shown on the watch.
- The `=` key occupies the bottom three rows of the right-hand column.

## Build from source

The self-contained build tree is in [`source/`](source/). It includes the
calculator C source, decimal expression engine, firmware `3.201.016` bindings,
watchface installer, logo, resource repacker, and linker script.

Requirements:

- `clang` with ARM bare-metal target support
- `ld.lld`
- Python 3
- A compatible Smart Band 10 Pro `resource.bin` template

From the `source/` directory:

```sh
sh build_scicalc.sh /path/to/resource.bin /path/to/SciCalc_custom.bin
```

The build targets ARM Cortex-M33 with the firmware's hard-float ABI. It retries
with size optimization when needed, checks that the ELF fits the resource
slot, and preserves the resource package's total size. The resource template
is not included because it is device/firmware-specific.

## Source layout

- `source/mb10pro/mb10p_sci.c` — native calculator UI and input handling.
- `source/mb10pro/sci_engine.h` — decimal expression evaluator.
- `source/mb10pro/wf_native_sci/app/lua/main.lua` — watchface installer.
- `source/platform/mb10p/fw_3.201.016/` — firmware-specific bindings.
- `source/build_scicalc.sh` — compile and package script.
- `releases/` — v1, v2, and v3 installable `.bin` packages.

## Known limitations

- Only firmware `3.201.016` is currently verified by the installer.
- Appearance and font glyph rendering can vary slightly with device rendering;
  verify the result on the watch after installation.
- The pressed-state pill uses a light background with its normal text color;
  this firmware binding does not currently expose a verified text-color setter.
- Swipe-to-delete is not enabled.

## Checksums

SHA-256:

```text
v1  b7ebc2a224ffc24f741e6302beacfeb9061e060d2439e5a69b4b68ce17ab5982  SciCalc_v1.bin
v2  4690e0c31342fb216a81ddf6bfc6b26efa09f7a8d10e12e0c309d738920cea53  SciCalc_v2.bin
v3  a76281232088fce7685736eb4b2578643ae7218000e2faae880385d1404435cb  SciCalc_v3.bin
```

## License

The source is provided under the MIT License; see [`LICENSE`](LICENSE).
