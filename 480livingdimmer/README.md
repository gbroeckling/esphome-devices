# 480px round LVGL touch dimmer

**Board:** ESP32-S3 (4848S040 round 480×480 touch panel)

## Status — where this actually stands

**✅ Running. Verified on hardware 2026-08-03.**

This is a wall-mounted LVGL touch dimmer: a grid of light-toggle buttons (green =
included in the group) plus a brightness slider that drives everything currently
selected. It ran as a house-light dimmer and worked well.

**Full disclosure:** shortly after publishing this repo I converted that physical
panel into a WLED controller for LED strips, so the config here is no longer what
is running in my living room. It is a good config and it worked — it is just no
longer under active daily use by me. If you adopt it and hit something, file an
issue and I will dig in, but I won't catch regressions myself.

That is also why this page carries no photo. The panel is still on the wall,
but it renders WLED's interface now, so a picture taken today would document a
different build rather than this one — and a 4848S040 with someone else's UI on
it is indistinguishable from the [alarm keypad](../480alarmokeypad/) by eye. The
screenshots that would show this config no longer exist to be taken.

### Notes

- The brightness slider has a **3% LED floor** — see the
  [rotary dimmer README](../rotorydialesp32/) for why sub-3% is a bad place to
  leave an LED load.
- The published config includes a proper `ota:` block. Note that some panels of
  this type shipped from me with a bare `ota:` and therefore **no working OTA at
  all** — see [480alarmokeypad](../480alarmokeypad/) for that story. If your panel
  refuses OTA on port 3232, you need a one-time USB flash to get onto this config.
- Light entity IDs are mine and are wired in as substitutions — repoint them
  before flashing.

## Usage

Adopt it straight from the ESPHome dashboard:

```
github://gbroeckling/esphome-devices/480livingdimmer/480livingdimmer.yaml@main
```

<details>
<summary>Or include as a package</summary>

```yaml
packages:
  480livingdimmer:
    url: https://github.com/gbroeckling/esphome-devices
    file: 480livingdimmer/480livingdimmer.yaml
    ref: main
    refresh: 1d
```
</details>

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
the Adopt flow generates your own.
