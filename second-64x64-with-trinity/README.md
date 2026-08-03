# 64x64 LED matrix on Trinity

**Board:** esp32dev (ESP32-Trinity)

HUB75 64x64 RGB matrix display on the ESP32-Trinity driver board.

## Usage

Add via `packages:`:

```yaml
packages:
  second-64x64-with-trinity:
    url: https://github.com/gbroeckling/esphome-devices
    file: second-64x64-with-trinity/second-64x64-with-trinity.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
