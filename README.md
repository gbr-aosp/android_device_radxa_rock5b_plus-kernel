AOSP 16 device kernel files for Radxa Rock 5B+ (RK3588).

Ported from the Orange Pi 5 Pro project. Supports booting from SD card, eMMC, NVMe, and USB.

---

Boot device configuration (config.txt)
---------------------------------------

NOTE: Rock 5B+ MMC numbering is the OPPOSITE of Orange Pi 5 Pro.
Verified against the board's SPI U-Boot environment (rkimg_bootdev variable).

For SD card -
boot_device=mmc
boot_devnum=1

For eMMC -
boot_device=mmc
boot_devnum=0

For NVMe (first M.2 slot) -
boot_device=nvme
boot_devnum=0

For NVMe (second M.2 slot) -
boot_device=nvme
boot_devnum=1

For USB -
boot_device=usb
boot_devnum=0

The SPI U-Boot on Rock 5B+ already tries devices in this order:
  usb0 -> mmc1 (SD) -> nvme0 -> nvme1 -> mmc0 (eMMC) -> ...
config.txt only controls where Android looks for its root filesystem,
not the SPI bootloader's device scan order.

By default, the selected boot device is SD card (boot_devnum=1).

---

Other settings (config.txt)
-----------------------------

fdtfile - Device Tree Blob filename. For Rock 5B+: rk3588-rock-5b-plus.dtb
fdtoverlay - Set to android-sdcard.dtbo when booting from SD card.
recovery - Set to true to boot into TWRP recovery instead of Android.

Note: TWRP recovery boots correctly but may show a blank screen on first boot.
Fix: unplug and reconnect the HDMI cable.

---

Not working -

Camera
3.5mm audio

Working -

Everything else including Vulkan.

---

This project can be ported to any device using a Rockchip SoC with minor changes.
The key difference between boards is the mmc/nvme device numbering — always verify
against the board's actual U-Boot environment with:
  sudo grep -ab "boot_devnum\|rkimg_bootdev" /dev/mtd0
