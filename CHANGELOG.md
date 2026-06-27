# Changelog

All notable public changes to this project are documented here.

## v0.1.0 — Initial Public Release

Initial community release of the DIY Apex CYD Feed Controller & Status Display.

### Included

- ESP32-2432S028 CYD touchscreen configuration example
- Apex Feed A / B / C / D touchscreen controls
- Apex Cancel control
- Optional Home Assistant-native Fusion-style 15-minute feed mode
- Fusion Cancel with staged equipment restoration
- Live Apex feed countdown using the local Apex feed-status endpoint
- Live Fusion feed countdown
- Date and time header sourced from Home Assistant
- Rotating alarm, warning, and informational status footer
- Multiple-alert rotation indicator
- Home Assistant examples for Apex REST commands, feed status, outlet status, alerts, countdowns, and Fusion behavior
- ESPHome and Home Assistant secrets templates
- Hardware, installation, customization, troubleshooting, privacy, and Apex API documentation
- Public touchscreen screenshots and feature walkthrough collage

### Important Limitations

- The Fusion restore example explicitly turns configured equipment on during restoration. It does not yet preserve arbitrary pre-feed equipment states.
- The optional `TridentBusy` status is a virtual-output or schedule-based indicator, not confirmed direct live Trident module telemetry.
- CYD touch calibration may need adjustment for individual boards.
- Users must test their own Apex REST behavior, feed modes, restore sequence, and safety logic before use around livestock or critical equipment.
