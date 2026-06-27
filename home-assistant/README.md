# Home Assistant Configuration

This folder contains portable example configuration for the Home Assistant side of the project.

The CYD touchscreen does not contact the Neptune Apex directly. Home Assistant handles:

- Apex Feed A / B / C / D REST commands
- Apex Cancel command
- Apex feed-status polling and remaining-time data
- Apex outlet-status polling from `status.xml`
- Optional Fusion-style 15-minute feed sequence
- Feed timers and staged restore logic
- Jinja-based alert and status text generation
- The entities sent to the ESPHome touchscreen

## Included Files

| File | Purpose |
|---|---|
| `apex-rest.example.yaml` | Local REST commands for Apex Feed A/B/C/D and Cancel |
| `apex-status.example.yaml` | Live Apex feed status and remaining-seconds polling |
| `apex-outlet-status.example.yaml` | Apex `status.xml` outlet-state polling |
| `apex-alerts-and-countdowns.example.yaml` | Fusion countdown plus consolidated CYD alert/footer template |
| `fusion-feed.example.yaml` | Optional Home Assistant-native Fusion 15 feed package |
| `secrets.example.yaml` | Private values users must add locally and never commit |
| `README.md` | This setup guide |

## Recommended Installation Method

The cleanest method is to use Home Assistant packages.

In your main `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Then place copied and customized project files in:

```text
config/
├── configuration.yaml
├── secrets.yaml
└── packages/
    ├── apex_feed.yaml
    ├── apex_status.yaml
    ├── apex_alerts.yaml
    └── fusion_feed.yaml
```

You can also copy individual sections into your existing YAML configuration if you do not use packages.

> [!IMPORTANT]
> Do not place every example file into the same package unchanged without checking for duplicate top-level keys such as `rest:`, `template:`, `script:`, `timer:`, or `automation:`. Merge matching sections as needed for your Home Assistant configuration style.

## Required Private Secrets

Copy the relevant entries from:

```text
secrets.example.yaml
```

into your own private Home Assistant `secrets.yaml`.

Do not upload your real `secrets.yaml` to GitHub.

Typical secrets include:

```yaml
apex_feed_status_url: "http://192.168.1.100/rest/status/feed"
apex_status_xml_url: "http://192.168.1.100/cgi-bin/status.xml"
apex_username: "YOUR_APEX_USERNAME"
apex_password: "YOUR_APEX_PASSWORD"
```

## Configuration Order

Install and test the project in this order:

1. Add private Apex values to `secrets.yaml`.
2. Add and test `apex-rest.example.yaml`.
3. Add and test `apex-status.example.yaml`.
4. Add and test `apex-outlet-status.example.yaml`.
5. Add and test `apex-alerts-and-countdowns.example.yaml`.
6. Add and test `fusion-feed.example.yaml` if using the optional Fusion mode.
7. Add the ESPHome touchscreen configuration.
8. Test all feed, cancel, timer, restore, countdown, and footer behavior.

## Entity Dependencies

The public ESPHome example expects Home Assistant to provide:

```text
sensor.apex_feed_status
  ├── state: active feed number
  └── attribute: active remaining seconds

sensor.fusion_feed_15_remaining_seconds
timer.fusion_feed_15
sensor.apex_cyd_alerts
```

The Home Assistant example files in this folder create or support those entities.

## Important Safety Note

The included Fusion feed example explicitly turns equipment on during restoration:

```text
Return ON
UV ON
Wait 1 minute
Skimmer ON
```

It does not yet preserve arbitrary pre-feed equipment states.

For example, if a device was intentionally off before Fusion Feed started, the base example may turn it on during restore.

Test and customize this behavior before relying on it around livestock or critical equipment.
