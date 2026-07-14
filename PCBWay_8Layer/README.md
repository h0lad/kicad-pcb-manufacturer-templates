# PCBWay 8-Layer FR-4 KiCad Template

KiCad project template matching **PCBWay's default online-quote settings** for a
standard FR-4 8-layer board -- the pre-selected, no-surcharge options on
<https://www.pcbway.com/orderonline.aspx>. Constraints target that default quote
(6/6 mil, 0.3 mm hole, 1 oz, TG150, HASL-with-lead), so a design that passes DRC
stays inside the cheapest order without hitting an upcharge. Capability data:
<https://www.pcbway.com/capabilities.html>. (July 2026.)

Files are KiCad 8 format; KiCad 10 opens and migrates them on first save.

## Default order settings (from the quote page)

| Option          | Default              |
|-----------------|----------------------|
| Material / TG   | FR-4, S1000H **TG150** |
| Thickness       | 1.6 mm               |
| Outer / inner Cu| 1 oz / 1 oz          |
| Min track/space | **6/6 mil (0.152 mm)** |
| Min hole        | **0.3 mm**           |
| Solder mask     | Green                |
| Silkscreen      | White                |
| Surface finish  | **HASL with lead**   |
| Via process     | **Tenting** (mask over vias) |

Finer trace and smaller holes exist but are **not** the default -- see Ordering notes.

## Installation

Copy the `PCBWay_8Layer/` directory into your user template path, then
File -> New Project from Template -> User Templates:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

## Stackup (1.6 mm)

| Layer   | Material            | Thickness | Plane |
|---------|---------------------|----------:|-------|
| F.Cu    | Cu 1 oz             | 0.0350 mm | SIG   |
| Prepreg | 2116 RC58, Dk 4.45  | 0.1195 mm |       |
| In1.Cu  | Cu 1 oz             | 0.0350 mm | GND   |
| Core    | FR-4, Dk 4.6        | 0.2300 mm |       |
| In2.Cu  | Cu 1 oz             | 0.0350 mm | SIG   |
| Prepreg | 7628 RC46, Dk 4.74  | 0.1750 mm |       |
| In3.Cu  | Cu 1 oz             | 0.0350 mm | PWR   |
| Core    | FR-4, Dk 4.6        | 0.2300 mm |       |
| In4.Cu  | Cu 1 oz             | 0.0350 mm | GND   |
| Prepreg | 7628 RC46, Dk 4.74  | 0.1750 mm |       |
| In5.Cu  | Cu 1 oz             | 0.0350 mm | SIG   |
| Core    | FR-4, Dk 4.6        | 0.2300 mm |       |
| In6.Cu  | Cu 1 oz             | 0.0350 mm | GND   |
| Prepreg | 2116 RC58, Dk 4.45  | 0.1195 mm |       |
| B.Cu    | Cu 1 oz             | 0.0350 mm | SIG   |

Plane plan SIG / GND / SIG / PWR / GND / SIG / GND / SIG. PCBWay's standard multilayer uses **1 oz inner copper**, so inner and outer
layers carry the same current. PCBWay assigns the final press-fit stackup at order
time; adjust in Board Setup if you request a specific one.

## Global DRC constraints (Board Setup -> Constraints)

