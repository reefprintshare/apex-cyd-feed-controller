# DIY Apex CYD Feed Controller & Status Display

A local, at-the-tank touchscreen controller and status display for Neptune Apex, Home Assistant, ESPHome, and the ESP32-2432S028 CYD.

This project turns a 2.8-inch ESP32 touchscreen into a dedicated reef-tank control panel for Apex feed modes, Home Assistant automations, live countdowns, and rotating Apex alarm/warning/status messages.

> **Local LAN only:** Normal operation does not depend on Apex Fusion or any cloud service.

## Features

- Apex Feed A / B / C / D touchscreen buttons
- Apex Feed Cancel button
- Optional Home Assistant-native 15-minute Fusion-style feed mode
- Fusion Cancel button with staged equipment restoration
- Live Apex feed countdown using Apex-reported remaining time
- Live Fusion feed countdown
- Date and time header sourced from Home Assistant
- Rotating Apex alarm, warning, and informational-status footer
- Support for multiple simultaneous active statuses
- Encrypted ESPHome API communication
- No Apex credentials stored on the touchscreen

## Architecture

```text
Neptune Apex local REST/XML endpoints
        ↓
Home Assistant REST integrations, scripts, automations, and Jinja templates
        ↓
Home Assistant entities and consolidated status text
        ↓
Encrypted ESPHome API
        ↓
ESP32-2432S028 CYD touchscreen
```

The CYD does not communicate directly with the Apex. Home Assistant is the bridge and logic layer.

## Screen Layout

```text
Date                 FEED CONTROL                 Time

APEX A                                      APEX B
APEX C                                      APEX D
FUSION 15 / FUSION CANCEL                   APEX CANCEL

Rotating alert/status footer or normal “SELECT FEED ACTION”
```

## Repository Layout

```text
.
├── docs/
│   ├── apex-setup.md
│   ├── alerts-and-status.md
│   ├── customization.md
│   ├── hardware.md
│   ├── installation.md
│   ├── privacy-checklist.md
│   └── troubleshooting.md
├── esphome/
│   ├── secrets.example.yaml
│   └── tank-touch.example.yaml
├── home-assistant/
│   ├── apex-alerts-and-countdowns.example.yaml
│   ├── apex-outlet-status.example.yaml
│   ├── apex-rest.example.yaml
│   ├── apex-status.example.yaml
│   ├── fusion-feed.example.yaml
│   └── secrets.example.yaml
├── images/
├── .gitignore
├── LICENSE
└── README.md
```

## Quick Start

1. Read the [hardware guide](docs/hardware.md).
2. Read the [installation guide](docs/installation.md).
3. Add your private values using the Home Assistant and ESPHome `secrets.example.yaml` files.
4. Install and test the Home Assistant examples.
5. Copy and customize `esphome/tank-touch.example.yaml`.
6. Flash the CYD through ESPHome.
7. Test every feed, cancel, timer, restore, and alert behavior before relying on it around livestock.

## Home Assistant Components

The Home Assistant examples provide:

| File | Purpose |
|---|---|
| `apex-rest.example.yaml` | Apex Feed A/B/C/D and Cancel REST commands |
| `apex-status.example.yaml` | Apex feed-state and remaining-seconds polling |
| `apex-outlet-status.example.yaml` | Apex `status.xml` outlet-state polling |
| `apex-alerts-and-countdowns.example.yaml` | Fusion countdown plus consolidated footer status text |
| `fusion-feed.example.yaml` | Optional Home Assistant-native staged Fusion feed sequence |
| `secrets.example.yaml` | Safe private-secret template |

See [Home Assistant configuration](home-assistant/README.md) for installation order and entity dependencies.

## ESPHome Components

The public CYD configuration is:

```text
esphome/tank-touch.example.yaml
```

It includes:

- CYD display and touch configuration
- Apex A/B/C/D and Cancel actions
- Dynamic Fusion Start / Fusion Cancel behavior
- Apex countdown smoothing between Home Assistant polls
- Fusion countdown display
- Date/time header
- Rotating alarm, warning, and informational footer
- Touch calibration values from the original build

See [ESPHome configuration](esphome/README.md) before flashing.

## Fusion Feed Behavior

The optional Fusion-style feed sequence does this:

```text
Start:
Return OFF
UV OFF
Skimmer OFF
Start 15-minute timer

Natural completion or Cancel:
Return ON
UV ON
Wait 1 minute
Skimmer ON
```

> [!IMPORTANT]
> The included restore behavior is staged and reliable, but it is **not** a true arbitrary state snapshot/restore system.
>
> If equipment was intentionally off before Fusion Feed started, the base example may turn it on during restoration. Test and customize this behavior for your own system.

## Alert Footer Format

Home Assistant sends the CYD a single pipe-separated text entity.

Example:

```text
A:LEAK DETECTED|W:HIGH TEMPERATURE WARNING|I:TRIDENT TESTING|B:SOW WAVEMAKER RUNNING
```

| Prefix | Meaning | Display Behavior |
|---|---|---|
| `A:` | Alarm | Red blinking footer |
| `W:` | Warning | Amber blinking footer |
| `I:` | Informational status | Green solid footer |
| `B:` | Informational status | Blue solid footer |
| `NORMAL` | No active condition | Normal idle footer |

See [Alerts and Status Footer](docs/alerts-and-status.md) for details.

## Security and Privacy

This repository intentionally does **not** include:

- Wi-Fi credentials
- ESPHome API encryption keys
- OTA passwords
- Apex IP addresses, hostnames, usernames, or passwords
- Home Assistant URLs or tokens
- Private network details
- Home Assistant backups, databases, or `.storage` files

Review the [privacy and security checklist](docs/privacy-checklist.md) before uploading YAML, logs, screenshots, or photos.

## Important Documentation

- [Hardware Overview](docs/hardware.md)
- [Installation Guide](docs/installation.md)
- [Neptune Apex Local API Setup](docs/apex-setup.md)
- [Alerts and Status Footer](docs/alerts-and-status.md)
- [Customization Guide](docs/customization.md)
- [Troubleshooting Guide](docs/troubleshooting.md)
- [Privacy and Security Checklist](docs/privacy-checklist.md)

## Disclaimer

This is an unofficial community project. It is not affiliated with or endorsed by Neptune Systems, Home Assistant, ESPHome, or any hardware manufacturer.

Always test feed behavior, restore logic, alert behavior, and restart recovery before relying on this project around livestock or life-support equipment.
