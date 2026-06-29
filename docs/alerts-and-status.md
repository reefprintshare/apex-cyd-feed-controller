# Alerts and Status Footer

The CYD receives one consolidated text sensor from Home Assistant:

```text
sensor.apex_cyd_alerts
```

That sensor combines Apex outlet states into short footer messages for the touchscreen.

The CYD does not query Apex directly. The data flow is:

```text
Apex local status.xml endpoint
        ↓
Home Assistant REST sensor
        ↓
Home Assistant Jinja template
        ↓
sensor.apex_cyd_alerts
        ↓
ESPHome CYD footer
```

## Footer Format

Each active item uses a prefix that tells the CYD how to display it.

```text
A:LEAK DETECTED|W:HIGH TEMPERATURE WARNING|I:TRIDENT TESTING|B:SOW WAVEMAKER RUNNING
```

| Prefix   | Meaning              | CYD behavior                                                |
| -------- | -------------------- | ----------------------------------------------------------- |
| `A:`     | Alarm                | Red blinking footer, priority locked                        |
| `W:`     | Warning              | Amber blinking footer, priority locked when no alarm exists |
| `I:`     | Informational status | Green solid footer                                          |
| `B:`     | Operational status   | Blue solid footer                                           |
| `NORMAL` | No active item       | Normal idle footer                                          |

## Priority Behavior

The display intentionally does not rotate critical conditions behind routine status messages.

1. If one or more `A:` alarms are active, the first alarm is shown and remains on screen.
2. If no alarm is active but one or more `W:` warnings are active, the first warning is shown and remains on screen.
3. Green `I:` and blue `B:` messages rotate every four seconds only when there is no active alarm or warning.
4. When there is more than one non-priority item, the footer shows an indicator such as `1/2` or `2/2`.

This keeps alarms and warnings visible instead of allowing them to disappear during normal status rotation.

## Included Alert Mappings

The public Home Assistant example automatically recognizes Apex virtual outlets that begin with:

```text
ALRM_
WARN_
```

Examples:

```text
ALRM_Leak
ALRM_TempHi
WARN_TempHi
WARN_pHLo
WARN_Po4Hi
```

Known outlets can use friendly display labels. Unknown `ALRM_` and `WARN_` names are converted into a readable fallback label automatically.

## Operational Status Examples

The example configuration also includes optional mappings for:

```text
WaterChange   → WATER CHANGE ACTIVE
OutageMode    → OUTAGE MODE ACTIVE
TridentBusy   → TRIDENT TESTING
SOW8          → SOW WAVEMAKER RUNNING
```

These are example outlet names only. Change or remove them to match your own Apex setup.

The `TridentBusy` example is based on a virtual output or schedule. It is not confirmed direct live Trident module telemetry.

## Manual Return Pump Warning

The example can show:

```text
RETURN PUMP MANUALLY OFF
```

when the configured return-pump outlet reports literal Apex state:

```text
OFF
```

That is intentionally different from:

```text
AOF
```

`AOF` means Apex Auto Off, which is the programmed state. It does not trigger the manual-return warning.

Change `Return_Pump` in the example to your own return-pump outlet name, or remove that rule if you do not want this behavior.

## ATO Reservoir-Low Inputs

The current public template works from Apex outlet data in the `status.xml` outlet list.

An ATO reservoir-low input is not included because Apex inputs are not outlet records in that structure. Add input-based alerts only after exposing and verifying the appropriate Apex input state in Home Assistant.

## Required Home Assistant Files

Install these examples together:

```text
home-assistant/apex-outlet-status.example.yaml
home-assistant/apex-alerts-and-countdowns.example.yaml
```

The outlet-status example creates:

```text
sensor.apex_outlet_states
```

The alerts-and-countdowns example uses that sensor to create:

```text
sensor.apex_cyd_alerts
```

## Customizing Your Footer

You can customize:

* Which Apex virtual outlets become alarms or warnings
* Friendly labels for existing outlets
* Optional operational-status mappings
* Whether a condition should be green `I:` or blue `B:`
* Whether a specific equipment outlet should be treated as a warning or alarm
* Footer wording and capitalization

Keep footer text short. The CYD has limited width, and short labels are easier to read at the tank.
