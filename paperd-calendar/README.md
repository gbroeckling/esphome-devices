# E-paper calendar display

**Board:** esp32dev (PaperD.ink)

Battery-friendly e-paper calendar that renders upcoming events from Home Assistant.

## Usage

Add via `packages:`:

```yaml
packages:
  paperd-calendar:
    url: https://github.com/gbroeckling/esphome-devices
    file: paperd-calendar/paperd-calendar.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
