# Alerts and Status Footer

The CYD can show a rotating footer for Apex alarms, warnings, and informational statuses.

The footer is designed to give a quick at-the-tank view of important controller conditions without opening Apex Fusion or Home Assistant.

## Data Flow

```text
Apex local status endpoint
        ↓
Home Assistant REST sensor
        ↓
Home Assistant Jinja template
        ↓
One consolidated text sensor
        ↓
ESPHome CYD footer display
