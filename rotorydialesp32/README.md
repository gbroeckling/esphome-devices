# Rotary dial controller

**Board:** esp32-s3-devkitc-1 (Makerfabs, CST826 touch)

Round-display rotary dial controller (Makerfabs board, CST826 touch via external component). Used as a countdown/garage controller.

## Usage

Add via `packages:`:

```yaml
packages:
  rotorydialesp32:
    url: https://github.com/gbroeckling/esphome-devices
    file: rotorydialesp32/rotorydialesp32.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
