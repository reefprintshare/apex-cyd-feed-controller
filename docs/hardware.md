# Hardware Overview

This project uses an ESP32-2432S028 touchscreen board, commonly called the **CYD** or “Cheap Yellow Display,” as a local reef-tank control panel.

The base project requires no external relays, sensors, or wiring beyond USB power. The touchscreen communicates with Home Assistant over the local network, and Home Assistant handles all communication with the Neptune Apex.

## Required Hardware

| Item                  | Purpose                                                                   |
| --------------------- | ------------------------------------------------------------------------- |
| ESP32-2432S028 CYD    | 2.8-inch ESP32 touchscreen controller                                     |
| USB power supply      | Supplies stable 5 V power to the CYD                                      |
| USB cable             | Power and initial firmware flashing                                       |
| Home Assistant host   | Runs the Apex integration, automation logic, timers, and templates        |
| Neptune Apex          | Provides feed-mode control and outlet/status data through local endpoints |
| Trusted local network | Connects the CYD, Home Assistant, and Apex                                |

## CYD Display

The ESP32-2432S028 used in this project has:

* ESP32 microcontroller
* 2.8-inch 320 × 240 touchscreen
* Landscape-oriented display layout
* Integrated display and touch hardware
* Wi-Fi connectivity
* USB power and programming connection

The public example configuration is designed for a 320 × 240 landscape interface.

## Basic Physical Connection

```text
USB power supply
        ↓
ESP32-2432S028 CYD
        ↓ Wi-Fi / LAN
Home Assistant
        ↓ LAN
Neptune Apex
```

The basic build has no direct wiring between the CYD and the Apex.

The CYD also does not connect directly to Apex Fusion, cloud services, or the Apex REST/XML endpoints. Home Assistant is the bridge and logic layer.

## Recommended Power Setup

Use a reliable 5 V USB power supply and a quality cable.

For a permanent tank-side install, consider:

* A short right-angle USB cable for cleaner routing
* A USB supply connected to the same protected power source as the controller equipment
* A small UPS if you want the display to remain available during short power interruptions
* A 3D-printed enclosure, stand, or cabinet mount
* Strain relief for the USB cable

Avoid running the display from an overloaded USB hub or an unstable low-current USB port.

## Network Requirements

All three devices must be reachable on the same trusted local network:

```text
CYD ↔ Home Assistant ↔ Neptune Apex
```

Recommended network practices:

* Use a DHCP reservation for the CYD instead of relying on a changing address.
* Keep Home Assistant and the Apex on a stable local network.
* Use encrypted ESPHome API communication.
* Protect OTA updates with a password.
* Do not enable public internet access to the CYD, ESPHome dashboard, Home Assistant, or Apex web interface.
* Do not use port forwarding for Apex or ESPHome control.

## Display and Touch Pinout

The ESP32-2432S028 contains integrated display and touch hardware, but board revisions and public YAML examples can differ.

For that reason, this project does **not** list display or touch GPIO assignments here until they are confirmed against the public ESPHome example configuration.

When the public configuration is complete, the authoritative pin mapping will be the one contained in:

```text
esphome/tank-touch.example.yaml
```

Users should use that file as the source of truth for:

* Display driver configuration
* SPI pin assignments
* Backlight control
* Touch controller configuration
* Display rotation
* Font and color resources

## Optional Expansion Ideas

The stock project does not require external hardware, but the CYD can later be expanded with:

* RGB status LED
* Buzzer for critical alarms
* Physical emergency or maintenance button
* External temperature display
* Cabinet-door magnetic sensor
* Leak-sensor status light
* Small relay module for non-critical auxiliary devices

Any expansion should be documented separately and should not bypass Apex or Home Assistant safety logic.

## Installation Location

A good permanent mounting location is:

* Near the tank or controller cabinet
* Easy to reach during feeding or maintenance
* Away from salt spray and splashes
* Positioned so the display remains readable while standing at the aquarium
* Routed so the USB cable cannot be snagged or pulled

The CYD board is not waterproof. Use a suitable enclosure or mount if it will be near humidity, splash zones, or saltwater equipment.

## Safety Notes

This touchscreen is a convenience control interface, not a replacement for Apex safeguards or manual verification.

Before relying on it:

1. Test each feed button while watching the corresponding Apex behavior.
2. Confirm that the Apex Cancel action stops the correct feed mode.
3. Confirm the Home Assistant Fusion feed sequence turns equipment off in the intended order.
4. Confirm the Fusion restore sequence returns equipment in the intended staged order.
5. Test the touchscreen after Home Assistant restarts, Apex restarts, and a temporary Wi-Fi interruption.
6. Verify all critical life-support equipment has independent safety behavior outside this project.
