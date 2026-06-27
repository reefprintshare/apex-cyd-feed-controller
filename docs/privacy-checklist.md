# Privacy and Security Cleanup Checklist

Use this checklist before committing any configuration, screenshot, photo, log, or example file to the public repository.

## Never Commit These

Do not upload or commit:

- Wi-Fi SSIDs
- Wi-Fi passwords
- ESPHome API encryption keys
- OTA passwords
- Apex usernames or passwords
- Apex IP addresses or local hostnames
- Home Assistant URLs or hostnames
- Home Assistant long-lived access tokens
- Webhook IDs or webhook URLs
- Private entity IDs that reveal personal device names
- Static DHCP reservations
- Home Assistant backups
- `.storage` contents
- Home Assistant databases
- ESPHome build folders
- ESPHome compiled firmware files
- Raw logs containing network details
- Screenshots showing private URLs, IP addresses, entity IDs, or credentials

## Use Public Placeholders

Replace private values with examples such as:

```yaml
wifi_ssid: "YOUR_WIFI_NAME"
wifi_password: "YOUR_WIFI_PASSWORD"

api_encryption_key: "YOUR_GENERATED_API_KEY"
ota_password: "YOUR_OTA_PASSWORD"

apex_host: "192.168.1.100"
apex_username: "YOUR_APEX_USERNAME"
apex_password: "YOUR_APEX_PASSWORD"
```

For public entity examples, use generic names:

```text
switch.return_pump
switch.uv_lamp
switch.skimmer

timer.fusion_feed_15

script.fusion_15_start
script.fusion_15_cancel
script.fusion_15_restore_equipment
```

Avoid publishing aquarium-specific outlet names, personal room names, address-related names, or device names that identify your home network.

## Before Uploading ESPHome YAML

Check for:

- `wifi:` credentials
- `api:` encryption keys
- `ota:` passwords
- `manual_ip:` addresses
- Device hostnames
- Home Assistant entity IDs
- Webhook URLs
- Local API URLs
- Comments that mention your actual hardware layout or personal automation names

Move all secrets into a private `secrets.yaml` file that is ignored by Git.

## Before Uploading Home Assistant YAML

Check for:

- Apex URLs
- REST usernames and passwords
- Private entity IDs
- Webhook IDs
- Automation IDs that reveal personal information
- Real alert aliases
- Physical outlet mappings
- Direct references to your local equipment naming

Use generic logical examples instead:

```text
Return Pump
UV Lamp
Skimmer
Feed Timer
Alert Banner
```

## Before Uploading Screenshots or Photos

Inspect every visible area for:

- Browser address bars
- Local IP addresses
- Home Assistant hostnames
- Apex hostnames
- Entity IDs
- Wi-Fi names
- Account names
- Email addresses
- Notification text
- Device serial numbers
- QR codes
- Camera views
- Other identifying details in the background

Crop, blur, or replace anything private before publishing.

## Final Pre-Publish Review

Before pushing a new file, ask:

1. Does it contain a password, key, token, or URL?
2. Does it expose a private IP address, hostname, or Wi-Fi name?
3. Does it reveal a personal entity, outlet, room, or alert name?
4. Does the example make sense for another Apex and Home Assistant user?
5. Could someone copy this without needing to know anything about my network?
6. Does the file clearly say what users must customize?
7. Have I tested that no private file is being tracked by Git?

## If Something Private Is Accidentally Committed

Treat it as exposed.

1. Change the password, key, or token immediately.
2. Remove the file or secret from the repository.
3. Rewrite repository history if necessary.
4. Review forks or copies if the repository was already public.
5. Do not assume deleting a file from the latest commit removes it from Git history.
