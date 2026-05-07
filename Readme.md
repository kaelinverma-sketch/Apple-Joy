# Apple Joy – CAD Assembly

A parametric CAD assembly modelled entirely in Python using **build123d**. The project consists of four parts: Top, Bottom, Mating Part, and a Gear.

---

## Requirements

```bash
pip install build123d ocp_vscode
```

---

## File Structure

```
├── Apple_Joy_Top.py          # Top U-channel enclosure
├── Apple_Joy_Bottom.py       # Bottom U-channel enclosure (mating part)
├── Gear.py                   # Circular gear disc with boss and arc wedge
└── README.md                 # This file
```

---

## Parts Overview

---

### 1. Apple Joy Top (`Apple_Joy_Top.py`)

#### Overall Dimensions
| Axis | Range | Size |
|------|-------|------|
| X | −20.67 → 359.89 mm | **380.56 mm** |
| Y | 0 → 120 mm | **120 mm** |
| Z | 0 → 510 mm | **510 mm** |

#### Methodology

**Base Profile** — An 8-point polyline (`pts`) defining the asymmetric U-channel cross-section is sketched on the XY plane and extruded 510 mm in Z.

**Tab Extrusions** — Left and right rectangular tabs (12 × 258 mm) are added at the top flanges using `Mode.ADD` extrusions on tilted construction planes.

**Body Cuts** — Narrower rectangular pockets (11.39 × 331.76 mm, 9 mm deep) are subtracted inward from each tab to form slide channels.

**Floor Cut** — A large 314 × 380 mm rectangular pocket (95 mm deep) is subtracted from the inner floor to create the hollow U-channel interior.

**Edge Finishing** — Chamfers and fillets are applied by filtering edges using `filter_by(Axis.*)` and `filter_by_position()`:
- 25 mm chamfer on inner floor X-edges
- 16 mm fillets on inner Z-edges at Y=25, Y=55, and on the chamfer angled face
- 20 mm fillets on outer bottom corners
- 12 mm chamfers on front/back X-edges of top and bottom faces

**Cylinder Boss** — A Ø64.6 mm cylinder (86 mm tall) is extruded from Y=111 downward at X=169.33, Z=310, with a Ø35 mm through-hole 101 mm deep.

**Large Cylinder Cuts** — Two Ø200 mm × 26 mm cylindrical pockets cut into the floor at X=59.33 and X=279.33.

**Triangular Wedge** — A right-triangle prism (86 × 86 mm legs, 64 mm wide) is added adjacent to the cylinder boss, with a 20 mm fillet on its hypotenuse.

**Rectangle Boss** — A separate 90 × 32 mm rectangular body (64 mm tall) is built as an independent `BuildPart` with a matching Ø35 mm hole and 23 mm fillet on top edges.

**Lip Bodies** — Left and right 250 × 10 mm lip flanges are added at Y=120 with a 9 mm chamfer on their outer X-axis edges.

**Text Engraving** — Four text features engraved 6 mm deep on the bottom face (XZ plane, normal along Y):
- `"Apple  II"` — centered at X=169.33, Z=58
- `"Y"` — above the "A", offset +90 mm in X, +92 mm in Z
- `"X"` — above the "II", offset −90 mm in X, +92 mm in Z
- `"Joystick"` — centered at X=169.33, Z=450

**Export** — STEP file via `tkinter` file dialog.

---

### 2. Apple Joy Bottom (`Apple_Joy_Bottom.py`)

#### Overall Dimensions
| Axis | Range | Size |
|------|-------|------|
| X | −20.67 → 359.89 mm | **380.56 mm** |
| Y | 0 → 120 mm | **120 mm** |
| Z | 0 → 510 mm | **510 mm** |

#### Methodology

The Bottom part shares an **identical script** to the Top. Both are the same U-channel geometry and are assembled as a matched pair — one placed above the other. The geometry, features, edge finishing, cylinder boss, wedge, text engraving, and export methodology are all identical to the Top (see above).

The two parts mate along their Y=0 base faces to form the complete Apple Joy enclosure.

---

### 3. Gear (`Gear.py`)

#### Overall Dimensions
| Axis | Range | Size |
|------|-------|------|
| X/Y | −~90 → +~90 mm | **~180 mm** (tooth tip to tooth tip) |
| Z | −10 → +10 mm | **20 mm** |

#### Methodology

**Base Disc** — A Ø172.12 mm cylinder (20 mm tall) is created with a concentric Ø63.92 mm through-hole subtracted using `Mode.SUBTRACT`.

**Teeth** — The number of teeth is calculated as `floor(circumference / tooth_base)` = 64 teeth. Each tooth is an isosceles triangle prism (base 8.44 mm, sides 5.89 mm) built from a `Polyline` sketch and extruded 20 mm. Teeth are added outward from the disc face by iterating over all positions with a `−2.8°` rotational offset applied via `rotate(Axis.Z, ...)`.

**Rectangular Boss** — A 24 × 12 mm rectangular extrusion (20 mm tall) is added at the centre, with its inner edge aligned to the hole wall at Y=31.96 mm (shifted −10.9 mm toward origin).

**Arc Wedge** — A pie-slice wedge spanning exactly 3 tooth spacings (16.875°) is constructed using a `RadiusArc` at the tooth tip radius (~90.17 mm). The wedge is rotated −90° to align with the −Y axis, then trimmed by subtracting a Ø88 mm cylinder.

**Ring** — A separate concentric ring (Ø172.80 mm outer, Ø160 mm inner, 20 mm tall) is combined with the gear body using `Mode.ADD`.

**Final Assembly** — Gear + boss + arc wedge + ring are combined into a single body using sequential `add()` calls in a final `BuildPart` context.

**Export** — STEP file via `tkinter` file dialog with success/failure message boxes.

---

## Assembly Notes

- The **Top** and **Bottom** parts are identical and mate along their Y=0 base faces.
- The **Gear** is an independent rotating component centred at the origin.
- All parts are exported as `.step` files for use in any downstream CAD application.

---

## Usage

```bash
python Apple_Joy_Top.py
python Apple_Joy_Bottom.py
python Gear.py
```

Each script opens a file dialog to save the exported `.step` file and displays the model live in the OCP CAD Viewer.
