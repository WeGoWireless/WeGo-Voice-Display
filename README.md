# WeGo Voice Display

A touchscreen Home Assistant dashboard for the **Waveshare ESP32-S3-Touch-LCD-4.3 (800 × 480)**, built with ESPHome.

> **Status: Release candidate documentation draft.** The private V4 configuration was tested on hardware, and the V5 configuration passed ESPHome validation. A public-ready configuration and complete graphics bundle have **not** yet been assembled or tested.

## Features

- Home screen with clock, weather and wallpaper (built-in fallback or online image)
- Lighting screen supporting Home Assistant `light.*` and `switch.*` entities
- Climate overview and individual thermostat control screens
- Responsive on-screen thermostat adjustments, with a two-second setpoint debounce
- Backlight modes: Always On, Manual, Schedule, Sunrise/Sunset; touch-to-wake
- Device names, entity IDs, and displayed device counts configured at the top of **one ESPHome YAML file**

## Hardware and software

- Waveshare ESP32-S3-Touch-LCD-4.3, 800 × 480, with 16 MB flash and 8 MB PSRAM
- Home Assistant with ESPHome Device Builder
- Working ESPHome API connection to Home Assistant
- Home Assistant climate, lighting and weather entities as appropriate

Developed and validated using ESPHome **2026.9.x**. Other versions are not yet verified.

## Repository layout (planned)

```text
wego-voice-display/
├── README.md
├── wego-voice-display.yaml
└── images/
    ├── mountain_background.jpg
    ├── home.png
    ├── cameras.png
    ├── climate.png
    ├── lighting.png
    ├── more.png
    └── weather/
        └── [weather icon PNGs]
```

**Important:** The graphics are required for compilation. The complete `images/` directory is not yet packaged in this draft. Do not publish the YAML by itself as a complete installation.

## Installation

1. Add the Waveshare display to ESPHome Device Builder in Home Assistant.
2. Place `wego-voice-display.yaml` in the ESPHome configuration directory and copy the complete `images/` folder beside it. Keep the paths as shown above.
3. Edit the `substitutions:` block at the beginning of the YAML. Set device counts, names, entity IDs and weather entity.
4. Set a unique `esphome.name` and `friendly_name` for your device.
5. Configure the normal ESPHome network, API, OTA and secrets settings as required by your installation. **The tested YAML assumes an existing device setup; this documentation does not yet establish a universal first-flash process.**
6. Run **Validate** in ESPHome Device Builder, then install the firmware on the display.
7. Test screen navigation, each thermostat's setpoint/mode/fan controls, each light/switch, wallpaper and backlight settings.

## Configuration

The top of the YAML contains a commented `substitutions:` section. Example:

```yaml
substitutions:
  thermostat_count: "2"
  light_count: "1"
  weather_entity: "weather.home"

  thermostat_1_name: "Living Room"
  thermostat_1_entity: "climate.living_room"
  thermostat_2_name: "Bedroom"
  thermostat_2_entity: "climate.bedroom"

  light_1_name: "Desk Lamp"
  light_1_entity: "light.desk_lamp"
```

This excerpt is illustrative; **retain the remaining substitutions in the complete YAML**. Configure slots in order, starting with 1.

| Setting | Description |
| --- | --- |
| `thermostat_count` | Number of thermostat tiles shown, 0–4 |
| `light_count` | Number of light/switch tiles shown, 0–8 |
| `thermostat_N_name` | Display label |
| `thermostat_N_entity` | Home Assistant `climate.*` entity |
| `light_N_name` | Display label |
| `light_N_entity` | Home Assistant `light.*` or `switch.*` entity |
| `weather_entity` | Home Assistant `weather.*` entity |

**Unused slots:** Leave their valid placeholder entity IDs in place. The counts hide them in the interface; unused Home Assistant subscriptions are still included in the firmware. Changing counts requires reinstalling firmware.

**Public-release safety:** The currently tested V5 file still contains private-installation entity IDs. Replace these with generic examples in the public YAML **without modifying the private tested copy**, then validate that public YAML separately.

## Wallpaper

The display has a built-in mountain fallback image and supports an online wallpaper URL configured through Home Assistant. Available modes include a single image and daily rotation. The placeholder URL in the YAML is not a real wallpaper service; configure a reachable image URL in Home Assistant. Avoid committing private server URLs or credentials.

## Backlight

Select Always On, Manual, Schedule or Sunrise/Sunset using the exposed Home Assistant controls. Touch-to-wake is supported. Test your preferred settings after installation, particularly time-based schedules.

## Troubleshooting

**“Entity ID must have exactly one dot in it.”** Check that every entity substitution resolves to a valid ID such as `climate.living_room` or `light.desk_lamp`. Do not make a substitution reference itself.

**Missing image error:** Confirm the complete `images/` folder exists next to the YAML, including `images/weather/`.

**An unused device appears:** Verify the appropriate `*_count` value and configure used slots consecutively.

**Slow thermostat updates:** The screen can update immediately while a command takes a few seconds to travel through Home Assistant, the thermostat and any separate dashboard. Distinguish touchscreen latency from device round-trip time.

## Before v1.0.0

- [ ] Assemble and verify the complete `images/` directory and asset redistribution rights.
- [ ] Create a **public** YAML with generic entity IDs and no private endpoints or secrets.
- [ ] Confirm clean setup and compilation using the public files, not only the private development installation.
- [ ] Add screenshots of Home, Lighting, Climate and thermostat pages.
- [ ] Document the tested initial flash, Wi-Fi/API/OTA setup and any required secrets.
- [ ] Confirm license and attribution for source code, icons, fonts and images.
- [ ] Tag `v1.0.0` only after these checks pass.

## License

To be selected before public release. Do not assume bundled images and icons share the same license as the YAML code.
