# Alarmo audio feedback

**Board:** esp32-s3-devkitc-1

Generates entry/exit/alarm audio feedback tones for the Alarmo integration.

## Usage

Add via `packages:`:

```yaml
packages:
  alarmo-audio-generator:
    url: https://github.com/gbroeckling/esphome-devices
    file: alarmo-audio-generator/alarmo-audio-generator.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
