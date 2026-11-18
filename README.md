# LAVA LXX518 (Agni 3 5G / MT6878)

Notes and artifacts from rooting and dumping a LAVA LXX518, plus the Android device tree derived
from its partitions.

| Repo | What it holds |
| :--- | :--- |
| **lava** (this repo) | Device notes, partition dump manifest, KernelSU flow, stock image inventory |
| [device_lava_LXX518](https://github.com/7sarus/device_lava_LXX518) | AOSP device tree built from the phone's own partitions |

Start with [`llm.md`](llm.md) for the current verified state, or
[`docs/device-specs.md`](docs/device-specs.md) for hardware details.

Partition dumps and patched images live in `firmware/` and are gitignored (11 GB). The tree contains
no serial, IMEI or MAC.
