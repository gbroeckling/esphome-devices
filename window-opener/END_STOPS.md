# End-stop protection (REQUIRED before unattended use)

## The problem

The Tuya/Zigbee 2-channel controller is just two dumb relays — **no position feedback, no
current limit**. Full travel ≈ **3 minutes**. The only thing stopping the motor is the HA `delay`;
if that's interrupted mid-travel (HA restart, Z2M drop) the relay stays latched and the motor
drives into the limit → destroyed window ($800) and/or burned motor.

## Requirement: safety must be in HARDWARE, not software

Ruled out:
- **Mechanical limit switches** — too hard to fit.
- **ESP32 / ESPHome watchdog** — a watchdog living inside the firmware it's meant to watch is a
  single point of failure: if the program hangs with the relay on, nothing saves the window.

So the protection must act by physics/analog hardware, independent of any running program.

## Key fact: a timer does NOT protect the window

5 minutes (or even 30 s) of stalling against the stop wrecks the window long before any timeout
fires. Only something that reacts in ~1 s protects it — a **fast stall-current trip** (or a torque
limiter). A max-runtime timer is only a *motor* backstop (motor ≈ $15), not window protection.

## Chosen: inline analog overcurrent cutoff (no MCU)

A dumb adjustable overcurrent module in series with the motor lead: passes running current, and
when the window hits the stop and current rises, it physically opens the circuit. No firmware.
Keep the Tuya relay for direction and the HA timer for the *normal* stop — this module sits
underneath as the independent guardian.

- Set trip just above running current, below stall.
- **Inrush is not a concern here.** On this small, low-geared motor the power-on current blip is
  brief (ms) and modest. Because the module trips on *sustained* overcurrent (≥ ~0.5 s), it
  ignores the startup blip naturally — no special blanking needed.
- Prefer a **latching** trip (manual/relay reset) so it doesn't chatter on/off at the stop; or an
  auto-reset breaker is acceptable if the HA timer turns the direction relay off within ~travel time.
- Optional: a cheap inline **thermal fuse / resettable breaker** as a second motor backstop.

### Motor specs (confirmed from datasheets)
JGY370-class, 12V: **~5 W, ~500 mA continuous, ~10 kg·cm**, stall likely **~1.5–2 A** (measure to confirm).
Tiny electrically → no meaningful inrush. BUT the threat to the window is **torque, not current**:
the worm reduction makes ~10 kg·cm of self-locking torque from that half-amp and dumps it into the
window operator at the stop. A fast current trip (<1 s) still protects it — stall = the over-torque
moment = the current rise.

### Need to size it
Because currents are small, use a **low-current-rated** overcurrent module (most cheap ones are
5–30 A and won't resolve ~1 A). Measure with a clamp meter on a motor lead:
- **Running current** (window moving normally)
- **Stall current** (motor held at the end stop)

Trip point goes between the two with margin. If the gap is small, a torque limiter (below) is the
fallback.

## Bulletproof alternative: mechanical slip clutch / torque limiter

A torque-limiting clutch (motor → clutch → window shaft) that slips at a safe torque. The window
physically cannot be overloaded regardless of any electrical/software fault. More mechanical
fitting on the 6–12 mm shaft + sourcing the right torque rating; kept as the fallback if the
current margin is too tight to trip reliably.

## Interim rule (until cutoff is fitted)

Every open/close is **supervised**, and `input_number.windowsopenerbedroom1_travel` is set
**shorter** than measured full travel so it always stops before the limit.
