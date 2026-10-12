# Nintendont Xbox 360 Experimental

**Unofficial Nintendont fork with experimental wired Xbox 360 controller support for Nintendo Wii and Wii U (vWii).**

Built on [Nintendont by FIX94 and contributors](https://github.com/FIX94/Nintendont) and the wired Xbox 360 controller work from [revvv's Nintendont-XBOX360](https://github.com/revvv/Nintendont-XBOX360). The goal is to improve USB controller reliability while keeping Nintendont's existing GameCube and HID functionality intact.

> [!WARNING]
> **Experimental alpha build.** Initial testing has succeeded on real Wii hardware, but extended stability testing is ongoing. The Xbox 360 additions have **not** yet been validated on Wii U (vWii). This is not an official Nintendont release.

**[Download Alpha 1](https://github.com/Nekkidso/Nintendont-Xbox360-Experimental/releases/tag/v1.2.0-xbox360-alpha.1)** · **[Releases](https://github.com/Nekkidso/Nintendont-Xbox360-Experimental/releases)** · **[Original Nintendont](https://github.com/FIX94/Nintendont)**

---

## Changes in this fork

### Wired Xbox 360 controller support

Based on revvv's implementation, with additional experimental work:

- Wired Xbox 360 USB controller support (`VID 045E`, `PID 028E`).
- Mapping for face buttons, D-pad, analog sticks and GameCube inputs through `controllers/045e_028e.ini`.
- Support for analog triggers, including fully pressed trigger states.
- Revised face-button mapping: Xbox **A → GameCube A**, **B → B**, **X → X**, **Y → Y**.

**Testing note:** The revised A/B/X/Y mapping in Alpha 1 has been successfully validated on real Nintendo Wii hardware using the official GitHub release package.

### USB initialization and hotplug fixes

- **USB cleanup before kernel startup:** releases loader-side USB resources before the game kernel starts, addressing Xbox 360 controller detection issues seen when starting games from SD.
- **Hotplug handling:** improves USB controller disconnection, reconnection and switching between wired Xbox 360 and USB HID controllers.
- **HID configuration cache:** invalidates relevant PowerPC-side cache lines for shared HID configuration data, preventing stale controller layouts after hotplug and the resulting incorrect button or analog stick behavior.

These changes have passed initial tests on real Wii hardware, including game launch from SD and USB storage and repeated Xbox 360 / HID controller switching.

## Install the experimental build

1. Download `Nintendont-Xbox360-Experimental-20261009.zip` from [Alpha 1](https://github.com/Nekkidso/Nintendont-Xbox360-Experimental/releases/tag/v1.2.0-xbox360-alpha.1).
2. Extract the ZIP **to the root** of the SD card or USB storage device. It contains:

    ```text
    apps/
      Nintendont-Xbox-Test/
        boot.dol
        icon.png
        meta.xml
    controllers/
      045E_028E.ini
    ```

3. Put compatible GameCube game files under `/games/`, following the original documentation below.
4. Connect a **wired Xbox 360 USB controller**, then launch **Nintendont Xbox Test** from the Homebrew Channel.

The package installs to a separate `apps/Nintendont-Xbox-Test/` directory so you can keep an existing Nintendont installation alongside it. Back up any controller INI you customize before replacing it.

**Note:** The *original Nintendont* quick-installation instructions further down describe the standard upstream layout. For this experimental alpha, use the packaged paths above.

## Testing status

**Initially validated on a real Nintendo Wii:**

- Wired Xbox 360 controller initialization and gameplay input.
- Starting games from SD and USB storage.
- USB resource cleanup before game startup.
- Controller disconnect/reconnect handling.
- Switching between wired Xbox 360 and USB HID controllers after launching a game.
- PowerPC-side HID configuration cache fix.
- Revised A/B/X/Y mapping validated using the official Alpha 1 release package.

**Still requires testing:**

- Multi-day stability and compatibility across more games.
- Wii U (vWii) behavior for the Xbox 360 modifications.
- Simultaneous use of multiple independent USB controllers.

## Known limitations

- This is experimental and not an official Nintendont release.
- The Xbox 360 implementation targets **wired** USB controllers; wireless Xbox 360 controller support is not provided by this implementation.
- Multiple independent USB controllers connected simultaneously are **not a validated use case**.
- Some HID controllers have device-specific initialization behavior.
- Rumble and player LED behavior can depend on the controller and its `EndpointOut` configuration.
- Not all games, controllers or system configurations have been tested.

## Maintenance and upstream changes

The experimental Xbox 360 support is maintained separately from [upstream Nintendont](https://github.com/FIX94/Nintendont). Official updates will be reviewed and tested before integration, especially changes affecting USB, HID and controller inputs, to avoid regressing the working Xbox 360 implementation.

## Credits

- **[FIX94 and the Nintendont contributors](https://github.com/FIX94/Nintendont):** original Nintendont project, architecture, features and development.
- **[revvv](https://github.com/revvv/Nintendont-XBOX360):** original wired Xbox 360 controller implementation that forms the foundation of this fork.
- **[Nekkidso](https://github.com/Nekkidso):** fork maintenance, development coordination, code integration, real Wii hardware testing, bug reproduction, validation and releases.
- **ChatGPT — GPT-6 (OpenAI):** AI-assisted code analysis, debugging, USB/HID investigation, development guidance and documentation for this release. GPT-6 is the model used to prepare this documentation; earlier assistance may have used other model versions.

This project does not claim ownership of the original Nintendont software. Hardware testing and release decisions are carried out by the project maintainer.

---

## Original Nintendont documentation

The following documentation is preserved **unchanged** from this repository's existing README. It covers Nintendont's general features, installation, build requirements and notes. For current upstream information, also consult [FIX94/Nintendont](https://github.com/FIX94/Nintendont).

### Nintendont
A Wii Homebrew Project to play GC Games on Wii and vWii on Wii U

### Features:
* Works on Wii and Wii U (in vWii mode)
* Full-speed loading from a USB device or an SD card.
* Loads 1:1 and shrunken .GCM/.ISO disc images.
* Loads games as extracted files (FST format)
* Loads CISO-format disc images. (uLoader CISO format)
* Memory card emulation
* Play audio via disc audio streaming
* Bluetooth controller support (Classic Controller (Pro), Wii U Pro Controller)
* HID controller support via USB
* Custom button layout when using HID controllers
* Cheat code support
* Changeable configuration of various settings
* Reset/Power off via button combo (R + Z + Start) (R + Z + B + D-Pad Down)
* Advanced video mode patching, force progressive and force 16:9 widescreen
* Auto boot from loader
* Disc switching
* Use the official Nintendo GameCube controller adapter
* BBA Emulation (see [BBA Emulation Readme](BBA_Readme.md))

### Features: (Wii only)
* Play retail discs
* Play backups from writable DVD media (Old Wii only)
* Use real memory cards
* GBA-Link cable
* WiiRd
* Allow use of the Nintendo GameCube Microphone

### What Nintendont will never support:
* Game Boy Player

### Quick Installation:
1. Get the [loader.dol](loader/loader.dol?raw=true), rename it to boot.dol and put it in /apps/Nintendont/ along with the files [meta.xml](nintendont/meta.xml?raw=true) and [icon.png](nintendont/icon.png?raw=true).
2. Copy your GameCube games to the /games/ directory. Subdirectories are optional for 1-disc games in ISO/GCM and CISO format.
   * For 2-disc games, you should create a subdirectory /games/MYGAME/ (where MYGAME can be anything), then name disc 1 as "game.iso" and disc 2 as "disc2.iso".
   * For extracted FST, the FST must be located in a subdirectory, e.g. /games/FSTgame/sys/boot.bin .
3. Connect your storage device to your Wii or Wii U and start The Homebrew Channel.
4. Select Nintendont.

### Compiling:
For compile Nintendont yourself, get the following versions of the toolchain compiling PPC tools:
* **devkitARM r53-1**
* **devkitPPC r35-2**
* **libOGC 1.8.23-1**

These versions can be downloaded here: https://www.mediafire.com/folder/j0juqb5vvd6z5/devkitPro_archives

On Windows, run the "Build.bat" batch script for build Nintendont.

On Unix, run the "Build.sh" script.

Please use these specific versions for compiling Nintendont, **because if you try to compile them on latest dkARM/dkPPC/libOGC, you'll get a lot of compiler warnings and your build will crash when attemping to return to Nintendont menu**, so be warned about that.

### Notes
* The Wii and Wii U SD card slot is known to be slow. If you're using an SD card and are having performance issues, consider either using a USB SD reader or a USB hard drive.
* USB flash drives are known to be problematic.
* Nintendont runs best with storage devices formatted with 32 KB clusters. (Use either FAT32 or exFAT.)
