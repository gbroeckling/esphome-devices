# esphome-devices

ESPHome configs for devices running in my home, published for anyone to adopt.
Each device lives in its own folder with its yaml and a short README.

| Device | What it is | Board |
|---|---|---|
| [voice7exp](voice7exp/) | ESP32-P4 voice assistant (experimental) | esp32p4, 16MB flash |
| [p4-voice-assist-guition](p4-voice-assist-guition/) | Guition ESP32-P4 voice assistant | esp32p4, 16MB flash |
| [stack5echo-voice-assist](stack5echo-voice-assist/) | M5Stack Atom EchoS3R voice assistant | esp32s3box |
| [paperd-calendar](paperd-calendar/) | E-paper calendar display | esp32dev (PaperD.ink) |
| [e-ink-bw](e-ink-bw/) | XIAO ESP32-C3 e-ink display | esp32-c3-devkitm-1 (Seeed XIAO) |
| [480livingdimmer](480livingdimmer/) | 480px round touch dimmer | esp32s3 |
| [480alarmokeypad](480alarmokeypad/) | 480px Alarmo touch keypad | esp32s3 |
| [32x64-countdown](32x64-countdown/) | 32x64 LED matrix countdown | esp32dev |
| [second-64x64-with-trinity](second-64x64-with-trinity/) | 64x64 LED matrix on Trinity | esp32dev (ESP32-Trinity) |
| [rotorydialesp32](rotorydialesp32/) | Rotary dial controller | esp32-s3-devkitc-1 (Makerfabs, CST826 touch) |
| [newrotary](newrotary/) | Rotary controller v2 | esp32-s3-devkitc-1 (Makerfabs, CST826 touch) |
| [alarmo-audio-generator](alarmo-audio-generator/) | Alarmo audio feedback | esp32-s3-devkitc-1 |
| [window-opener](window-opener/) | Motorized window -> HA cover (early scaffolding, HA-side only) | TBD |

## Using a config

Every yaml is self-contained and importable:

```yaml
packages:
  device_name:
    url: https://github.com/gbroeckling/esphome-devices
    file: <folder>/<folder>.yaml
    ref: main
```

WiFi creds come from `secrets.yaml` (`wifi_ssid` / `wifi_password` — see
`secrets.yaml.example`). API encryption keys and OTA passwords are intentionally
absent; add your own.

## License

MIT
