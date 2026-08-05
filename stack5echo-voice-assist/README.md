# M5Stack Atom EchoS3R voice assistant

**Board:** `esp32s3box` (see the partition note below — this is deliberate)
ES8311 codec + LP5562 LED via the m5stack external components.

## Status — where this actually stands

**✅ Live, verified end-to-end.**

Announcements are audible, the microphone streams, the wake engine stays in
detect state, and it survives daily use. Unlike the ESP32-P4 pair in this repo,
this is **one board that both hears and speaks**.

### The bug that cost me months

This device was deaf for a very long time and looked completely fine the whole
time — connected, logging, no errors in Home Assistant.

**Root cause:** ESPHome **2026.2.1** ships a tflite runtime that cannot load
current microWakeWord models. On every boot it throws
`Failed to allocate tensors for the streaming model`, the wake engine silently
stops, and nothing else complains. The device is healthy in every way except that
it never hears you.

**Fix:** build with **ESPHome 2026.7.3 or newer.** Never build a wake-word
satellite on 2026.2.1. Older builds flash perfectly happily — that is the trap.

### Other things worth knowing

- **The board is set to `esp32s3box` on purpose.** The partition table already on
  flash has 1.75MB app slots, and OTA cannot change a partition table. Moving to
  the official 8MB M5Stack layout requires a USB flash. If you are adopting onto a
  fresh device you may prefer the official layout instead.
- **Firmware has to stay under 1.75MB** as a result. To get there I dropped the
  Casita status images (replaced with solid-colour pages), the `okay_nabu`,
  `mycroft` and `stop` wake models, and the cloud MP3 assets. If you have room,
  add them back.
- **Do not run a BLE tracker on this device.** I had BLE room-presence scanning on
  it and it starved the voice assistant. Removing it was part of the fix.
- Wake sensitivity is set to *Very sensitive* (0.83) via the Home Assistant
  select entity. Tune to taste.

## Usage

Adopt it straight from the ESPHome dashboard:

```
github://gbroeckling/esphome-devices/stack5echo-voice-assist/stack5echo-voice-assist.yaml@main
```

<details>
<summary>Or include as a package</summary>

```yaml
packages:
  stack5echo-voice-assist:
    url: https://github.com/gbroeckling/esphome-devices
    file: stack5echo-voice-assist/stack5echo-voice-assist.yaml
    ref: main
    refresh: 1d
```
</details>

Requires `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see the repo root
`secrets.yaml.example`). API encryption keys and OTA passwords were stripped —
the Adopt flow generates your own.
