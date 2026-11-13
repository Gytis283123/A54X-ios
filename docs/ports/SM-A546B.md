# iOS port target: Samsung Galaxy A54 5G (SM-A546B)

This is the initial hardware record for a future `a54x` port, based on the
existing `ios-sweet` XNU bring-up. It is a planning record, not evidence that
the sweet boot image or drivers work on Samsung hardware.

## Confirmed target identity

| Property | Value | Evidence |
| --- | --- | --- |
| Model | Samsung Galaxy A54 5G, SM-A546B | [A54 device tree](https://github.com/ExynosEnclave/android_device_samsung_a54x) identifies SM-A546B |
| Device codename | `a54x` | Same device tree |
| SoC | Samsung Exynos 1380, `S5E8835` | [A54 TWRP device tree](https://github.com/Mesa-Labs-Archive/android_device_samsung_a54x) |
| CPU | 8 cores: 4 Cortex-A78 + 4 Cortex-A55 | Same device tree; exact MPIDRs and start method remain to be measured |
| GPU | Arm Mali-G68 MP5 | Same device tree |
| Display | 1080 × 2340 Super AMOLED, 120 Hz | Same device tree; Linux framebuffer/panel handoff is unknown |
| Storage | UFS 2.2, 128/256 GB variants | Same device tree; partition map and usable target are unknown |

## Bring-up status

- No A546B boot image has been built or booted from this workspace.
- No A546B stock DTB, live device tree, boot/recovery image, partition map, or
  serial log is present in the port repository.
- The sweet shim is Qualcomm/ABL-specific. Its address map, display handoff,
  PSCI conduit, GIC setup, UFS setup, and temporary boot procedure must not be
  reused until verified against A546B evidence.
- The A54 device tree and kernel source are useful leads, but Android device
  configuration alone does not establish the secure-boot or bootloader path
  required to enter a custom XNU shim.

## Next port milestone: read-only inventory

Before writing a boot path:

1. Confirm the exact handset model/region and whether its bootloader can be
   unlocked. Record the current firmware build and Android security state.
2. Collect stock `boot`, `vendor_boot`, `dtbo`, `vbmeta`, and recovery metadata
   plus a live DTB, keeping device-specific images and identifiers out of git.
3. Extract and record memory banks/reservations, CPU MPIDRs and PSCI method,
   GIC version and register ranges, timer frequency, console, framebuffer,
   storage controller and partition map from the stock DTB and read-only boot
   logs.
4. Identify a reversible RAM-only boot method for an unsigned experimental
   loader. Do not flash or repartition during this milestone.
5. Once the entry path and facts are known, create an A546B shim/ADT target
   with explicit board constants and a QEMU test profile before attempting a
   device boot.

Keep backups and one recovery path for the stock firmware. Do not treat any
partition as disposable until the user explicitly confirms a destructive
step for this specific handset.

## Sources

- [ExynosEnclave A54 device tree](https://github.com/ExynosEnclave/android_device_samsung_a54x)
- [Archived A54 TWRP device tree and hardware summary](https://github.com/Mesa-Labs-Archive/android_device_samsung_a54x)
- [Samsung A54 product specification](https://image-us.samsung.com/SamsungUS/samsungbusiness/pdf/spec-sheets/HHP-A545G-DATA-GENERIC-MAR23-FINAL.pdf)

