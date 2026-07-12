# JLCPCB 8-Layer FR-4 KiCad Template

JLCPCB 8-layer, 1.6 mm, JLC08161H-3313 order-flow default stackup. Material is
Nan Ya NP-155F (TG150) — 8-layer runs on the high-precision line, not the
TG135 laminate used for 1-6 layer standard. Free via-in-pad (POFV) included.

**Stackup verified from the JLCPCB order flow** (Advanced PCB, 8 layers,
"No requirement" = JLC08161H default). Uses 1080 prepreg internally, not 7628.

## Stackup: JLC08161H default (1.6 mm)

| Layer          | Material     | Thickness  | Er   |
|----------------|--------------|-----------:|------|
| F.Cu           | Cu 1 oz      | 0.0350 mm  |      |
| Prepreg        | 2116         | 0.1164 mm  | 4.16 |
| In1.Cu (GND1)  | Cu 0.5 oz    | 0.0152 mm  |      |
| Core           | FR-4         | 0.3000 mm  | 4.6  |
| In2.Cu (SIG)   | Cu 0.5 oz    | 0.0152 mm  |      |
| Prepreg        | 1080 x2      | 0.1528 mm  | 4.1  |
| In3.Cu (PWR)   | Cu 0.5 oz    | 0.0152 mm  |      |
| Core           | FR-4         | 0.3000 mm  | 4.6  |
| In4.Cu (GND2)  | Cu 0.5 oz    | 0.0152 mm  |      |
| Prepreg        | 1080 x2      | 0.1528 mm  | 4.1  |
| In5.Cu (SIG)   | Cu 0.5 oz    | 0.0152 mm  |      |
| Core           | FR-4         | 0.3000 mm  | 4.6  |
| In6.Cu (GND3)  | Cu 0.5 oz    | 0.0152 mm  |      |
| Prepreg        | 2116         | 0.1164 mm  | 4.16 |
| B.Cu           | Cu 1 oz      | 0.0350 mm  |      |

Verified from the order flow (JLC08161H, "No requirement"). Material NP-155F
(TG150), free ENIG and POFV. Note the outer prepreg is 2116 (0.1164 mm), not
3313 -- so the outer-layer impedance geometries below differ from the 6-layer
template.


## Global DRC constraints

Identical multilayer envelope to the 4L/6L templates. Value = template setting,
spec row = JLCPCB capabilities-page label, `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set        | Value    | JLCPCB spec row                          | Spec value |
|-------------------------------|----------|------------------------------------------|------------|
| Min track width (global)      | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
| Min clearance (global)        | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
| Min via diameter (global)     | 0.45 mm  | Min. Via hole size/diameter (multilayer) | 0.25 mm    |
| Min via drill (global)        | 0.20 mm  | Min. Via hole size/diameter (multilayer) | 0.15 mm    |
| Min via annular ring (global) | 0.125 mm | derived (dia-drill)/2                     | --         |
| PTH annular ring `[dru]`      | 0.15 mm  | PTH annular ring (multi 1oz, abs. min)   | 0.15 mm    |
| Via hole-to-hole (global)     | 0.20 mm  | Via Hole-to-Hole Spacing                 | 0.20 mm    |
| PTH pad hole-to-hole `[dru]`  | 0.45 mm  | Pad Hole-to-Hole Spacing                 | 0.45 mm    |
| PTH hole to track `[dru]`     | 0.28 mm  | PTH to Track                             | 0.28 mm    |
| PTH hole to copper, inner `[dru]` | 0.30 mm | Inner layer PTH pad hole to copper     | 0.30 mm    |
| Via hole to copper (global)   | 0.20 mm  | Inner layer via hole to copper clearance | 0.20 mm    |
| SMD pad-to-pad `[dru]`        | 0.15 mm  | SMD pad to pad clearance (diff. nets)    | 0.15 mm    |
| NPTH min drill `[dru]`        | 0.50 mm  | Min. Non-plated holes                    | 0.50 mm    |
| Copper to routed edge (global)| 0.20 mm  | Routed                                   | 0.20 mm    |
| Drill to edge `[dru]`         | 0.50 mm  | design margin (spec: copper 0.2 mm)      | --         |
| Silk line / text (global+dru) | 0.15 / 1.0 mm | Minimum Line Width / Minimum text height | 0.15 / 1.0 mm |
| Pad to silkscreen (global)    | 0.15 mm  | Pad To Silkscreen                        | 0.15 mm    |
| Mask expansion (board)        | 1:1 (0)  | Soldermask Expansion                     | 1:1        |
| Mask bridge / sliver (global) | 0.10 mm  | Soldermask bridge (1oz colour)           | 0.10 mm    |
| Blind/buried/microvias        | disabled | Blind/Buried Vias                        | Not supported |

## Via-in-pad

POFV (resin-filled, copper-capped) is free on 8-layer. BGA via-in-pad fanout
allowed; the .kicad_dru does not block vias in SMD pads. Select "Epoxy Filled
& Capped" via covering when ordering.

Aspect ratio at 0.2 mm drill / 1.6 mm board = 8:1 — OK, but don't combine
0.2 mm drills with 2.0 mm thickness.

## Net classes

Harmonized classes (JLCPCB-referenced, clamped to this vendor's floor):

- Default 0.15/0.15 mm, via 0.6/0.3
- Signal_Fine 0.09/0.09 mm (at the manufacturing floor)
- Power 0.5/0.2 mm, via 0.8/0.4
- HighCurrent 1.0/0.25 mm, via 1.2/0.6
- Impedance (calculator-verified): Diff_90R_USB, Diff_100R, RF_50R


## Order defaults (quote page)

- Surface finish: ENIG (only option for 6+ layers besides costly HASL).
- Via covering: default Plugged -- select "Epoxy Filled & Capped" (or copper
  paste for thermal vias) when using via-in-pad.
- Min via selector: default 0.3 mm/(0.4/0.45). Drills < 0.3 mm require the
  matching option; no surcharge only with via diameter >= 0.45.
