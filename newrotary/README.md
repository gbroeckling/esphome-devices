# Rotary countdown timer (garage)

**Board:** esp32-s3-devkitc-1 (Makerfabs round display, CST826 touch)

## Status — where this actually stands

**✅ Live, stable, and deliberately frozen.**

A round rotary-dial countdown timer. Turn the knob to set a duration, and the
countdown is published to Home Assistant so other things can react to it — in my
house the LED matrix displays
([32x64-countdown](../32x64-countdown/) and
[second-64x64-with-trinity](../second-64x64-with-trinity/)) read their countdown
values from this device.

This one has been working reliably for a long time and I do not touch it. That is
not a warning about the config — it is a good config — it is just that it is load-
bearing in my house and stable, so it does not get experimented on. It has not
been through the ESPHome-2026.7 modernisation that some other configs here have.

### Notes

- Not to be confused with [rotorydialesp32](../rotorydialesp32/), which is similar
  hardware doing a completely different job (group light dimming).
- Same CST826 touch external component as the dimmer, so the same **ESPHome
  2026.2.1** version constraint most likely applies. I have not tested it on a
  newer branch.
- Published entity names are mine; repoint anything consuming the countdown.

## Usage

Adopt it straight from the ESPHome dashboard:

```
github://gbroeckling/esphome-devices/newrotary/newrotary.yaml@main
```

<details>
<summary>Or include as a package</summary>

```yaml
packages:
  newrotary:
    url: https://github.com/gbroeckling/esphome-devices
    file: newrotary/newrotary.yaml
    ref: main
    refresh: 1d
```
</details>

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
the Adopt flow generates your own.
