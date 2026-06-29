# Changelog

All notable public changes to this project are documented here.

## v0.2.0 — Status, Safety, and UI Polish

Major refinement of the CYD interface after the original feed-control build.

### New Safety and Status Behavior

- Added safety-first footer priority behavior:
  - Red `A:` alarms display first and remain locked on screen.
  - Amber `W:` warnings display next and remain locked on screen when no alarm exists.
  - Green `I:` and blue `B:` informational statuses rotate only when no alarm or warning is active.
- Added automatic recognition of Apex virtual outputs beginning with:
  - `ALRM_` for red alarm footers
  - `WARN_` for amber warning footers
- Added mapped operational statuses for:
  - Water Change active
  - Outage Mode active
  - Trident testing indicator
  - SOW wavemaker running
  - Return pump manually forced literal `OFF`
- Clarified that Apex `AOF` means Auto Off and does not trigger the manual-return warning.
- Left the ATO reservoir-low input out of this version because it is an Apex input rather than an outlet record in the current consolidated status sensor.

### Display and Control Improvements

- Added active Apex feed button styling.
- Added short pressed-state visual feedback for touchscreen buttons.
- Added matching color rims for Apex, Fusion, and Cancel controls.
- Added staged Fusion skimmer-restoration countdown display.
- Changed the background to near-black.
- Refined the orange header with highlight, shadow, and raised-panel styling.
- Added a quiet idle-footer panel for `SELECT FEED ACTION` and command confirmations.
- Added Wi-Fi signal-strength bars.
- Added separate Apex REST-data freshness and Home Assistant heartbeat freshness indicators.

### Night Dimming

- Full brightness from 10:00 AM through 10:59 PM.
- 20% brightness from 11:00 PM through 9:59 AM.
- First touch during dim mode wakes the screen without activating a button.
- Night wake remains active for 60 seconds.
- Active alarms and warnings force full brightness until the priority condition clears.

### Design Decisions

- Kept Apex and Fusion status permanently visible above the controls.
- Did not add redundant periodic footer rotation for Apex/Fusion state because it would add movement without useful new information.
- Preserved the existing staged Fusion restore behavior:
  - Return ON
  - UV ON
  - Wait one minute
  - Skimmer ON

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
