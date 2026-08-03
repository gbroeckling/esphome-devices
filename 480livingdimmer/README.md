# 480px round touch dimmer

**Board:** esp32s3

LVGL touch dimmer panel on a 480px display for controlling room lights from the wall.

## Usage

Add via `packages:`:

```yaml
packages:
  480livingdimmer:
    url: https://github.com/gbroeckling/esphome-devices
    file: 480livingdimmer/480livingdimmer.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
