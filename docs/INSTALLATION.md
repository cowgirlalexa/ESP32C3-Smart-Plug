# Installation and updating

## Prerequisites

ESPHome **2026.11.0** for the reference v1.0 configuration; ESP32-C3 plug with the documented Athom V3 peripheral wiring; compatible existing flash/partition layout; secure local Wi-Fi; an ESPHome Device Builder/CLI host.

## Configure

1. Copy `plug-us_v1.yaml` to your ESPHome configuration directory.
2. Copy `secrets.example.yaml` to `secrets.yaml` locally and replace every placeholder. Never upload `secrets.yaml` to GitHub.
3. Set a distinct `substitutions.name` and `friendly_name` for each plug; keep names valid ESPHome hostnames.
4. Verify `current_limit`, `relay_restore_mode`, and the GPIO mapping against the physical hardware.
5. If a pre-existing device uses a **different API key**, preserve that actual key in your local `secrets.yaml` or follow a supported key rotation procedure before OTA.

## Validate and compile

```bash
esphome config plug-us_v1.yaml
esphome compile plug-us_v1.yaml
```

Inspect `firmware.bin` under `.esphome/build/<node>/.pioenvs/<node>/`. For the standard ESPHome 4 MB dual-OTA layout with 0x1C0000-byte application slots, a firmware image must fit within **1,835,008 bytes**, and leave margin for future upgrades. Review the compile output instead of assuming the previous version's binary size applies.

## Flashing

**Existing ESPHome on the correct dual-OTA layout:** use ESPHome Device Builder/CLI for OTA or upload the generated OTA image via the local web dashboard. The local web updater is enabled through `ota: - platform: web_server` plus `web_server:`. Native OTA uses `ota: - platform: esphome` and API-key-derived encryption.

**Fresh hardware or Tasmota with a different partition scheme:** do **not** assume an OTA binary includes the correct bootloader/partitions. Follow hardware-specific installation instructions, serial recovery or [partition migration](RECOVERY.md).

Do not flash arbitrary Athom-hosted stock firmware over this customized installation: it may replace the features and potentially be incompatible with the modified partition map.

## Updating after v1.0

Back up your local YAML and secrets; validate with the target ESPHome version; compile; ensure the output fits; deploy to one test plug first; verify relay, current monitoring, Bluetooth controls, web OTA, native OTA and API before rolling out broadly.

## Security

Do not publish actual Wi-Fi credentials, web UI passwords, API encryption keys, or an OTA credential. Put management interfaces on a trusted IoT network; use a unique strong web dashboard password. The plug contains a captive-portal fallback AP. Protect access to the device itself.
