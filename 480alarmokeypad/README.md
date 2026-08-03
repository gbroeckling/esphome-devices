# 480px Alarmo touch keypad

**Board:** esp32s3

Touchscreen keypad for the Alarmo alarm integration — arm/disarm with code entry, state colors.

## Usage

Add via `packages:`:

```yaml
packages:
  480alarmokeypad:
    url: https://github.com/gbroeckling/esphome-devices
    file: 480alarmokeypad/480alarmokeypad.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
