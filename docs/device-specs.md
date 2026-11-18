# LAVA LXX518 device specs

Captured via adb on 2026-11-18. Serial, IMEI and MAC are intentionally omitted.

## Identity
| Field | Value |
| :--- | :--- |
| Brand / model | LAVA / LXX518 (Agni 3 5G family) |
| Device codename | LXX518 |
| Form factor | Phone (telephony, 5G-capable model line) |
| Display | 1200 x 2652, 520 dpi, 60/120 Hz, about 6.8 in |
| Locale / timezone | en-IN / Asia/Calcutta |

## Hardware
| Field | Value |
| :--- | :--- |
| SoC | MediaTek MT6878 (Dimensity 7300 family) |
| CPU | 8 cores: 4x Cortex-A78 (part 0xd41) + 4x Cortex-A55 (part 0xd05) |
| ABI | arm64-v8a |
| RAM | 8 GB LPDDR4 (7.2 GiB visible to Android) |
| Storage | UFS, 108 GB user data (96 GB free at capture) |
| GPU | Mali (Vulkan), EGL "meow" |
| Battery | Li-ion, 100% at capture, 4452 mV |
| Sensors / radios | Accelerometer, camera (front + rear, flash, full level), fingerprint, Bluetooth LE, telephony + IMS |

## Software
| Field | Value |
| :--- | :--- |
| Android | 15 (SDK 35), security patch 2025-11-05 |
| Installed build | `LAVA_LXX518_SW_V_V05_HW_V1.0_20251215` |
| Build type / tags | user / release-keys |
| Kernel | Linux 6.1.141 (android14 kernel branch) |
| Google services | gmsversion 15_202507 |
| Partitions | Dynamic, A/B slot `_a`, AVB 1.2 |

## Security / bootloader
| Field | Value |
| :--- | :--- |
| Bootloader | Locked (`ro.boot.flash.locked=1`) |
| Verified boot | Green, enforcing |
| Boot reason | PowerKey |

## Newer firmware available
Latest listed build is `SW_B_V08` (2026-09-01). Installed build is `SW_V_V05` (2025-12-15), so the device is several updates behind.

Sources: filewale.com (V08 listing), firmwarefile.com (V06 listing), needrom.com (Agni 3 LXX518 Android 15 listing).
