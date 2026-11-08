# AERA Recovery device tree for OnePlus 13T / 13s (pagani)

Unofficial, untested bring-up for **OnePlus 13T / 13s** (`pagani`, model
CPH2723 / CPH2727 / PKX110, device `OP612BL1` / `OP60F5L1`), Snapdragon 8 Elite
(SM8750, `sun`), kernel 6.6.

Based on the official
[AERA OnePlus 13 tree (dodge)](https://github.com/AERA-Recovery/android_device_oneplus_dodge-AERA)
with pagani specifics from
[koaaN/android_device_oneplus_pagani-orangefox](https://github.com/koaaN/android_device_oneplus_pagani-orangefox).

## What changed from dodge

- Product renamed to `pagani`; super size `14956888064`; screen 1216x2640;
  CPU temperature zone `thermal_zone2`; assert devices
  `pagani,PKX110,CPH2723,CPH2727,OP60F5L1,OP612BL1`.
- Vendor/ODM blobs, trustlets (`vendor/firmware_mnt/image`), `msm_kgsl.ko`,
  vibrator effects (`odm/etc/vibrator/809`) and Wi-Fi config refreshed from an
  OxygenOS 15.0.2.601 (EX01) pagani device.
- Removed for the first bring-up: dodge audio stack (`--dodge-stock-agm`),
  mobile data (carried dodge-specific NV data), Bluetooth post-decrypt modules,
  ultrasonic fingerprint path (`AERA_SUPPORT_FINGERPRINT := 0`).

Touch: pagani uses a Synaptics S3910 HBP panel. Its kernel driver is loaded by
the stock `vendor_boot` recovery module list, and basic touch works without
`touchDaemon` (same as dodge).

## Build

```bash
mkdir -p ~/android/AERA_16.0 && cd ~/android/AERA_16.0
repo init -u https://github.com/AERA-Recovery/android_manifest.git -b aera-16.0
repo sync -c --force-sync --no-clone-bundle --no-tags -j$(nproc)
git clone -b aera-16.0 <this repo> device/oneplus/pagani
source build/envsetup.sh
lunch twrp_pagani-bp2a-eng
mka adbd recoveryimage
```

Or run the **Build AERA (pagani)** GitHub Actions workflow.

## Testing safely

The device has dedicated `recovery_a`/`recovery_b` partitions. Back up both
first, then flash only the recovery partition:

```bash
fastboot flash recovery_a recovery.img
fastboot reboot recovery
```

Restore with `fastboot flash recovery_a recovery_a-backup.img`.
