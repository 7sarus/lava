# lava

## Purpose
Project for the LAVA LXX518 Android device. Source repo: TBD (not yet linked).

## Target device
- LAVA LXX518, MT6878, Android 15 (SDK 35), arm64-v8a, bootloader locked
- Full specs: `docs/device-specs.md`
- Serial is not stored in the repo; get it live with `adb devices`

## Stack
TBD

## Build commands
TBD

## Conventions
- Commit locally; never `git push` without explicit user instruction.
- Stamp APK `versionName` with short commit hash and `versionCode` with commit count (if Android app).

## Verification status
Not started. Device connected via adb; no source pulled yet.

## Roadmap
1. Link GitHub repo and pull source.
2. Document stack and build commands here.
