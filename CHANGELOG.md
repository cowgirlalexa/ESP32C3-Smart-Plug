# Changelog

## v1.0 — 2026-11-21

Initial public source release: **Custom Firmware for ESP32-C3 Based Smart Plugs**, targeting Athom V3-compatible external hardware and GPIO wiring.

- Local relay management, physical-button child lock and power-on preference
- CSE7766 electricity measurements, daily/total energy, configurable software current cutoff
- Encrypted native ESPHome OTA, web dashboard OTA and corrected dual-OTA compatibility
- BLE master enable, scanning preference and persistent next-boot proxy-forwarding control
- Vendor manifest HTTP updater and upstream Athom dashboard-import references omitted
- Secret placeholders moved to a local `secrets.yaml` convention; reproducible configuration and hardware/recovery docs

**Verification:** YAML structure and source-level review performed. A new clean ESPHome 2026.11.0 compilation and general-hardware validation are still required. Previous tests on a reference Athom V3-compatible plug demonstrated core functionality and both OTA update routes.
