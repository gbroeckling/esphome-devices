# XIAO ESP32-C3 e-ink display

**Board:** esp32-c3-devkitm-1 (Seeed XIAO)

Compact black/white e-ink status display on a Seeed XIAO ESP32-C3. Fonts pull from URLs at compile time — no local assets needed.

## Usage

Add via `packages:`:

```yaml
packages:
  e-ink-bw:
    url: https://github.com/gbroeckling/esphome-devices
    file: e-ink-bw/e-ink-bw.yaml
    ref: main
    refresh: 1d
```

Or take the config whole — it also carries `dashboard_import`, so devices flashed
from it show up as adoptable in the ESPHome dashboard.

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
add your own after adoption.
