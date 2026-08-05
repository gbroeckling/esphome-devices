# MaTouch round 480×480 rotary dimmer

**Board:** esp32-s3-devkitc-1 (MaTouch round display, ST7701S + CST826 touch)
**Controls:** rotary encoder on GPIO10/13, push button on GPIO14

> **Sibling dial** — [newrotary](../newrotary/) is the same Makerfabs round
> hardware running a countdown timer instead of a dimmer, and it drives the
> [countdown system](../32x64-countdown/) displays. Same CST826 touch component,
> so the ESPHome version constraint below applies to both dials.

## Status — where this actually stands

**⚠️ Works, and feels good on LEDs now — but it is pinned to an older ESPHome.**

A wall dimmer that drives a group of lights from one knob, using delta-from-a-baseline
group dimming with per-light bitmask toggles on the LVGL buttons, so you can pick
which lights the knob is currently driving.

### Version constraint — read this first

**This config only compiles on ESPHome 2026.2.1.** The `cst826` touch external
component does not build on newer branches. That is the opposite of the wake-word
devices in this repo, which *require* 2026.7.3+. There is a genuine version split
here and I have not resolved it.

### Making a rotary knob feel right on LED loads

The first version felt broken on LED lights. Three separate causes, all fixed in
this config, and all worth knowing if you build anything similar:

1. **The commit-to-HA throttle had a leading delay in `restart` mode.** That meant
   *zero* updates were sent while the knob was actually turning — the lights only
   jumped once you stopped. Fixed with rate-limited live sends roughly every
   300ms during rotation, plus a trailing final commit ~350ms after the last
   detent.
2. **A linear step curve is wrong for LEDs.** Steps of 0.2–1% per detent are below
   most LED drivers' resolution at the bottom (knob feels dead) and invisible at
   the top. Now it steps **multiplicatively** — about 4% of the current level,
   clamped to 0.5–3% — which is closer to perceptual.
3. **No low-end handling walked lights through the 1–3% flicker zone.** There is
   now a `led_min_pct` floor (default 3%): turning up from off jumps straight to
   the floor, turning down lands on the floor and the next detent is a clean off.
   The clamp is per-light and non-accumulating, so exact reversibility is
   preserved.

### Other notes

- Turning to 0% issues a real `turn_off` rather than `turn_on` at brightness 0 —
  the latter is ambiguous for some light platforms and left lights in a weird
  state.
- Fast spin is capped at 10% per detent.
- **Behaviour change worth knowing:** turning the knob up from all-off now ramps
  from the 3% floor rather than restoring the previous scene.
- The lights this drives are mine and are wired in as substitutions — repoint
  them before flashing.

## Usage

Adopt it straight from the ESPHome dashboard:

```
github://gbroeckling/esphome-devices/rotorydialesp32/rotorydialesp32.yaml@main
```

<details>
<summary>Or include as a package</summary>

```yaml
packages:
  rotorydialesp32:
    url: https://github.com/gbroeckling/esphome-devices
    file: rotorydialesp32/rotorydialesp32.yaml
    ref: main
    refresh: 1d
```
</details>

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
the Adopt flow generates your own.
