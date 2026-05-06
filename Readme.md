# Apple Joy Top — CAD Model

A parametric 3D model built using [build123d](https://github.com/gumyr/build123d) and visualised in the [OCP CAD Viewer](https://github.com/bernhard-42/vscode-ocp-cad-viewer) VS Code extension.

## Dependencies

```bash
pip install build123d ocp-vscode
```

## Running

```bash
python "Apple_Joy top.py"
```

Open the OCP CAD Viewer panel in VS Code to see the result.

---

## Overview

The model is a structural U-channel assembly built entirely in Python using parametric CAD. It consists of two boolean-unioned bodies displayed as a single solid.

## Construction Steps

**U-Channel Base** — An 8-point cross-section profile (380mm wide, 120mm tall) is extruded 510mm along Z to form the main structural body with tapered outer walls and a raised inner floor.

**Cuts & Pockets** — Rectangular cuts are made on the top wall faces and a large floor pocket (314mm × 380mm, 95mm deep) is carved into the inner floor region. Chamfers are applied to key pocket edges.

**Cylinder & Through Hole** — A solid cylinder (⌀64.6mm, 86mm tall) sits on the inner floor. A 35mm through hole passes through the entire assembly in both Y directions.

**Triangular Wedge** — A right-angle wedge (86mm × 86mm, 64mm thick) is fused to the inner floor with a 15mm fillet on its hypotenuse edges.

**Rectangular Body** — A separate body (90mm × 32mm × 64mm) is placed along the cylinder's curved face, filleted and boolean-unioned with the main body.

## Coordinate System

| Axis | Direction |
|------|-----------|
| X | Width |
| Y | Height |
| Z | Depth |
