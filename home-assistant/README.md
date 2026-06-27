# Home Assistant Configuration

This folder contains portable example configuration for the Home Assistant side of the project.

The CYD touchscreen does not contact the Neptune Apex directly. Home Assistant handles:

- Apex Feed A / B / C / D REST commands
- Apex Cancel command
- Optional Fusion-style 15-minute feed sequence
- Feed timers and staged restore logic
- Apex feed-status polling
- Apex outlet-status polling
- Jinja-based alert and status text generation
- The entities sent to the ESPHome touchscreen

## Included Files

| File | Purpose |
|---|---|
| `apex-rest.example.yaml` | Example local REST commands for Apex Feed A/B/C/D and Cancel |
| `fusion-feed.example.yaml` | Optional Home Assistant-native Fusion 15 feed package |
| `secrets.example.yaml` | Private values users must add locally and never commit |
| `README.md` | This guide |

Additional example files for Apex status polling, countdown data, and the rotating alert footer will be added later.

## Recommended Installation Method

The cleanest method is to use a Home Assistant package.

For example, in your main `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named packagesgive m
