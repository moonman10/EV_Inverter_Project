# Safety Plan — EV Inverter Bench (≤ 48 V DC)

**Read this before powering anything.** Your bench will run 24–48 V DC with high peak currents. Treat it with respect.

---

## 1) Golden Rules (TL;DR)
- **E‑Stop / main disconnect on DC+**: PSU(+) → Switch → Fuse → XT60(+) → VM(+). PSU(–) → XT60(–) → GND.
- **Fuse is 58 V‑rated** MINI blade (not 32 V). Start 10 A at 24 V; raise only as needed.
- **Current limit the PSU** to **2–3 A** for first bring‑up; increase gradually.
- **One‑hand rule** when live: keep one hand behind your back; no jewelry/watches.
- **Scope safely**: only probe **phase‑to‑GND** with a standard probe. **Never** clip the probe ground to a switching high‑side node.
- **No aggressive regen** until the **brake chopper** (MOSFET + 10 Ω/100 W resistor) is installed and tested.
- **Fan on MOSFETs**; brake resistor on aluminum plate (gets **hot**).
- Keep a **Class ABC extinguisher** nearby; keep the bench clean of flammables.

---

## 2) Bench Setup
- **Disconnect path (preferred):** Blue Sea 6006 battery switch (or similar DC‑rated) on DC+.
- **Inline fuse**: Littelfuse MINI **58 V** in waterproof inline holder; locate close to the PSU.
- **Connectors**: XT60 for DC feed, 4 mm bullets for phases A/B/C.
- **Wire**: 12 AWG silicone for DC bus; 14–16 AWG for phases on this power level.
- **TVS diode (optional)**: SMBJ58A across VM↔GND near inverter (cathode to VM).

---

## 3) First Power‑Up Checklist (No Motor)
- [ ] E‑Stop OFF (open), PSU set to **24 V**, **2–3 A limit**.
- [ ] Verify polarity with a DMM end‑to‑end (PSU → board VM/GND).
- [ ] Visually inspect for solder whiskers, loose screws, stray strands.
- [ ] Release E‑Stop: board idle current should be < ~100 mA.
- [ ] Measure **VM** at J1 and **3.3 V** logic rail; record values in `notes/`.

---

## 4) Motor Bring‑Up Checklist
- [ ] Connect phases A/B/C; secure motor; install guard/cover.
- [ ] Run **is02_offset_gain_cal**; save offsets in `notes/`.
- [ ] Run **is05_motor_id** at low current; copy Rs/Ld/Lq/flux into `user.h`.
- [ ] Run **is06_torque_control** with small Iq steps; tune Id/Iq PI; capture plots.
- [ ] Run **is07_speed_control** with ramps; **NO hard regen** yet.

---

## 5) Brake Chopper (Regen Safety)
- **Topology:** VM(+) → 10 Ω/100 W resistor → N‑MOSFET → GND. MOSFET gate via **UCC27517** @ 10–12 V.
- **Control:** Hysteresis on VM: ON > 27 V, OFF < 25 V (adjust for higher bus later).
- **Wiring:** Keep loop short/thick; shared ground; gate resistor ~10 Ω; gate‑to‑source pulldown 100 kΩ.
- **Thermal:** Mount resistor to aluminum with thermal paste; ensure airflow.
- **Test:** Gentle regen step; verify clamp on scope before any aggressive decels.

---

## 6) Measurement Rules
- **Scope probe ground** must only clip to system **GND**.
- **Phase measurements**: measure **phase‑to‑GND**, not across phases with a single probe.
- For differential phase‑phase, use a **differential probe** or two probes + math (only if you know how).
- Never move probes on a live circuit you don’t control; hit E‑Stop first.

---

## 7) Encoder & Moving Parts
- Cover the spinning can; tie back sleeves and hair.
- Mount encoder (AMT102‑V) on rear shaft; strain‑relief the cable.
- Verify encoder counts direction matches your sign convention.

---

## 8) Fault Handling & Emergency
- **Over‑current**: disable PWM, log fault, require manual re‑enable.
- **Over‑voltage**: force chopper ON; if VM remains high, shut down PWM and log.
- **Watchdog**: resets if control loop stalls; blink fault LED.
- **Emergency**: SMELL/SMOKE → **E‑Stop**, then PSU OFF, then unplug.
- Record a short incident note in `notes/fault_log.md` with time, conditions, and photos.

---

## 9) Bring‑Up Sign‑off
- [ ] Offsets measured and saved
- [ ] Motor ID values in `user.h`
- [ ] Iq step response stable (plots committed)
- [ ] Speed loop tuned with ramps
- [ ] Brake chopper clamps VM within thresholds (scope shots committed)
- [ ] Safety doc read by everyone who touches the bench
