# M5Stack Atom EchoS3R voice assistant

**Board:** esp32s3box

Voice Assistant satellite for the M5Stack Atom EchoS3R (ES8311 codec + LP5562 LED via m5stack external components). Note: micro-wake-word models require ESPHome >= 2026.7 — older builds flash fine but the device is deaf.

## Usage

Add via `packages:`:

```yaml
packages:
  stack5echo-voice-assist:
    url: https://github.com/gbroeckling/esphome-devices
    file: stack5echo-voice-assist/stack5echo-voice-assist.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
