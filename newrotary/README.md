# Rotary controller v2

**Board:** esp32-s3-devkitc-1 (Makerfabs, CST826 touch)

Newer iteration of the rotary dial controller.

## Usage

Add via `packages:`:

```yaml
packages:
  newrotary:
    url: https://github.com/gbroeckling/esphome-devices
    file: newrotary/newrotary.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
