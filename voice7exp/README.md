# ESP32-P4 voice assistant (experimental)

**Board:** esp32p4, 16MB flash

Voice Assistant satellite build on the ESP32-P4 with micro-wake-word. Experimental — expect rough edges.

## Usage

Add via `packages:`:

```yaml
packages:
  voice7exp:
    url: https://github.com/gbroeckling/esphome-devices
    file: voice7exp/voice7exp.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
