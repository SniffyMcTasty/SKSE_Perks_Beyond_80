# Perks Beyond 80

Perks Beyond 80 is an SKSE/CommonLibSSE NG plugin experiment for Skyrim Special Edition.
It is intended as a gameplay supplement to [Removed Perk tree skill level limit](https://www.nexusmods.com/skyrimspecialedition/mods/80084), initially targeting its Ordinator setup. The goal is to remove remaining skill-level requirements on higher perk ranks, including requirements of skill level 80 and above, while preserving prerequisite perks and perk-point costs.

The initial SKSE plugin scaffold is in place. Gameplay functionality is planned but not yet implemented.

The original restriction-removal mod leaves skill-level requirements on higher ranks of multi-rank perks. This supplement is intended to extend that behavior for more flexible build experimentation; it does not introduce new perk trees or change character-level progression. See the [original mod description](https://www.nexusmods.com/skyrimspecialedition/mods/80084) for its existing behavior.

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

Planned gameplay integration targets are Removed Perk tree skill level limit and Ordinator with its matching patch. Exact supported versions will be established during implementation; they are not required just to compile or load the current plugin scaffold.

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

From a regular PowerShell terminal, the build script locates Visual Studio's C++ tools, CMake, and Ninja and configures an x64 build:

```powershell
.\scripts\build.ps1
.\scripts\build.ps1 -Configuration release
```

Install the C++ CMake tools component through Visual Studio Installer if CMake or Ninja is missing. The script uses `VCPKG_ROOT` and restores the terminal's environment when finished. No global PATH changes or local user preset are required for this workflow.

Alternatively, configure and build from a Visual Studio developer shell:

```powershell
cmake --preset debug
cmake --build --preset debug
```

The first configure step lets vcpkg download and build CommonLibSSE NG.

Successful compilation checks the native plugin and its dependencies. There are no automated tests yet; loading the DLL and checking gameplay behavior still requires Skyrim with SKSE.

## Deploying The Plugin

By default, build output stays inside the local build directory.

The PowerShell build script disables automatic deployment unless `-Deploy` is supplied:

```powershell
.\scripts\build.ps1 -Configuration release -Deploy
```

Direct CMake builds use the deployment environment variables whenever they are set.

To deploy directly to a Skyrim install, set `SKYRIM_FOLDER` to the folder that contains `SkyrimSE.exe`.

To deploy into a mod manager's mods folder, set `SKYRIM_MODS_FOLDER`. When this variable is set, the DLL is copied to:

```text
%SKYRIM_MODS_FOLDER%\PerksBeyond80\SKSE\Plugins\
```

## Repository Hygiene

Generated files and local editor settings should stay out of git. Do not commit `build/`, `cmake-build*/`, `vcpkg_installed/`, `.idea/`, `.vscode/`, or `CMakeUserPresets.json`.
