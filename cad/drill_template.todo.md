# Drill Template — 1:1 Printable Guide (Minimal CAD)

## When to use
- You’re about to drill the baseplate for: LaunchPad standoffs, brake resistor, switch, fuse/tie mounts, XT60 anchor.
- You want repeatable hole placement without re-measuring.

## Inputs (from baseplate_notes.md)
- Plate size (W×H): ______ × ______ mm
- Hole centers (x,y mm):
  - LaunchPad P1: (____, ____)  P2: (____, ____)  P3: (____, ____)  P4: (____, ____)
  - Brake resistor: C1 (____, ____)  C2 (____, ____)
  - Switch: pattern ______
  - Fuse/tie mounts: (____, ____)  (____, ____)
  - XT60 anchor: (____, ____)  (____, ____)

## Generate the template (pick one)
1) **No-frills PDF (recommended, minimal CAD)**
   - Use any 2D tool that exports PDF (LibreCAD/Illustrator/PowerPoint/Google Slides).
   - Create a page slightly larger than plate.
   - Draw **crosshairs** at each (x,y) from origin (0,0).
   - Label each hole; add a **100 mm scale bar**.
   - **Export as PDF** with print scale = 100%.

2) **Paper transfer (no software)**
   - Tape graph paper to plate; set a corner as origin.
   - Measure from edges with calipers; mark centers; draw light crosshairs.
   - (Still print the **100 mm scale bar** from any source to verify your printer scaling.)

## Print & scaling check
- Print at **100% / “Actual size”** (no “fit to page”).
- Measure scale bar: **must be 100 mm ±0.5 mm**.
- If off, fix printer settings and reprint.

## Tape & punch
- Tape template to plate; align edges.
- Center-punch every crosshair.
- Remove template; verify all prick marks against parts.

## Drill sequence
- **Pilot drill**: 2.0–2.5 mm through all marks.
- **Final size**:
  - M3 clearance: **3.2–3.4 mm** (start at 3.2; open if needed)
  - XT60/anchor/tie mounts: as required (use step bit for larger holes)
  - Switch/fuse patterns: per datasheet
- Deburr both sides; dry-fit parts.

## Fit check
- LaunchPad: all four standoffs seat? board square? headers clear?
- Brake resistor: screw centers match; air gap OK.
- Switch/fuse/XT60: labels/readability OK; wire paths clear.

## Safety & quality
- Clamp the plate; eye protection; one-hand rule around wiring.
- Keep at least **20 mm** between motor phases and encoder/CAN routes.
- Avoid drilling in no-drill zones (under resistor body, under board headers).

## Rev note
- Template filename: `drill_template_baseplate_rev__.pdf`
- If any hole needed “opening up” >0.5 mm, fix coordinates and regenerate template.
