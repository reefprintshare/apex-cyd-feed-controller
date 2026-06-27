# Documentation

This folder contains the setup and reference guides for the DIY Apex CYD Feed Controller & Status Display.

## Available Guides

| File | Purpose |
|---|---|
| `hardware.md` | Hardware overview, power, network, mounting, and safety notes |
| `installation.md` | End-to-end installation and testing order |
| `alerts-and-status.md` | Rotating Apex alarm, warning, and informational footer behavior |

## Planned Guides

The following documentation will be added after the public ESPHome and Home Assistant examples are sanitized from the working project:

| Planned File | Purpose |
|---|---|
| `apex-setup.md` | Apex local REST/XML endpoint setup and testing |
| `customization.md` | Button labels, colors, entity mappings, aliases, and display behavior |
| `troubleshooting.md` | Common Apex, Home Assistant, ESPHome, Wi-Fi, and display issues |
| `privacy-checklist.md` | Pre-publish cleanup checklist for screenshots, YAML, and secrets |

## Important Project Design

The touchscreen does not connect directly to the Apex.

```text
Neptune Apex local REST/XML endpoints
        ↓
Home Assistant REST integrations, scripts, automations, and Jinja templates
        ↓
Home Assistant entities and consolidated status text
        ↓
Encrypted ESPHome API
        ↓
ESP32 CYD touchscreen
