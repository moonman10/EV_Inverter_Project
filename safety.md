# Safety — Day 1 Baseline

## Quick rules
- One-hand rule when probing live circuits.
- Battery switch (hard DC disconnect) within reach.
- Logic E-Stop (NC contact on ENABLE) tested before each run.
- Fuse on DC+ close to source (start 10 A @ 24 V bring-up).
- PSU current limit = 2–3 A for first power-ups.
- Eye protection, no metal jewelry, tidy bench.

## Interlocks to implement this week
- Battery switch path: PSU(+) → Switch → 58 V MINI fuse → XT60(+) → VM(+).
- Logic E-Stop: NC contact **in series** with DRV ENABLE; 10 kΩ pull-down to GND.
- Brake chopper available to clamp VBUS (separate 12 V logic).

## Today’s checks (to initial and date)
- [ ] Repo updated; safety.md replaced (initials/date)
- [ ] E-Stop wiring plan documented (initials/date)
- [ ] Fuse value chosen for bring-up: 10 A MINI @ 58 V (initials/date)
