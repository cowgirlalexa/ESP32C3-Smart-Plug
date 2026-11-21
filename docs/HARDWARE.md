# Supported hardware and GPIO assignment

## v1.0 hardware profile

ESP32-C3 with **Athom Plug V3-compatible external hardware**, 4 MB flash, and the following observed wiring. ESP32-C3 devices with different peripherals or pinouts are not automatically supported.

| Function | Pin/interface | Notes |
|---|---|---|
| Relay | GPIO5 | Outlet switching, configured as ESPHome GPIO switch |
| Physical button | GPIO3 | Input pull-up, inverted; short press toggles relay unless child lock enabled |
| Status LED | GPIO6 | Inverted output |
| CSE7766 energy monitor | GPIO20 RX UART | 4800 baud, 8 data bits, EVEN parity, 1 stop bit |
| BLE | On-chip ESP32-C3 | Shares MCU resources with Wi-Fi |
| Wi-Fi | On-chip ESP32-C3 | Device name and network are user-configured |

## Electrical limitations

The sample YAML sets `current_limit: "16"` and `relay_restore_mode: RESTORE_DEFAULT_ON`. These are **configuration defaults, not universal hardware guarantees**. Change them to match the actual plug's labeled maximum current and your preferred fail-safe relay state. The over-current action depends on firmware, metering accuracy and relay operation; it is not certified over-current protection.

## Compatibility checklist

Check MCU model/revision, flash size, relay control pin and polarity, button GPIO/polarity, status LED polarity, CSE7766 UART wiring, mains model and current rating, and existing flash partition layout. Do not attempt to operate an unknown plug with this profile until those match.
