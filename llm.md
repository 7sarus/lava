# lava

## Purpose
Device work on a LAVA LXX518 (Agni 3 5G family) phone: stock ROM, bootloader unlock, KernelSU root. Repo holds notes and docs only. No source code yet.

## Target device
- LAVA LXX518, MediaTek MT6878 (Dimensity 7300), arm64-v8a, 8 GB RAM
- Full specs: `docs/device-specs.md`
- Serial is not stored in the repo; get it live with `adb devices`
- Installed build: `LAVA_LXX518_SW_V_V05_HW_V1.0_20251215` (Android 15, patch 2025-11-05)
- Kernel: `6.1.141-android14-11` (GKI branch matches KernelSU)

## Stack
- Flash: SP Flash Tool (MediaTek scatter flow), not fastboot ROMs
- Device tools: adb, fastboot (fastbootd and bootloader modes)
- Linux udev rules already cover 0e8d:201c (no MediaTek driver needed on Linux)

## Build commands
None. Repo is docs only.

## Conventions
- Commit locally; never `git push` without explicit user instruction.
- `.ai/` is gitignored (live state only).
- Firmware goes under `firmware/` (gitignore it before adding; files are 4 GB+).

## Verification status
- Bootloader UNLOCKED. Confirmed 2026-11-18: `ro.boot.vbmeta.device_state=unlocked`, `verifiedbootstate=orange`. User ran `fastboot oem unlock`; data was wiped.
- adb works in Android. USB debugging was re-enabled after the wipe.
- Boot partition unreadable without root (`dd` on `/dev/block/by-name/boot_a` returns Permission denied).
- OTA in progress or paused: Google Play system update to `LAVA_LXX518_SW_B_V08_HW_V1.0_20260901` (Android 16, patch 2026-08-05). Not confirmed installed.
- Local V12 flash file (`/mnt/data/...SW_U_V12...RMM(1).zip`, Android 14, 2024-11-06): version MISMATCH. Do not use its boot.img. Contains userdata.img and an "RMM" tag, treat as untrusted.
- Exact V08 file name not found publicly. Closest: filewale.com `..._SW_B_V08_HW_V1.0_20260901_Filewale.com.zip` (reseller copy, needs account).

## Roadmap
1. Confirm current build: `adb shell getprop ro.build.display.id`.
2. Get a stock ROM matching that build; place it in `firmware/`.
3. Extract `boot.img` from the ROM zip.
4. Patch `boot.img` with KernelSU (manager app or ksud).
5. Flash patched boot to the active slot from bootloader mode; keep stock boot as rollback.
