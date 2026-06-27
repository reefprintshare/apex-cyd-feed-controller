# Troubleshooting Guide

This guide covers common problems with the Apex, Home Assistant, ESPHome, and CYD touchscreen portions of the project.

## Start With the Layer That Failed

The system has four layers:

```text
Neptune Apex
        ↓
Home Assistant
        ↓
ESPHome
        ↓
CYD touchscreen
```

When something does not work, test each layer separately instead of troubleshooting everything at once.

For example:

1. Confirm the Apex command works from Home Assistant.
2. Confirm the Home Assistant entity changes as expected.
3. Confirm ESPHome receives that entity.
4. Confirm the CYD display or touchscreen action behaves correctly.

## Apex Feed Button Does Nothing

Check the Home Assistant REST command first.

Open Home Assistant Developer Tools and run the relevant command:

```text
rest_command.apex_feed_a
rest_command.apex_feed_b
rest_command.apex_feed_c
rest_command.apex_feed_d
rest_command.apex_feed_cancel
```

If it does not work from Home Assistant, the touchscreen is not the problem.

Check:

- Apex local IP or hostname
- Apex credentials in `secrets.yaml`
- Home Assistant network access to the Apex
- The REST endpoint path
- The feed number sent in the payload
- Whether the intended Apex outlets are programmed to react to that feed mode

## Apex Feed Countdown Is Wrong or Does Not Appear

The touchscreen depends on:

```text
sensor.apex_feed_status
```

It expects:

```text
State:
0 = no feed active
1 = Feed A
2 = Feed B
3 = Feed C
4 = Feed D

Attribute:
active = remaining seconds
```

Check the sensor in Home Assistant Developer Tools.

If the state or `active` attribute is missing:

- Verify `apex-status.example.yaml` is loaded.
- Verify `apex_feed_status_url` is correct in `secrets.yaml`.
- Confirm the Apex response has a `feed` object.
- Confirm Home Assistant can authenticate to the endpoint.
- Check the REST sensor logs for errors.

The CYD counts down locally every second between Home Assistant polls, then resynchronizes when Home Assistant receives fresh Apex data.

## Fusion Button Does Not Switch to Fusion Cancel

The touchscreen changes the button based on:

```text
timer.fusion_feed_15
```

The timer must change to:

```text
active
```

when the Fusion start script runs.

Check:

- `script.fusion_15_start` exists.
- The script successfully starts `timer.fusion_feed_15`.
- The ESPHome YAML uses the correct timer entity ID.
- The timer state is visible in Home Assistant Developer Tools.
- ESPHome logs show the timer state changing.

## Fusion Feed Does Not Restore Equipment Correctly

The public example intentionally restores equipment in this order:

```text
Return ON
UV ON
Wait 1 minute
Skimmer ON
```

Check:

- The switch entity IDs match your own equipment.
- The restore script is enabled and loaded.
- The timer-finished automation is enabled.
- The cancel script stops any currently waiting restore script before restarting it.
- The equipment is not blocked by another automation, interlock, safety condition, or manual override.

> [!IMPORTANT]
> The base Fusion example is not a state snapshot/restore system. It explicitly turns the listed equipment on during restoration.

## Fusion Countdown Does Not Update

The CYD expects:

```text
sensor.fusion_feed_15_remaining_seconds
```

Check:

- The timer entity is named correctly.
- The template sensor is loaded.
- The template sensor updates every second.
- The timer has a valid `finishes_at` attribute while active.
- ESPHome imports the correct sensor entity ID.

When the timer is inactive, the touchscreen should show:

```text
FUSION READY
```

## Alert Footer Does Not Appear

The footer depends on:

```text
sensor.apex_cyd_alerts
```

Expected examples:

```text
NORMAL
A:LEAK DETECTED
W:HIGH TEMPERATURE WARNING
I:TRIDENT TESTING
B:SOW WAVEMAKER RUNNING
```

Check:

- `sensor.apex_outlet_states` is updating.
- Its `outlet` attribute contains the Apex outlet list.
- The Jinja template is loaded without errors.
- The outlet names match your alias or special-status mappings.
- The outlet state is `AON` or `ON`.
- ESPHome imports `sensor.apex_cyd_alerts` as a text sensor.

## Alert Footer Shows the Wrong Text

Check the friendly label map in:

```text
home-assistant/apex-alerts-and-countdowns.example.yaml
```

For example:

```jinja
'ALRM_Leak': 'LEAK DETECTED',
'WARN_TempHi': 'HIGH TEMPERATURE WARNING'
```

Unknown `ALRM_` and `WARN_` names fall back to a cleaned-up version of the Apex outlet name.

If a virtual output should not appear, remove or rename it so it does not begin with:

```text
ALRM_
WARN_
```

## CYD Cannot Connect to Home Assistant

Check:

- Wi-Fi SSID and password in private ESPHome `secrets.yaml`
- ESPHome API encryption key
- Home Assistant and CYD network/VLAN access
- Firewall rules
- CYD DHCP lease or IP address
- ESPHome logs
- Whether the device is online in the ESPHome dashboard

The CYD should use encrypted ESPHome API communication.

## CYD Touches Trigger the Wrong Button

Touch calibration varies between CYD boards.

Check the values under:

```yaml
touchscreen:
  - platform: xpt2046
```

The public example includes working values from the original build, but your board may need adjustment.

Use ESPHome logs to inspect raw touch coordinates. The project logs values like:

```text
Touch raw=x,y mapped=x,y
```

Adjust the calibration ranges carefully, flash the update, and test each button.

## CYD Display Is Blank or Dim

Check:

- USB power supply quality
- USB cable
- Backlight configuration
- Display SPI pins
- Display driver model
- ESPHome compile logs
- Board type
- Whether the display dimensions and rotation match your CYD

The public project is built for an ESP32-2432S028 with a 320 × 240 landscape interface.

## Home Assistant YAML Does Not Load

Check Home Assistant’s configuration validation before restarting.

Common causes:

- Incorrect indentation
- Duplicate top-level keys after manually combining example files
- Package include path errors
- Duplicate entity IDs
- Missing secrets
- Incorrect Jinja syntax
- Copying a full package file into a YAML section where only one subsection belongs

If you do not use packages, merge matching top-level sections such as:

```text
rest:
template:
script:
timer:
automation:
```

instead of repeating them.

## ESPHome YAML Does Not Compile

Check:

- Indentation
- Secret names in `secrets.yaml`
- Home Assistant entity IDs
- YAML quoting around lambda conditions
- ESPHome component compatibility
- Whether the CYD board pin mapping matches your hardware revision

Use ESPHome validation and logs before flashing.

## Recovery After a Restart

After a Home Assistant, Apex, Wi-Fi, or CYD restart, verify:

1. The CYD reconnects to Home Assistant.
2. Date and time return.
3. Feed status refreshes from Apex.
4. Fusion timer state updates.
5. Alert footer updates.
6. Touchscreen buttons still call the correct Home Assistant actions.

Always test restart behavior before relying on the controller for routine tank operation.
