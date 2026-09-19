# Perks Beyond 80

Perks Beyond 80 is an SKSE/CommonLibSSE NG plugin experiment for Skyrim Special Edition.
The goal is to extend a perk-tree setup that removes skill-level locks from perks, so late perks can remain reachable in playthroughs that continue beyond the vanilla level 80-ish progression wall.

This repository currently contains the cleaned Responsive Combat SKSE template with the project renamed for this mod. The gameplay implementation still needs to be designed and built.

When loaded successfully, the plugin prints this message to the in-game console after data load:

```text
[PerksBeyond80] Plugin loaded successfully.
```

## Requirements

- Visual Studio 2022 with the Desktop development with C++ workload
- CMake 3.21 or newer
- Ninja
- vcpkg
- SKSE for the Skyrim runtime you are targeting
- A perk-tree/load-order setup that removes perk skill-level restrictions

Set `VCPKG_ROOT` to your vcpkg checkout:

```text
VCPKG_ROOT=C:\path\to\vcpkg
```

## First-Time Setup

Open this folder in a CMake-aware editor such as Visual Studio, Visual Studio Code, or CLion.

The shared presets in `CMakePresets.json` define `debug` and `release` builds. If your editor cannot find Ninja or needs machine-specific paths, create a local `CMakeUserPresets.json`. This file is ignored by git.

Example:

```json
{
  "version": 3,
  "configurePresets": [
    {
      "name": "debug-local",
      "displayName": "Debug Local",
      "inherits": "debug",
      "cacheVariables": {
        "CMAKE_MAKE_PROGRAM": "C:/path/to/ninja.exe"
      }
    }
  ]
}
```

Configure and build from a Visual Studio developer shell:

```powershell
cmake --preset debug
cmake --build --preset debug
```

## Deploying The Plugin

By default, build output stays inside the local build directory.

To deploy directly to a Skyrim install, set `SKYRIM_FOLDER` to the folder that contains `SkyrimSE.exe`.

To deploy into a mod manager's mods folder, set `SKYRIM_MODS_FOLDER`. When this variable is set, the DLL is copied to:

```text
%SKYRIM_MODS_FOLDER%\PerksBeyond80\SKSE\Plugins\
```

## Repository Hygiene

Generated files and local editor settings should stay out of git. Do not commit `build/`, `cmake-build*/`, `vcpkg_installed/`, `.idea/`, `.vscode/`, or `CMakeUserPresets.json`.
