# Neptune Apex Local API Setup

This project uses the Neptune Apex **local** REST and XML endpoints through Home Assistant.

The touchscreen does not connect to the Apex directly. Home Assistant handles Apex authentication, REST commands, feed-state polling, outlet-state polling, and conversion of Apex data into simple entities that ESPHome can display.

## Local-Only Architecture

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
