# Test Procedure — DRV8320RS + F280049C (24 V bench)

**Goal:** Run the same milestones every time so changes are safe, measurable, and comparable.

---

## 0) Pre-Flight (every session)
- Bench PSU: **24 V**, **2–3 A current limit** (first runs), fan clear
- Logic E-Stop (NC → Enable) verified yesterday
- Motor secured; XT60 polarity double-checked
- Scope/DMM ready; CAN logger ready (if used)

**Pass gate:** No loose wires, correct polarity, E-Stop drops Enable LOW.

---

## 1) Smoke Test (daily quick run, ≤3 min)
1. Power **ON** (PWM disabled, Enable LOW).
2. Idle current (no motor) ______ A (target < **0.15 A**).
3. Read Vbus via ADC: ______ V (within **±5%** of DMM).
4. Press E-Stop → Enable LOW detected; release; keep PWM disabled.

**Log:** time, idle_current_A, vbus_adc_V, vbus_dmm_V, e_stop_ok

---

## 2) ADC Sanity (VM & currents)
**Setup:** No motor. PWM disabled. DMM on bus.

- Vbus ADC vs DMM: Δ = ______ V (target **≤ ±5%**).
- Phase current offsets at idle (Ia/Ib/Ic): ______ A (target **|offset| ≤ 0.05 A** each).
- Thermal sensor (if any) stable: ______ °C (room ±5 °C).

**Pass gate:** Vbus scale correct; offsets noted in `parameters.yaml`.

**Log columns:** time, vbus_adc, vbus_dmm, ia_offset, ib_offset, ic_offset, temp

---

## 3) PWM Timing (no motor)
**Setup:** Enable LOW (driver asleep), observe MCU PWM pins or low-voltage test points.

- PWM frequency = ______ kHz (target e.g. **10–20 kHz**).
- Deadtime measured = ______ ns (target **≥ 250 ns**, per design).
- Phase alignment / complementary outputs correct.

**Pass gate:** Frequency & deadtime within spec; no unexpected toggles.

**Log:** pwm_freq_khz, deadtime_ns, notes

---

## 4) eQEP / Encoder
**Setup:** Connect AMT102-V to eQEP header (e.g., J12). Power ON.

- Stationary counts stable (±1 LSB jitter max).
- Hand-turn forward/backward: counts monotonic; index once/rev.
- CPR configured = ______ ; measured index period = ______ rev.

**Pass gate:** Direction correct, index detected, CPR confirmed.

**Log:** eqep_cpr_cfg, eqep_counts_delta, index_seen_bool

---

## 5) First Spin (open-loop)
**Setup:** Connect motor to J5 A/B/C. Current limit still ≤3 A.

- Ramp from 0 → low duty; observe smooth start.
- PSU current peak ______ A (target **≤ 2 A** for low speed).
- If direction wrong, swap any two phases (power OFF first).

**Pass gate:** Smooth rotation, no buzzing/stall, current reasonable.

**Video/Log:** rpm_cmd, rpm_meas, i_psu_A, notes

---

## 6) FOC Current Loop (Iq step)
**Setup:** Close inner Id/Iq loop, low current limits.

- Command Iq step: 0 → **2.0 A**, hold 300 ms, back to 0.
- Peak overshoot ______ % (target **≤ 20%**).
- 2% settling time ______ ms (target **≤ 50 ms**).
- No sustained oscillation; Id near 0 (|Id| ≤ 0.3 A).

**Pass gate:** Meets overshoot/settling targets, currents within limits.

**Log:** iq_cmd_A, iq_meas_A, overshoot_pct, t_settle_ms, id_meas_A

---

## 7) Speed Loop (velocity step)
**Setup:** Outer speed loop enabled, current limit enforced.

- Step 0 → **500 rpm** → 0.
- Overshoot ______ % (target **≤ 10%**), settle ______ s (target **≤ 0.5 s**).
- Current within set limit; no fault flags.

**Pass gate:** Meets speed response and current limit targets.

**Log:** rpm_cmd, rpm_meas, iq_peak_A, overshoot_pct, t_settle_s

---

## 8) Regen Clamp (brake chopper)
**Setup:** Brake chopper assembled (buck→12 V, UCC27517, MOSFET + 10 Ω/100 W, 100 kΩ gate pulldown). Thresholds e.g. **27 V ON / 25 V OFF**.

- Apply small negative torque / hand-spin down.
- Observe Vbus crosses ON threshold → chopper gate active.
- Vbus held between ON/OFF thresholds (no chatter).
- Resistor temperature after 1–2 min ______ °C (target **< 80 °C**).

**Pass gate:** Vbus clamped, stable hysteresis, temps acceptable.

**Log:** vbus_V, chopper_state, on_V, off_V, temp_resistor_C

---

## 9) CAN Telemetry
**Setup:** USB-to-CAN attached; two 120 Ω terminators present; bitrate matched (e.g., **500 kbit/s**).

- Telemetry frames at ______ Hz (target **50–100 Hz**).
- No error frames over 60 s.
- CSV saved; columns match `software/log_format.md`.

**Pass gate:** Clean stream, correct rates/IDs, CSV usable.

**Log:** can_rate_hz, err_frames, csv_path

---

## 10) FLASH Boot & Startup Gating
**Setup:** Build FLASH image with startup inhibit (PWM disabled until checks pass).

- Cold power cycle without debugger.
- On boot: PWM **disabled**, Enable LOW, no faults.
- Only after Enable HIGH and interlocks OK does PWM start.

**Pass gate:** Safe standalone behavior; no unintended spin.

**Log:** boot_ok_bool, interlock_status, time_to_ready_s

---

## 11) Weekly Reliability (once per week)
- 30–60 min run at modest load, log temps (MOSFETs, resistor), Vbus, current.
- 10× E-Stop drills during motion: stop time ______ ms (target **< 50 ms** command drop**).
- Post-run inspection: screws tight, no browning/odor, connectors cool.

**Pass gate:** No drift/faults; stable temperatures.

**Log:** t_run_min, temp_fet_C, temp_res_C, vbus_mean_V, faults

---

## Data Files to Save (each session)
- `notes/logs/YYYY-MM-DD_run.csv` (CAN/telemetry)
- `hardware/photos/YYYY-MM-DD_*.jpg` (wiring, scope shots)
- Plots: Iq step, speed step, Vbus clamp (PNG or PDF)

---

## Troubleshooting Notes
- **High idle current:** check Enable truly LOW, driver sleep, no shorts on VM.
- **No eQEP index:** encoder DIP/PPR mismatch; wrong header; missing 5 V/GND.
- **FOC unstable:** wrong current polarity, bad scaling, too aggressive PI, low PWM freq.
- **Chopper chatters:** hysteresis too narrow; noisy Vbus ADC; add small RC filter to threshold sense.
- **CAN errors:** missing terminator; bitrate mismatch; long untwisted leads.

---
