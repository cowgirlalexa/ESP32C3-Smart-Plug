# Bluetooth controls

Three Home Assistant switches are provided, each with a different purpose.

| Switch | When OFF | When ON |
|---|---|---|
| **Bluetooth Enabled** | Stops scanning and disables the BLE stack | Enables BLE; may resume scanning if scanning preference is ON and API is connected |
| **Bluetooth Scanning Enabled** | Stops scanning, BLE remains available | Allows scanning when BLE is enabled and an API client is connected |
| **Bluetooth Proxy on Next Boot** | After the **next reboot**, suppresses proxy advertisement forwarding and turns off active proxy connections capability | After next reboot, allows advertisement forwarding and active proxy connections capability |

The third switch is an **on-next-boot preference**. Changing it has no immediate forwarding effect. ESPHome stores preferences with `flash_write_interval: 5min`; allow the setting time to persist before restarting or losing power. The boot log is configured to print `proxy_gate` with the effective forwarding status.

The advertisement-filter hook is enabled through `USE_BLUETOOTH_PROXY_ADVERTISEMENT_FILTER`. This uses lower-level ESPHome interfaces that may change in later versions; pin/test the ESPHome release before upgrading.

**Limitations:** The component remains compiled and may still be presented as an adapter in Home Assistant. The setting does not guarantee already-established GATT connections are disconnected. The Bluetooth Enabled and Scanning Enabled switches are optimistic requested states; verify real scan state in logs.

## Suggested functional test

1. Set all three ON; reboot; verify `Bluetooth Proxy: Active: YES` and `Scanner State: RUNNING`.
2. Switch proxy-next-boot OFF, wait for persistence, then reboot. Keep BLE and scanning ON. Verify scanning is running and proxy active capability is NO.
3. Use a known BLE device and **inspect advertisement origin** to verify this plug does not forward while gated. Other proxies can mask this result.
4. With proxy OFF, turn scanning OFF and ON; ensure the BLE stack is not disabled.
5. With scanning preference ON, disable Bluetooth: scan must stop. Reenable Bluetooth: expected to resume only while API connected.
6. Restore proxy ON, persist and reboot, then confirm advertisements through this specific proxy resume.
