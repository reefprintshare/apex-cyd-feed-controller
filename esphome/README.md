# ESPHome Configuration

This folder contains the public ESPHome portion of the project.

The CYD handles:

- Touchscreen layout and button presses
- Home Assistant service calls for Apex Feed A/B/C/D, Apex Cancel, and Fusion feed actions
- Date and time display
- Apex feed countdown display
- Fusion feed countdown display
- Dynamic Fusion Start / Fusion Cancel button behavior
- Rotating alert and status footer rendering
- Local countdown smoothing between Home Assistant updates

The CYD does not connect directly to the Neptune Apex.

```text
Neptune Apex local REST/XML endpoints
        ↓
Home Assistant REST integrations, scripts, automations, and Jinja templates
        ↓
Encrypted ESPHome API
        ↓
ESP32-2432S028 CYD touchscreen
```

## Included Files

| File | Purpose |
|---|---|
| `secrets.example.yaml` | Example private ESPHome secrets file |
| `README.md` | This guide |

The sanitized public touchscreen configuration will be added later as:

```text
tank-touch.example.yaml
```

## Before Using the Public YAML

Users will need to customize:

- Wi-Fi credentials through a private `secrets.yaml`
- ESPHome API encryption key
- OTA password
- Home Assistant entity IDs
- Home Assistant script IDs
- Home Assistant timer IDs
- Optional alert sensor entity ID
- Any display labels, colors, or button behavior they want to change

Do not put private credentials directly into the public YAML.

## Required Home Assistant Data

The touchscreen expects Home Assistant to provide the information it displays, including:

- Active Apex feed number
- Apex-reported remaining feed seconds
- Fusion timer state
- Fusion remaining seconds
- Date and time
- Consolidated alert/status text

The public Home Assistant examples are located in:

```text
../home-assistant/
```

## Security Notes

Use encrypted ESPHome API communication and password-protected OTA updates.

Do not enable a fallback access point or captive portal for a permanent aquarium installation unless you specifically understand and want that behavior.

Keep the CYD, Home Assistant, and Apex on a trusted local network. Do not expose the ESPHome device or dashboard directly to the internet.
