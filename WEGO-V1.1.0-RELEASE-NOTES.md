# WeGo Voice Display V1.1.0 — public release candidate

This is a **candidate for validation**, not a confirmed tested release. Based on the working V1.1.0 YAML supplied by the project owner.

## Changes
- Added `device_name` and `friendly_name` substitutions at the top.
- Default thermostat and light counts to `"0"`; all thermostat and lighting names/entity IDs are generic placeholders.
- SEN66 disabled by default (enable only with connected hardware).
- Replaced location-specific weather entity with `weather.home` example.
- Removed embedded API encryption key, OTA password, and fallback Wi-Fi credentials; uses `!secret` references.
- Preserved 180-degree display rotation and mirrored touch axes, and existing menus, display drawing, controls, alarms and SEN66 code.

## Setup
1. Copy the YAML and **all existing `images/` assets** from the project into the ESPHome Builder configuration directory. This file alone is not sufficient: the YAML references many image assets.
2. Change `device_name`, `friendly_name`, and your Home Assistant entity IDs in `substitutions:`. Default thermostat and light counts are zero; set counts only after configuring the corresponding slots.
3. Add your own credentials to the local ESPHome Builder `secrets.yaml`. Do **not** commit that file to a public repository. Required secret names:
   - `wifi_ssid`
   - `wifi_password`
   - `api_encryption_key` (generate a valid ESPHome encryption key)
   - `ota_password`
   - `fallback_ap_password` (choose at least 8 characters)
4. Validate the YAML in your ESPHome Builder version and test on hardware before publishing a release.
5. Alarm features reference `input_boolean.wake_up_alarm_active` and `input_datetime.wake_up_alarm`; calendar features reference `input_text.wego_calendar_today` and `input_text.wego_calendar_tomorrow`. Create/configure these helpers or expect those features to show no data. These features are **not automatically disabled** by the zero thermostat/light counts.
6. Some thermostat controls assume particular HVAC/fan capabilities. Confirm compatibility with the target climate integration before describing it as universal.
7. The online wallpaper URL is a placeholder (`https://example.com/wallpaper.jpg`). Configure a reachable image URL using the device controls if using online wallpaper.

## Caution
The source YAML still uses internal identifiers such as `furnace_*`, `pellet_*`, `solarroom_*`, and `sunroom_*` for the four thermostat slots. These are code identifiers, not customer entity IDs. They have deliberately been left unchanged to minimize risk to working display behavior. The source also contains version references in comments from earlier iterations.

No ESPHome compile/flash validation was performed in this environment. Keep your known-good personal YAML separately.
