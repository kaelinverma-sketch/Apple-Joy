# Apple Joy – Mating Part

## Overview
This repository contains the build123d Python script for the **Apple Joy Top Mating Part** — a precision-modelled enclosure component designed to mate with the Apple Joy U-channel top assembly.

## Methodology

### Toolchain
- **build123d** — parametric CAD modelling in Python
- **OCP CAD Viewer** (`ocp_vscode`) — for live 3D preview
- **STEP export** — for downstream CAD/CAM use

### Modelling Approach

1. **Base Profile**
   The main U-channel cross-section is defined as an 8-point polyline (`pts`) and extruded 510mm along the Z axis. All key dimensions are stored as named constants (`DEPTH`, `CUT_Y`, `FLOOR_Y`, etc.) at the top of the script for easy modification.

2. **Feature Operations**
   Features are applied sequentially using `Mode.ADD` and `Mode.SUBTRACT` extrusions on construction planes (`Plane` with explicit `x_dir` and `z_dir`). This includes tab extrusions, body cuts, floor cuts, cylinder bosses, through-holes, and a triangular wedge.

3. **Edge Finishing**
   Chamfers and fillets are applied by filtering edges using `filter_by(Axis.*)` and `filter_by_position()` with precise Y/X position tolerances. This avoids hardcoding edge indices which are unstable across geometry changes.

4. **Separate Bodies**
   The rectangle boss and lip bodies are built as independent `BuildPart` contexts and combined with the main body using Boolean addition (`+`) before mirroring.

5. **Text Engraving**
   Engraved text ("Apple II", "Y", "X", "Joystick") is created as standalone `BuildPart` solids using `Text()` sketches on positioned planes, then subtracted from the combined body.

6. **Mirror & Origin Shift**
   The combined body is mirrored about the XZ plane (`Plane.XZ`) to produce the mating orientation. The origin is then translated to the top corner of the model using `mating_part.moved(Location(Vector(20.67, 0, -510)))`.

7. **Export**
   The final part is exported as a STEP file via a `tkinter` file dialog for use in any downstream CAD application.

## File Structure
```
├── Apple_Joy_top_mating.py   # Main build123d script
└── README.md                 # This file
```

## Requirements
```
pip install build123d ocp_vscode
```
A file dialog will appear to save the exported `.step` file. The model is also displayed live in the OCP CAD Viewer.
