# SpaceEngineers2-fix

[![Linux](https://img.shields.io/badge/Platform-Linux-blue)](#)
[![Shell Script](https://img.shields.io/badge/Language-Shell-informational)](#)
[![Proton](https://img.shields.io/badge/Proton-GE--Proton-orange)](#)
[![License](https://img.shields.io/badge/License-MIT-green)](./LICENSE)

Fix **Space Engineers 2** on Linux with Proton using an automated script that can:

- Apply Space Engineers 2 compatibility fixes
- Repair the Proton prefix with **.NET 9**
- Install or update to the latest **GE-Proton**

It can also be used as a standalone GE-Proton updater.

---

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Recommended Launch Options](#recommended-launch-options)
- [Manual Recovery / Verification](#manual-recovery--verification)
- [Troubleshooting](#troubleshooting)
- [Known Issues](#known-issues)
- [Safety Notes](#safety-notes)
- [Contributing](#contributing)

---

## Features

- **Distro-agnostic** workflow for Proton-based Space Engineers 2 fixes
- Auto-detects Steam paths (including custom library storage)
- Reads `libraryfolders.vdf` to locate game libraries
- Uses App ID-aware prefix targeting for safer compatdata operations
- Supports both interactive mode and CLI flags

---

## Prerequisites

Before running any fix command:

1. Install **Space Engineers 2** from Steam.
2. Ensure Steam has run the game at least once (creates initial compatdata/prefix structure).
3. Install `protontricks` if you plan to run manual recovery commands.

---

## Quick Start

Clone or download this repository, then run:

```bash
chmod +x ./SE2Repair
./SE2Repair
```

For direct CLI mode:

```bash
./SE2Repair --fix
```

Or install/update GE-Proton only:

```bash
./SE2Repair --eggroll
```

---

## Usage

```text
Usage: ./SE2Repair [OPTION]

If run without options, an interactive graphical menu will launch.

Options:
  --fix          Directly apply Space Engineers 2 compatibility fixes.
                 Resets UI/hardware cache and repairs .NET 9 environment.
  --eggroll      Directly download and install latest GE-Proton.
  -h, --help     Display this help documentation.
```

---

## Recommended Launch Options

Set this in Steam Launch Options for Space Engineers 2:

```bash
DISABLE_PRESSURE_VESSEL=1 PROTON_HIDE_NVIDIA_GPU=1 SDL_VIDEODRIVER=x11 %command% -nosplash -skipintro
```

---

## Manual Recovery / Verification

If you need to verify or manually recover the setup, use these steps.

1. Set Compatibility Tool to **GE-Proton10-32** (or latest available GE-Proton).
2. Disable Steam Overlay for the game.
3. If stuck in a "Minimum Requirements" loop, remove:

```text
[Your_Library_Path]/steamapps/compatdata/1133870/pfx/drive_c/users/steamuser/AppData/Roaming/SpaceEngineers2/Temp
```

4. Force Windows 10 mode for the game prefix:

```bash
protontricks 1133870 win10
```

5. Run .NET 9 runtime installer manually through Protontricks:

```bash
protontricks -c "wine [Your_Library_Path]/steamapps/common/SpaceEngineers2/redist/dotnet-runtime-9.0-latest.exe" 1133870
```

Example Steam library root:

```text
/media/steamuser/SteamLibrary
```

(With `steamapps` at `/media/steamuser/SteamLibrary/steamapps`.)

> Steam may have multiple library paths. Use the one that actually contains your Space Engineers 2 install.

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| Game fails startup with requirement loop | Corrupted cache/temp data | Delete Temp folder in compatdata path, rerun `--fix` |
| Script cannot find game path | Game in non-default Steam library | Ensure library is registered in Steam; rerun script |
| .NET repair appears unsuccessful | Installer did not run in correct prefix | Run manual protontricks command with correct library path |
| Black screen / UI issues | Proton/graphics environment mismatch | Apply launch options, use latest GE-Proton |
| GE-Proton not updated | Network/permissions/download failure | Rerun `--eggroll`, verify write access to compatibilitytools.d |

---

## Known Issues

- Steam library auto-discovery depends on valid `libraryfolders.vdf` entries.
- Some systems may require running the script after a full Steam restart.
- Desktop environments using Wayland may still behave better with the provided X11 launch override.
- If App ID or install paths change upstream, manual path verification may be required.

---

## Safety Notes

- The script is intended to target only Space Engineers 2-related Proton prefix data.
- Review script contents before running if your setup is highly customized.
- Back up critical compatdata directories if you maintain complex modded environments.

---

## Contributing

PRs and issue reports are welcome.

When reporting an issue, include:

- Distro and kernel version
- GPU + driver version
- Proton version used
- Full terminal output from script run
- Whether game is installed in default or custom Steam library path
