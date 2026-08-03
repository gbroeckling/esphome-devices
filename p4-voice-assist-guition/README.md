# Guition ESP32-P4 voice assistant

**Board:** esp32p4, 16MB flash

Home Assistant Voice Assistant satellite on the Guition ESP32-P4 dev board. P4 voice configs are still rare — this one works.

## Usage

Add via `packages:`:

```yaml
packages:
  p4-voice-assist-guition:
    url: https://github.com/gbroeckling/esphome-devices
    file: p4-voice-assist-guition/p4-voice-assist-guition.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
