# Customization Guide

This project is designed as a portable starting point. Each user will need to customize Home Assistant entity IDs, Apex behavior, alert names, touchscreen labels, and optional Fusion equipment logic for their own system.

## 1. Home Assistant Entity IDs

The public ESPHome example uses these default entities:

```text
sensor.apex_feed_status
sensor.fusion_feed_15_remaining_seconds
timer.fusion_feed_15
sensor.apex_cyd_alerts

script.fusion_15_start
script.fusion_15_cancel

rest_command.apex_feed_a
rest_command.apex_feed_b
rest_command.apex_feed_c
rest_command.apex_feed_d
rest_command.apex_feed_cancel
```

If your Home Assistant entity IDs differ, update them in:

```text
esphome/tank-touch.example.yaml
```

Search for:

```text
entity_id:
action:
```

and replace only the example IDs with your own.

## 2. Apex Feed Modes

The default mapping is:

| Touchscreen Button | Apex Feed Number |
|---|---:|
| Apex A | `1` |
| Apex B | `2` |
| Apex C | `3` |
| Apex D | `4` |

The feed REST commands are in:

```text
home-assistant/apex-rest.example.yaml
```

Before using the touchscreen, test each command directly from Home Assistant and verify that the intended Apex outlets respond.

## 3. Fusion Feed Sequence

The optional Fusion-style feed mode is defined in:

```text
home-assistant/fusion-feed.example.yaml
```

The public example uses:

```text
Start:
Return OFF
UV OFF
Skimmer OFF
Start 15-minute timer

Finish or Cancel:
Return ON
UV ON
Wait 1 minute
Skimmer ON
```

Replace these example entities with your own:

```yaml
switch.return_pump
switch.uv_lamp
switch.skimmer
```

### Important Restore Limitation

The base example explicitly turns those devices on during restoration.

It does not preserve arbitrary states from before feed mode started.

For example, if your skimmer was intentionally off before Fusion Feed started, the default restore sequence may turn it on.

Users who need true state preservation should build a scene snapshot or other state-tracking method before relying on Fusion restore behavior.

## 4. Fusion Duration

The public example uses a 15-minute timer:

```yaml
duration: "00:15:00"
```

To change it, update the duration in:

```text
home-assistant/fusion-feed.example.yaml
```

The timer name can remain `timer.fusion_feed_15`, even if you use another duration, but renaming it may make your configuration easier to understand.

If you rename it, also update:

```text
sensor.fusion_feed_15_remaining_seconds
timer.fusion_feed_15
```

everywhere they appear.

## 5. Alert and Warning Names

The CYD footer automatically recognizes Apex virtual outputs beginning with:

```text
ALRM_
WARN_
```

Examples:

```text
ALRM_Leak
ALRM_RtrnOff
WARN_TempHi
WARN_AlkLo
```

Friendly display labels are configured in:

```text
home-assistant/apex-alerts-and-countdowns.example.yaml
```

Example:

```jinja
'ALRM_Leak': 'LEAK DETECTED',
'WARN_TempHi': 'HIGH TEMPERATURE WARNING'
```

Add your own entries to the `labels` list as needed.

Unknown `ALRM_` and `WARN_` names still appear automatically using a cleaned-up version of the Apex outlet name.

## 6. Optional Informational Statuses

The public example includes two optional special statuses:

```text
TridentBusy
SOW8
```

They are mapped to:

```text
I:TRIDENT TESTING
B:SOW WAVEMAKER RUNNING
```

These are examples only.

Replace or remove them in:

```text
home-assistant/apex-alerts-and-countdowns.example.yaml
```

Do not describe a virtual-output schedule as direct device telemetry unless it actually comes from a supported live device-status entity.

## 7. Footer Colors and Behavior

Footer styling is defined in:

```text
esphome/tank-touch.example.yaml
```

The alert prefixes are:

| Prefix | Meaning | Display Behavior |
|---|---|---|
| `A:` | Alarm | Red blinking footer |
| `W:` | Warning | Amber blinking footer |
| `I:` | Informational status | Green solid footer |
| `B:` | Informational status | Blue solid footer |
| `NORMAL` | No active condition | Normal idle footer |

To change colors, look for the `color:` section in the ESPHome YAML.

To change footer timing, look for:

```yaml
- interval: 4s
```

This controls how often multiple active footer messages rotate.

## 8. Button Labels and Screen Title

The visible button labels and title are directly drawn in the display lambda.

Search the ESPHome file for text such as:

```text
FEED CONTROL
APEX A
APEX B
FUSION 15
FUSION CANCEL
APEX CANCEL
SELECT FEED ACTION
```

You can replace these with your own preferred wording.

Keep labels short enough to fit the existing button and footer areas.

## 9. Touchscreen Calibration

Touch calibration values are in:

```text
esphome/tank-touch.example.yaml
```

under:

```yaml
touchscreen:
  - platform: xpt2046
```

The public example includes calibration values that worked for the original board:

```yaml
x_min: 280
x_max: 3860
y_min: 340
y_max: 3860
```

Your CYD may need different values.

If touches register in the wrong place, use ESPHome logs to inspect the raw coordinates and adjust calibration carefully.

## 10. Device Name and Friendly Name

The public example uses:

```yaml
esphome:
  name: apex-cyd-feed-panel
  friendly_name: Apex Feed Panel
```

Change these before flashing if you want a different device name in ESPHome and Home Assistant.

Avoid using a name that reveals your address, household, Wi-Fi name, or other private detail if you plan to share screenshots or logs.
