# lava

## Purpose
Device work on a LAVA LXX518 (Agni 3 5G, MT6878): stock partition dumps, bootloader unlock,
KernelSU root, and the Android device tree derived from the dumps.

Related repo: [device_lava_LXX518](https://github.com/7sarus/device_lava_LXX518) — AOSP device tree
built from these partitions.

## Target device
- LAVA LXX518, MediaTek MT6878 (Dimensity 7300), arm64-v8a, 8 GB RAM
- Specs: `docs/device-specs.md`
- Serial is never stored; get it live with `adb devices`
- Running build: `LAVA_LXX518_SW_B_V08_HW_V1.0_20260901_20260901-1850` (Android 16, patch 2026-08-05)
- Kernel: `6.1.172-android14-11-g5b6e3d242716-ab15726104` (GKI, KMI `android14-6.1`)
- Active slot: `_b` (was `_a` on the Android 15 V05 build; the B_V08 OTA switched slots)

## Stack
- Flash: SP Flash Tool / mtkclient over EDL (BROM), fastbootd for `boot` and `vbmeta`
- `fastboot flash init_boot` does NOT work: bootloader answers `unknown command`, fastbootd
  `Invalid partition`. Use EDL for `init_boot`.
- Device tools: adb, fastboot, mtkclient, ksud, avbtool, lpdump, unpack_bootimg

## Build commands
None — this repo is docs plus gitignored firmware. Device tree builds live in the separate
`device_lava_LXX518` repo.

## Conventions
- Commit locally; push only when the user says so.
- `.ai/` is gitignored (live state only).
- `firmware/` is gitignored (dumps are ~11 GB).
- Never write serial, IMEI or MAC into the tree.

## EDL notes
- Enter: power off, hold Vol Up + Vol Down, then plug USB.
- mtkclient must start while the phone is off the bus (`Please disconnect, start mtkclient and reconnect`).
- Working pair: `preloader_k6878v1_64_flip.bin` + `download_agent/DA_BR.bin` from the local V12 ROM folder.
- DA patches partial_protect and auth, so reads and writes both work.
- `/tmp/opencode/mtkrun.py` injects `--preloader`/`--loader` (mtkclient CLI ignores them);
  env `MTK_PRELOADER`, `MTK_DA`.

## Verification status
- Bootloader UNLOCKED: `ro.boot.flash.locked=0`, `verifiedbootstate=orange`. Data wiped at unlock.
- Full dump 2026-11-18: 77 partitions + `gpt.bin` + `gpt_backup.bin`, 11 GB in `firmware/dump_all/`,
  `SHA256SUMS.txt` checked, no read errors. `userdata` excluded on purpose.
- Boot chain dumped for both slots (`boot`, `init_boot`, `vendor_boot`, `vbmeta`) in `firmware/dump/`.
- KernelSU v3.3.1 (LKM, KMI `android14-6.1`) written to `init_boot_b` over EDL — write confirmed
  (`sector 430592, count 2048`). Manager APK installed. **Root not yet verified** (phone was left in
  preloader mode after the write).
- Patching `boot` does nothing on this device: it has no ramdisk, the LKM never loads, manager shows
  "Not installed". `init_boot` is the correct target.
- `firmware/patched/boot_a_ksu.img` (Android 15 slot-a kernel) is stale — do not flash.

## Rollback
- `firmware/dump_all/boot_b.img` and `init_boot_b.img` are the stock Android 16 images.
- `fastboot flash boot firmware/dump_all/boot_b.img` works from fastbootd;
  restoring `init_boot_b` requires EDL.

## Roadmap
1. Boot the phone, confirm KernelSU manager reports "Working".
2. Publish the device tree (done: `device_lava_LXX518`).
3. Fill the device tree gaps: proprietary blobs, sepolicy, kernel build config.
4. Optional: TWRP/OFox bring-up on top of the same fstab and partition data.

## Roadmap history
- Android 16 OTA `B_V08` installed itself mid-session; the V05 Android 15 dump remains valid only as
  an archive, not as the current boot chain.