Value = template setting. `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set             | Value          | PCBWay default / spec |
|------------------------------------|----------------|-----------------------|
| Min track width / spacing (global) | 0.15 mm        | 6/6 mil (default; 3-5 mil = option) |
| Min clearance (global)             | 0.15 mm        | 6 mil (default) |
| Min via diameter / drill (global)  | 0.60 / 0.30 mm | 0.3 mm hole + 0.15 mm ring |
| Min via annular ring (global)      | 0.15 mm        | 6 mil |
| Min PTH drill (global)             | 0.30 mm        | default min hole (0.15-0.25 = option) |
| PTH annular ring `[dru]`           | 0.15 mm        | 6 mil |
| PTH pad hole-to-hole `[dru]`       | 0.50 mm        | hole spacing |
| PTH hole to track `[dru]`          | 0.20 mm        | hole to copper |
| PTH hole to copper, inner `[dru]`  | 0.25 mm        | inner clearance |
| SMD pad-to-pad `[dru]`             | 0.15 mm        | 6 mil (default) |
| NPTH min drill `[dru]`             | 0.50 mm        | min non-plated hole |
| Copper to routed edge (global)     | 0.20 mm        | trace to outline |
| Drill to edge `[dru]`              | 0.50 mm        | design margin |
| Silk line / text height            | 0.15 / 0.8 mm  | min char width / height |
| Mask dam / sliver (global)         | 0.10 mm        | soldermask dam |
| Blind/buried/microvias `[dru]`     | disabled       | HDI = separate service |

## Custom rules (.kicad_dru)

- PTH pad hole-to-hole >= 0.50 mm
- PTH annular ring >= 0.15 mm (6 mil)
- PTH hole to copper, inner layers >= 0.25 mm
- PTH hole to track >= 0.20 mm
- SMD pad-to-pad (different nets) >= 0.15 mm (6 mil)
- NPTH drill >= 0.50 mm
- No via in SMD pad (unfilled via-in-pad wicks solder)
- Blind/buried/microvias disallowed (standard FR-4 = through vias only)
- Drills >= 0.50 mm from board edge (margin)
- Silkscreen text >= 0.15 mm / 0.8 mm, silk layers only

## Track width presets

0.15 (6 mil, default-order min) - 0.2 (default) - 0.25 - 0.3 - 0.4 - 0.5 - 0.8 -
1.0 - 1.5 - 2.0 mm

Rough 1 oz current capacity (IPC-2221, 10 C rise):
0.15 mm ~ 0.5 A, 0.3 mm ~ 0.9 A, 0.5 mm ~ 1.3 A, 1.0 mm ~ 2.3 A, 2.0 mm ~ 4 A.
Inner layers are 1 oz too -- same capacity -- but keep high current on outer layers.

## Via presets

| Preset      | Pad/Drill   | Use                              |
|-------------|-------------|----------------------------------|
| 0.60 / 0.30 | **default / minimum** (default order) | general signal   |
| 0.80 / 0.40 | power       | ~0.7 A each, stitch in groups    |
| 1.20 / 0.60 | thermal / high current           |            |

Smaller vias (0.5/0.25, 0.45/0.2) need the non-default 0.15-0.25 mm hole option.

## Net classes

- Default 0.2/0.2 mm, via 0.6/0.3
- Signal_Fine 0.15/0.15 mm (6 mil, the default-order floor), via 0.6/0.3
- Power 0.5/0.2 mm, via 0.8/0.4
- HighCurrent 1.0/0.25 mm, via 1.2/0.6


Impedance classes (outer-layer microstrip over In1/GND, 0.1195 mm 2116, Dk 4.45), via 0.6/0.3:

- Diff_90R_USB 0.207/0.207 mm, gap 0.2
- Diff_100R 0.169/0.169 mm, gap 0.2
- RF_50R (single-ended) 0.205 mm, clearance 0.3

Widths are computed from **this** stackup (Hammerstad microstrip), not copied from
JLCPCB. Impedance control is a **paid** PCBWay option -- verify widths against their
impedance calculator with the stackup they assign.

## Ordering notes

- Everything above is the **default** quote -- no surcharge. To go finer you must
  change the order options (and possibly pay more):
  - Min track/space: 5/5, 4/4 mil; 3/3 mil on sample orders (>=3.5/3.5 mil bulk).
    Lower the global track/clearance + Signal_Fine class if you select these.
  - Min hole: 0.25, 0.2, 0.15 mm. Lower min hole + via drill if you select these.
  - Surface finish: lead-free HASL, ENIG, OSP, immersion tin/silver, etc.
  - Controlled impedance: paid; request the exact stackup and re-check widths.
- Blind/buried/micro vias need the HDI material option (>= 4 layers) -- disabled here.
- Copper 1 oz inner/outer standard; thickness 1.6 mm (0.4-3.2 mm available, +-10 %).

## Sources

- [PCBWay PCB Capabilities](https://www.pcbway.com/capabilities.html)
- [PCBWay Multi-layer laminated structure](https://www.pcbway.com/multi-layer-laminated-structure.html)
- PCBWay online quote default selections (2/4/6/8-layer), July 2026.
