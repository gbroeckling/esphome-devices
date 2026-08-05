# 64×64 HUB75 LED matrix on ESP32-Trinity

**Board:** ESP32-Trinity (esp32dev core) · 64×64 HUB75 RGB matrix

## Status — where this actually stands

**⚠️ Live and running. Same two known issues as its sibling.**

The second of two LED matrix displays in the house, on the
[ESP32-Trinity](https://github.com/witnessmenow/ESP32-Trinity) driver board —
which is a much nicer way to drive HUB75 than hand-wiring a bare esp32dev.

It renders the same pages as its sibling — see the photos in
[32x64-countdown](../32x64-countdown/) for what the output looks like.

### Known issues

Identical to [32x64-countdown](../32x64-countdown/):

- **The countdown sensor is dead** — `alarmo_time_remaining` needs a Home
  Assistant template sensor to exist, which I have not built. Alarm state
  displays fine; the countdown area does not populate.
- **`pending` is displayed as "ARMING"**, which is wrong during the entry delay.

### Notes

- The Trinity handles the HUB75 pinout for you, which removes the most common
  source of "my matrix shows garbage" problems.
- Countdown values are consumed from the rotary timer
  ([newrotary](../newrotary/)) in my setup.

## Usage

Adopt it straight from the ESPHome dashboard:

```
github://gbroeckling/esphome-devices/second-64x64-with-trinity/second-64x64-with-trinity.yaml@main
```

<details>
<summary>Or include as a package</summary>

```yaml
packages:
  second-64x64-with-trinity:
    url: https://github.com/gbroeckling/esphome-devices
    file: second-64x64-with-trinity/second-64x64-with-trinity.yaml
    ref: main
    refresh: 1d
```
</details>

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
the Adopt flow generates your own.
