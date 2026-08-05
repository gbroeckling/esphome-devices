# window-opener

Motorized window opener integrated with Home Assistant as a `cover` entity (open / close).

## Status — where this actually stands

**📐 Design and bill of materials only. Nothing is built yet.**

Parts were ordered 2026-06-30. There is no working hardware, no flashed device,
and no verified end-stop handling. The relay truth table below *has* been measured,
and the mechanical approach is thought through, but treat this whole folder as a
worked design rather than a build you can follow to completion.

Note also that this is **not actually an ESPHome device** — control is Zigbee
through an existing Zigbee2MQTT coordinator, with no ESP board in the path. It
lives in this repo because it is part of the same home-automation effort. If you
were expecting a `.yaml` to flash, there isn't one.

Open items are listed at the bottom.

## Goal

Drive a window via a worm-geared motor coupled to the window's splined drive shaft, controlled from HA — open / close, with safe end-stop handling. Worm gear is self-locking, so the window holds position with no power applied.

## Bill of materials (ordered 2026-06-30)

| Part | Role | Specs / choice | Link |
|------|------|----------------|------|
| JGY370-style worm geared DC motor (reversible CW/CCW, "window/door opener") | Drive | DC 12V, **5 RPM** (slow, high torque), worm gear = **self-locking** | [AliExpress](https://www.aliexpress.com/item/1005009615399277.html) |
| Tuya / Zigbee 2-channel motor controller (RF433 + Zigbee) | Driver + HA bridge | 7–32V (12/24V), 2 relays = forward/reverse, **requires Zigbee hub** | [AliExpress](https://www.aliexpress.com/item/1005009435022266.html) |
| GTRIC D25L35 flexible jaw/spider coupling (aluminum) | Motor shaft → window shaft | bore 5–14mm (fits **6–12mm splined shaft**), 12 N·m max | [AliExpress](https://www.aliexpress.com/item/1005006031199563.html) |

Still needed: 12V DC power supply (sized to motor stall current), mounting bracket, wiring, and end-stop hardware (see open items).

## Architecture

```
12V PSU ──► Tuya/Zigbee 2-ch controller ──► worm geared motor (5 RPM) ──► D25L35 coupling ──► window splined shaft (6–12mm)
                       │ Zigbee
                       ▼
              Z2M (SLZB-06) ──► Home Assistant `cover`
```

Single reversible motor wired across the two channels (interlocked). No ESP board — control is Zigbee via the existing Z2M coordinator.

**Relay truth table (measured):**

| Relay 1 | Relay 2 | Action |
|---------|---------|--------|
| ON | OFF | **Close** |
| OFF | ON | **Open** |
| OFF | OFF | Stop / hold (worm gear self-locks) |
| ON | ON | ⛔ never — must be interlocked out |

So in the HA `cover`: close = R1 on / R2 off; open = R1 off / R2 on; stop = both off. Never drive both relays on simultaneously.

**HA entity:** `windowsopenerbedroom1`, area **Bedroom** (naming scheme implies multiple units — bedroom1, bedroom2, …).

## Home Assistant (built)

Paired in Z2M as **two `light` entities** (not a native cover):
- `light.windowopenerbedroom1_light` = Relay 1 → **CLOSE**
- `light.windowopenerbedroom1_light_2` = Relay 2 → **OPEN**

A template `cover` (`cover.windows_opener_bedroom_1`) + temperature automations + input helpers
are in [`ha/windowsopenerbedroom1.package.yaml`](ha/windowsopenerbedroom1.package.yaml) (drop in
`config/packages/`). Temperature source: `sensor.invisoutlet_temperature` (InvisMasterBath ensuite).

## ⚠️ Critical: end stops — see [END_STOPS.md](END_STOPS.md)

Full travel ≈ **3 minutes**, and the controller has **no limit detection**. The HA timer is the
only software stop; if it's interrupted mid-travel the motor runs into the limit and burns out /
strips the gearing. Mechanical limit switches were **ruled out** (too hard to fit). Chosen
direction: **current-based stall cutoff + hard max-runtime timer** — best done with a small
ESP32/ESPHome controller (replaces the Tuya module). Until fitted: supervised runs only.

## Open items / decisions

- **End-stop implementation** — DECIDE: ESP32 + ESPHome `current_based` cover (recommended) vs dumb overcurrent + timer modules. If ESPHome, the Tuya module is replaced and the HA package below is superseded by an ESPHome cover.
- **Confirm relay direction** — (Tuya path) verify R1=close / R2=open by one supervised jog.
- **Confirm temp sensor** — MasterBath (`sensor.invisoutlet_temperature`) vs SpareBath (`_2`); set thresholds.
- **Measure travel time / stall current** — needed to tune cutoff + max_duration.
- **Enclosure dims** — measure motor + controller (+ ESP, if added) for the 3D-printed housing.

## Status

🔧 Assembled & paired in HA; temp source = InvisMasterBath. HA package written (Tuya template
cover + temp automation) as an interim/supervised path. **Blocked on end-stop method decision**
(current-cutoff via ESPHome vs modules) before unattended use. Enclosure: 3D-printed, parametric
model pending part measurements.
