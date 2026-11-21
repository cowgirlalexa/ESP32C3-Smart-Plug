# Custom Firmware for ESP32-C3 Based Smart Plugs

**Release: v1.0** · **ESPHome 2026.11.0 reference** · **Hardware profile: Athom V3**

Community-maintained ESPHome firmware for ESP32-C3 smart plugs with **Athom Plug V3-compatible external hardware and GPIO wiring**. The aim is local-first control, energy monitoring, resilient OTA updates and independently managed Bluetooth functionality.

> **Compatibility is hardware-specific.** An ESP32-C3 alone does not imply compatibility. Confirm the relay pin, button, LED, metering IC, flash size and electrical rating before flashing. This project is not an official Athom product or firmware release.

## Features

- Relay control, selectable power-on behavior and child lock for the physical button
- CSE7766 voltage, current, power, energy, apparent/reactive power and power factor reporting
- Current-threshold relay shutdown (software protection; **not** a substitute for certified electrical protection)
- Home Assistant native API using an encryption key
- Two OTA paths: encrypted ESPHome Device Builder OTA and password-protected local web-server upload
- Bluetooth Proxy, BLE stack kill switch, scanning preference and a proxy-forwarding preference applied on next boot
- Fallback Wi-Fi access point, captive portal, diagnostics and local web dashboard
- No vendor-hosted firmware-update manifest and no upstream `dashboard_import` requirement

## Supported hardware

**Currently supported:** ESP32-C3 smart plugs matching the Athom Plug V3 external circuitry and GPIO assignments. See [hardware and pinout](docs/HARDWARE.md). Other designs, even with the same MCU, are **untested**.

## Quick start

1. Copy [`plug-us_v1.yaml`](plug-us_v1.yaml) into your ESPHome configuration directory.
2. Create a local `secrets.yaml` from [`secrets.example.yaml`](secrets.example.yaml) and supply your unique values. **Never commit secrets.yaml**.
3. Check GPIO compatibility, device's amp rating, selected relay startup mode, and the device name.
4. Run `esphome config plug-us_v1.yaml`, then `esphome compile plug-us_v1.yaml` with ESPHome **2026.11.0**.
5. Check `.esphome/build/plug-us/.pioenvs/plug-us/firmware.bin` size and the partition layout before flashing; see [installation guide](docs/INSTALLATION.md).
6. Install using the appropriate method for the **existing firmware and partition table**. Do **not** blindly push an OTA binary to a Tasmota Safeboot-layout device.

## Documentation

- [Installation and updating](docs/INSTALLATION.md)
- [Hardware profile and GPIO mapping](docs/HARDWARE.md)
- [Bluetooth control behavior](docs/BLUETOOTH.md)
- [OTA partition migration and recovery](docs/RECOVERY.md)
- [Release notes](CHANGELOG.md)

## Important limitations

The proxy-next-boot feature gates forwarded BLE advertisements and the proxy's active-connection capability; it **does not unregister the Bluetooth Proxy API or prevent all conceivable existing connection traffic**. The setting applies after reboot and should be field-tested with a BLE advertiser through this specific proxy. Bluetooth and scanning switches show requested (optimistic) states, not hardware telemetry.

The reference YAML is **structurally checked but not freshly compiled in this repository release workflow**. The previously tested customized plug was operational with ESPHome 2026.11.0; this portable v1.0 template requires a clean compile and on-device verification before being described as tested across more devices.

## Attribution

Built on [ESPHome](https://esphome.io/) and an Athom V3 hardware profile. This is independent community firmware. Device/hardware brand names identify compatibility only. No upstream vendor update endpoint is configured.

## Safety

Mains-powered equipment can be hazardous. Do not open or connect a USB-UART adapter to a mains-powered plug. Verify device ratings and local electrical requirements. Do not rely on the software current limit as the sole safety mechanism.
