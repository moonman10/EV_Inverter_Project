# Baseplate Notes — EV Inverter Bench Rig

**Project:** DRV8320RS + LAUNCHXL-F280049C  
**Rev:** 0.1 (draft)  
**Date:** YYYY-MM-DD  
**Material:** ☐ Aluminum plate ☐ Plywood ☐ 3D-print only for small brackets (optional)  
**Plate thickness:** ____ mm

---

## 1) Coordinate System & Overall Size
- **Origin (0,0):** lower-left corner (top view)
- **X axis:** left→right, **Y axis:** bottom→top
- **Outer size:** Width **W = ____ mm**, Height **H = ____ mm**
- **Desk mount:** ☐ rubber feet ☐ through-bolts ☐ clamps

---

## 2) LaunchPad Mount (LAUNCHXL-F280049C, Booster on top)
- **Hole size:** M3 clearance (Ø ≈ 3.2–3.4 mm) or measured
- **Standoff height:** ____ mm (clear underside pins)
- **Corner hole coordinates (from origin):**
  - P1 (x=____, y=____) mm
  - P2 (x=____, y=____) mm
  - P3 (x=____, y=____) mm
  - P4 (x=____, y=____) mm
- **Keepouts:** under headers: ____ × ____ mm

---

## 3) Power Path (left→right flow recommended)
### 3.1 Main/Battery Switch (e.g., Blue Sea 6006)
- **Mounting pattern:** ____ × ____ mm; hole Ø ____ mm
- **Center position:** (x=____, y=____) mm
- **Rotation/label:** ON points → ______

### 3.2 Inline Fuse Holder (MINI, 58 V rated)
- **Mount style:** ☐ screws ☐ zip-tie saddles ☐ adhesive pad
- **Tie/holes coords:** (x=____, y=____), (x=____, y=____)

### 3.3 XT60 Pass-Through / Anchor
- **Type:** ☐ panel mount ☐ pigtail anchor
- **Anchor holes coords:** (x=____, y=____), (x=____, y=____)

### 3.4 J1 VM+/GND Entry (to BoosterPack)
- **Shortest path (XT60 → J1):** ____ mm
- **Wire gauge:** 12 AWG (last ____ mm may step to 14–16 AWG)

---

## 4) Motor & Signal Routing (simple channels/clips)
### 4.1 Motor Phases (A/B/C → J5)
- **Exit direction:** ☐ left ☐ right ☐ top ☐ bottom
- **Channel/clip spacing:** positions (x=____, y=____), (x=____, y=____)
- **Label near exit:** A / B / C

### 4.2 Encoder (AMT102-V → eQEP J12/J13)
- **Route separation from phases:** ≥ 20 mm
- **Clips/tie points:** (____, ____), (____, ____)
- **Slack loop diameter:** ____ mm

### 4.3 CAN Twisted Pair (to USB-to-CAN)
- **Route:** opposite side of motor phases (≥ 20–30 mm separation)
- **Tie points:** (____, ____), (____, ____)
- **Bench terminator switch (if any):** position (____, ____)

---

## 5) Brake Resistor (10 Ω / 100 W aluminum-clad)
- **Mount holes (center-to-center):** ____ mm; hole Ø ____ mm
- **Position:** (x=____, y=____) mm
- **Standoff height (air gap under body):** ____ mm
- **Clearance to plastic/wood:** ≥ ____ mm
- **Cable entry to MOSFET/chopper board:** ____ mm

---

## 6) Keepouts & Labels
- **High-current keepout halo** around switch/fuse/XT60: radius ____ mm
- **No-drill zones:** under LaunchPad headers, under brake resistor body
- **Adhesive/silk labels to place:**
  - “VM+” at J1 route
  - “GND” at J1 return
  - “A/B/C” at motor exit
  - “E-STOP” near button
  - “CAN H/L” near connector

---

## 7) Fasteners & Small Hardware
- **Standoffs:** M3 × ____ mm (qty ____)
- **Screws:** M3 × ____ mm pan head (qty ____) + washers
- **Tie-mounts:** size ____ mm; hole Ø ____ mm (qty ____)
- **Optional 3D prints (only if needed):** small wire clips, XT60 anchor block

---

## 8) Harness Cut List (initial)
- **PSU(+) → Switch → Fuse → XT60F(+):** ____ mm
- **PSU(–) → XT60F(–):** ____ mm
- **XT60M(+) → J1 VM+:** ____ mm
- **XT60M(–) → J1 GND:** ____ mm
- **J5 → Motor A/B/C:** ____ / ____ / ____ mm
- **Encoder → eQEP header:** ____ mm
- **CAN → USB-CAN:** ____ mm

---

## 9) Drilling Notes (minimal CAD)
- **Template:** create simple 1:1 PDF/DXF with hole centers only
- **Hole tolerances:** drill Ø +0.2 mm for M3 clearance
- **Countersink/counterbore:** ☐ none ☐ yes (depth ____ mm)
- **Edge deburr:** yes

---

## 10) To-Do Before Drilling
- [ ] Verify LaunchPad hole spacing with calipers
- [ ] Confirm brake resistor mount spacing & cable reach
- [ ] Mark no-drill zones under boards/resistor
- [ ] Dry-fit switch → fuse → XT60 path with wire lengths
- [ ] Photograph bench layout for reference
