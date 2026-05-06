# Gear Disc Model

A parametric circular gear disc built using [build123d](https://github.com/gumyr/build123d) and visualised in the [OCP CAD Viewer](https://github.com/bernhard-42/vscode-ocp-cad-viewer) VS Code extension.

## Dependencies

```bash
pip install build123d ocp-vscode
```

## Overview

The model assembles four components into a single solid body — a toothed gear disc, a rectangular boss, an arc wedge, and an outer ring.

## Key Dimensions

| Feature | Value |
|---------|-------|
| Outer diameter | 172.12 mm |
| Height | 20 mm |
| Central through-hole | Ø 63.92 mm |
| Tooth base | 8.44 mm |
| Tooth side | 5.89 mm |
| Boss | 24 × 12 mm, 20 mm tall |
| Outer ring | Ø 172.80 mm / Ø 160 mm inner |

## Construction

**Gear Disc** — A solid cylinder with a central through-hole. Isosceles triangular teeth are extruded outward from the outer curved face, evenly distributed around the full circumference and offset by -2.8°.

**Rectangular Boss** — A 24 × 12 mm rectangle extruded 20mm, centred at the origin with its inner edge aligned to the hole wall.

**Arc Wedge** — A pie-slice wedge spanning exactly 3 tooth spacings, with its curved face tracing the tooth tip radius. A Ø88mm cylinder is subtracted to hollow the inner curve. The wedge is rotated to align with the -Y axis.

**Outer Ring** — A thin annular ring (Ø172.80mm outer, Ø160mm inner, 20mm tall) fused around the gear.

All four components are boolean-unioned into a single exported solid.

## Coordinate System

| Axis | Direction |
|------|-----------|
| X/Y | Radial plane |
| Z | Height |
