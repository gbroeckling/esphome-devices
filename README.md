# esphome-devices

ESPHome configs for devices actually running in my house — published as-is, with
honest notes on what works, what half-works, and what bit me.

These are not polished reference designs. They are working configs off a live
system, with the scars documented. Every device below has a **Status** section in
its own README saying exactly where it stands.

![Alarmo touch keypad running one of these configs, wall mounted and showing live armed state](images/hero-alarmo-keypad.jpg)

## Adopt one in about 30 seconds

Every config carries `dashboard_import`, so the easiest path is the ESPHome
dashboard's **Adopt** flow — no file editing:

1. ESPHome dashboard → **+ New Device** → **Continue**
2. Pick **Adopt** when the device appears, or paste the config URL directly:
   ```
   github://gbroeckling/esphome-devices/<folder>/<folder>.yaml@main
   ```
   e.g. `github://gbroeckling/esphome-devices/e-ink-bw/e-ink-bw.yaml@main`
3. ESPHome generates a local config wrapping mine, with **your** API key and OTA
   password. Flash it.

You need `wifi_ssid` / `wifi_password` in your `secrets.yaml` (see
`secrets.yaml.example`). API encryption keys and OTA passwords are intentionally
stripped from every config here — the Adopt flow gives you your own.

<details>
<summary>Advanced: include as a package instead</summary>

```yaml
packages:
  device_name:
    url: https://github.com/gbroeckling/esphome-devices
    file: <folder>/<folder>.yaml
    ref: main
    refresh: 1d
```

Use this if you want to inherit the config and override pieces of it.
</details>

## The devices

### 🕐 Countdown system — one dial, two displays

These three are **one system, not three gadgets.** The rotary dial sets a
countdown and publishes it to Home Assistant; both LED matrices subscribe to it
and render it big enough to read from across the room, alongside live alarm
state. Adopt the dial on its own if you like, but the matrices are much less
interesting without something feeding them.

```
[newrotary]  ──set countdown──►  Home Assistant  ──►  [32x64-countdown]
 rotary dial                                     └──►  [second-64x64-with-trinity]
```

| Device | Role | Board | Status |
|---|---|---|---|
| [newrotary](newrotary/) | The dial — set the countdown | esp32-s3-devkitc-1 (round) | ✅ Running |
| [32x64-countdown](32x64-countdown/) | HUB75 matrix — countdown + alarm state | esp32dev | ✅ Running |
| [second-64x64-with-trinity](second-64x64-with-trinity/) | Same, on an ESP32-Trinity board | esp32-trinity | ✅ Running |

### 🎙️ Voice assistants

| Device | What it is | Board | Status |
|---|---|---|---|
| [stack5echo-voice-assist](stack5echo-voice-assist/) | M5Stack Atom EchoS3R — hears **and** speaks | esp32s3box | ✅ Running |
| [alarmo-audio-generator](alarmo-audio-generator/) | ReSpeaker Lite voice + Alarmo audio | esp32-s3-devkitc-1 | ✅ Running |
| [voice7exp](voice7exp/) | ESP32-P4 — **mic half of a pair** | esp32p4 | ✅ Running — verified end to end |
| [p4-voice-assist-guition](p4-voice-assist-guition/) | Guition ESP32-P4 — **speaker half of a pair** | esp32p4 | ✅ Running |

### 🖥️ Wall controls

| Device | What it is | Board | Status |
|---|---|---|---|
| [480alarmokeypad](480alarmokeypad/) | 480×480 Alarmo touch keypad | esp32s3 | ✅ Running |
| [rotorydialesp32](rotorydialesp32/) | MaTouch round rotary light dimmer | esp32-s3-devkitc-1 (round) | ✅ Running |
| [480livingdimmer](480livingdimmer/) | 480×480 LVGL touch dimmer | esp32s3 | ✅ Running |

**Two round dials, same hardware, different jobs:**
[rotorydialesp32](rotorydialesp32/) dims lights, while
[newrotary](newrotary/) runs the countdown above. Both are Makerfabs round
displays on the CST826 touch component, so both are pinned to the older ESPHome
branch — if you build one, the other's notes apply too.

### 📄 E-paper displays

| Device | What it is | Board | Status |
|---|---|---|---|
| [e-ink-bw](e-ink-bw/) | XIAO 7.5" e-paper calendar, 5 views | esp32-c3-devkitm-1 | ✅ Running |
| [paperd-calendar](paperd-calendar/) | Paperd.ink Merlot e-paper, 7 views | esp32dev | ✅ Running |

### 🔧 Not built yet

| Device | What it is | Board | Status |
|---|---|---|---|
| [window-opener](window-opener/) | Motorized window via Zigbee relays (no ESP) | n/a | 🚧 In progress — hardware wired, HA logic built, no `cover` entity yet |

## Things that cost me real time

If you build anything in this space, these four will save you a weekend.

**1. Wake-word satellites go silently deaf on older ESPHome.** On ESPHome
2026.2.1 the bundled tflite runtime cannot load current microWakeWord models. It
fails with `Failed to allocate tensors for the streaming model`, the wake engine
stops, and **the device otherwise looks completely healthy** — connected, logging,
responding. It just never hears you. This cost me months on the Atom EchoS3R.
Build wake-word devices on **2026.7.3 or newer**.

**2. …but not everything can move forward.** The `cst826` touch component in the
MaTouch rotary only compiles on the older branch. There is a genuine version split
in this repo: wake-word devices need new ESPHome, some touch devices need old.
Each README says which.

**3. ESPHome 2026.7 broke LVGL rotation, twice.** Rotation moved from the
`display:` block to the `lvgl:` block. And once LVGL owns rotation you must
**delete the `touchscreen:` transform block** — LVGL transforms touch itself, and
keeping both double-transforms every tap so that *every* press lands on one wrong
widget. Symptom: every button on the screen fires the same unrelated entity.

**4. "OTA successful" lies.** I have had the flashing process report success
while the device kept running old firmware. Always read the `compiled on` stamp
out of the live device log before believing a flash landed.

**5. E-paper keeps its last image with no power — so put a timestamp on it.**
The screen alone can never tell you whether what you're reading is current. Both
e-paper configs here render uptime and an "updated HH:MM" stamp for exactly that
reason. On e-paper that's a design requirement of the medium, not a nice-to-have.

## ESP32-P4 voice: read this before you buy

**I do not yet have a single ESP32-P4 doing both microphone and speaker.**

P4 is fast and cheap and the Wi-Fi-over-C6 story mostly works, but the audio side
is still immature in ESPHome and the boards themselves keep undercutting it. What
I ended up with is a **two-board voice assistant**: [voice7exp](voice7exp/) has
the microphone and runs wake-word detection, and it relays the spoken reply over
to [p4-voice-assist-guition](p4-voice-assist-guition/), which has the speaker.
One assistant, two P4s. That works, and it is what is running, but it is a
workaround, not a design.

Board reality doesn't help: one Guition P4 panel I have has a speaker *port* with
nothing wired to it and no built-in microphone at all — a "voice" panel that can
neither hear nor speak out of the box.

If you want one board that hears and answers today, the ESP32-S3 configs here
([stack5echo-voice-assist](stack5echo-voice-assist/),
[alarmo-audio-generator](alarmo-audio-generator/)) are the ones that actually do
it. I'll update this section the moment a single P4 does the whole job.

## Contributing

Issues and Discussions are open. If one of these works on a board I don't have, or
you improve on a caveat I've flagged, I want to hear about it.

## License

MIT
