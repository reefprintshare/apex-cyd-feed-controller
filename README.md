# DIY Apex CYD Feed Controller & Status Display

A local, at-the-tank touchscreen controller and status display for Neptune Apex, Home Assistant, ESPHome, and the ESP32-2432S028 CYD.

This project turns a 2.8-inch ESP32 touchscreen into a dedicated reef-tank control panel for Apex feed modes, Home Assistant automations, countdown timers, and live tank-status alerts.

> **Local LAN only:** Normal operation does not depend on Apex Fusion or any cloud service.

![Project status](https://img.shields.io/badge/status-in%20development-yellow)
![ESPHome](https://img.shields.io/badge/ESPHome-supported-blue)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-required-41BDF5)
![License](https://img.shields.io/badge/license-MIT-green)

## Features

* Apex Feed A / B / C / D touchscreen buttons
* Apex Feed Cancel button
* Separate Home Assistant automation feed button, such as a 15-minute “Fusion Feed”
* Live Apex feed countdown with active feed-mode display
* Live Home Assistant feed countdown
* Date and time header sourced from Home Assistant
* Rotating Apex alarm, warning, and informational status footer
* Support for multiple simultaneous alerts
* Local-only control on a trusted LAN
* No Apex credentials stored on the touchscreen

## Architecture

```text
Neptune Apex local REST/XML endpoints
        ↓
Home Assistant REST integrations + Jinja templates
        ↓
Home Assistant entities + consolidated status text
        ↓
Encrypted ESPHome API
        ↓
ESP32-2432S028 CYD touchscreen
```

The CYD does not communicate directly with the Apex. Home Assistant acts as the secure integration layer between the display and the controller.

## Hardware

* ESP32-2432S028 CYD touchscreen board
* USB power supply and USB cable
* Home Assistant instance
* ESPHome
* Neptune Apex with local network access
* Trusted local Wi-Fi/LAN shared by Home Assistant, the Apex, and the touchscreen

See [hardware documentation](docs/hardware.md) for more detail.

## Repository Layout

```text
.
├── docs/                 # Setup guides and customization notes
├── esphome/              # Public/sanitized ESPHome configuration
├── home-assistant/       # Public/sanitized HA examples
├── images/               # Project photos and screenshots
├── .gitignore
├── LICENSE
└── README.md
```

## Security and Privacy

This repository intentionally does **not** include:

* Wi-Fi names or passwords
* ESPHome API encryption keys
* OTA passwords
* Apex IP addresses, hostnames, usernames, or passwords
* Home Assistant URLs
* Personal Home Assistant entity IDs
* Personal alert names or aquarium-specific automation details
* Home Assistant backups, databases, or `.storage` files

Use local `secrets.yaml` files for all credentials and private network information.

## Project Status

The public configuration and documentation are being cleaned up for release.

Planned documentation includes:

* CYD hardware setup
* ESPHome installation and flashing
* Home Assistant REST/XML integration examples
* Apex Feed A/B/C/D control examples
* Countdown timer setup
* Consolidated alert/status banner setup
* Customization and troubleshooting notes

## Disclaimer

This is an unofficial community project. It is not affiliated with or endorsed by Neptune Systems, Home Assistant, or ESPHome.

Always test Apex commands and automations safely before relying on them around livestock or critical equipment.
