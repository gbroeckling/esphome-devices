# 32x64 LED matrix countdown

**Board:** esp32dev

HUB75 32x64 RGB matrix countdown timer driven from Home Assistant.

## Usage

Add via `packages:`:

```yaml
packages:
  32x64-countdown:
    url: https://github.com/gbroeckling/esphome-devices
    file: 32x64-countdown/32x64-countdown.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
