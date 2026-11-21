# OTA partition recovery and Tasmota migration

## Why this matters

A Tasmota Safeboot partition table differs from ESPHome's standard dual-OTA layout. An ESPHome application can boot under the former but report `OTA updates are impossible` when a suitable alternate app partition is absent.

## Before doing anything

Back up the YAML and credentials; inspect logs for running partition address, used firmware size, current partition table and `Partition access allowed`. Do not apply a migration binary to a different flash capacity/layout without verifying suitability.

## Recovery path used on a 4 MB ESP32-C3 Athom V3 profile

The reference device had ESPHome 2026.11.0 running at `0xE0000` with a 1,675,264-byte image and partition access allowed. Its firmware exceeded the smaller migration methods' limits, so the **official Method C** route was used:

1. Install the correct ESPHome-provided **Method C intermediate partition table** with `esphome upload --partition-table --file partitions_tasmota_migration_c.bin <device.yaml>`.
2. Reboot and confirm network/API accessibility.
3. Install a **transitional small ESPHome image** that fits the official Method C limit (851,968 bytes). Preserve Wi-Fi/API/OTA partition access and keys.
4. Install the official standard dual-OTA table using `esphome upload --partition-table --file partitions_esphome.bin <device.yaml>`.
5. Restore full firmware via OTA; verify application partitions at `0x10000` and `0x1D0000`, each size `0x1C0000`.
6. Perform another normal OTA; confirm booting from the alternate slot. In the reference recovery, both native OTA and a subsequent web OTA were observed working.

**Do not treat these filenames as universal.** Only use verified official ESPHome migration artifacts for the appropriate flash layout and running image size; review the upstream instructions before running any partition-table write.

Official guide: https://esphome.io/guides/migrate_sonoff_tasmota/#esp32-partition-table-migration

## When OTA is not possible

If both application and recovery paths are inaccessible, use the ESP32-C3 ROM serial flashing mode with the correct complete firmware/bootloader/partition files, using proper electrical isolation and expertise. Never attach a USB-to-serial programmer while a mains plug is energized.
