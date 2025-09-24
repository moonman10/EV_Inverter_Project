# Bring-Up Checklist — DRV8320RS + F280049C (24 V bench)

**Purpose:** Power up safely, verify basics, then achieve a controlled first spin.

---

## Tools & setup
- Bench PSU set to **24 V**, **2–3 A current limit**
- DMM (continuity + voltage), oscilloscope (optional but useful)
- IR thermometer (optional), safety glasses
- Logic E-Stop wired: **NC** in series with **Enable**, 10 kΩ pull-down to GND
- Inline **MINI fuse** on + line near source (start **10 A @ 58 V** rated holder)
- Battery switch on + line; XT60 quick-disconnect

---

## Stage A — Cold inspection (no power)
1. Check polarity labels end-to-end: PSU(+) → switch → fuse → XT60(+) → **VM+ (J1)**; PSU(–) → XT60(–) → **GND (J1)**.
2. Tug-test all crimps/solder joints; no stray strands.
3. Verify motor **not** connected yet. Encoder unplugged.
4. E-Stop: button released (closed); you should plan for Enable to go LOW when pressed.

**Pass criteria:** Clear polarity, tight connections, E-Stop wiring identified.

---

## Stage B — Source harness test (board still disconnected)
1. Switch **OFF**. Set PSU 24 V, 2–3 A.
2. Turn switch **ON**. Measure at **XT60F**: ~24 V between + and –.
3. Switch **OFF**; voltage decays to 0 V.

**Pass criteria:** Correct voltage present only when switch ON.

---

## Stage C — Board-only power (no motor, PWM disabled)
1. Mate XT60F ↔ XT60M. Ensure the board is on standoffs; fan area clear.
2. Ensure firmware comes up with **PWM disabled** and **Enable = LOW**.
3. Switch **ON**. PSU should read a small idle current (typically < **150 mA** for logic + LEDs).
4. Check **nFAULT** indicator (should be deasserted). Check 3.3 V rail if accessible.
5. Read/print **Vbus** (ADC). It should match ~24 V within a few percent.
6. Press **E-Stop**: confirm **Enable goes LOW** (LED/state) and your firmware reports “stopped”.
7. Release E-Stop; keep PWM **disabled** for now.

**Pass criteria:** Idle current modest, no fault lights, Vbus sane, E-Stop drops Enable reliably.

---

## Stage D — PWM sanity (still no motor)
*(Optional, but useful for mapping signals)*
1. Keep **Enable LOW** so the driver is asleep (no gate drive).
2. Toggle your PWM outputs at the MCU and confirm the signals at the **MCU pins** or low-voltage header (don’t probe switch nodes yet without a differential probe).
3. Verify frequency and deadtime are what you expect.

**Pass criteria:** PWM clocks route correctly; no unexpected faults.

---

## Stage E — First spin (open-loop, low energy)
1. Power **OFF**, disconnect XT60, wait for caps to discharge.
2. Connect motor phases to **J5 A/B/C**. Secure the motor so it can’t walk.
3. Reconnect XT60, switch **ON**. Keep a hand near the battery switch.
4. Set a **very low duty/command**. Bring **Enable HIGH** to allow gate drive.
5. Ramp slowly. Watch PSU current—should rise smoothly; keep it **< 1–2 A** for the first run.
6. If rotation is reverse, **power OFF** and swap any two phase wires.
7. Stop command → motor should coast; verify E-Stop cuts commutation instantly.

**Pass criteria:** Smooth, low-speed rotation; no surging or abnormal current; safe stop works.

---

## Stage F — Encoder quick check (optional today)
1. Power **OFF**. Connect encoder 5 V/GND and A/B/Z to chosen eQEP header (**J12** if using eQEP1).
2. Power **ON** with motor stationary: counts steady.
3. Hand-turn shaft slowly: counts increase/decrease consistently; index pulse seen once per rev.

**Pass criteria:** eQEP counts behave and index is detected.

---

## Stage G — Notes to record
- PSU idle current (no motor): ______ A
- Vbus reading at 24 V: ______ V
- First-spin current: peak ______ A, typical ______ A
- Direction fix needed? yes/no
- E-Stop action verified? yes/no
- Any heat/smell/noise anomalies? describe briefly

---

## Safety reminders
- Use the **one-hand rule** when probing live.
- If you lack a differential probe, **do not** clip oscilloscope ground to any switching node; measure phase-to-GND only.
- Stop immediately on unexpected current spikes, smell, or heat; switch **OFF** before touching wiring.

---

## Troubleshooting hints
- **No power:** check fuse continuity; switch orientation; XT60 polarity.
- **nFAULT asserted:** verify Enable level, supply rails, driver config; read fault bits if available.
- **High idle current:** look for wiring mistakes, hot parts, or enabled gate drive when it should be asleep.
- **No spin:** wrong phase order, Enable not asserted, PWM not routed, or current limit too low on PSU.
