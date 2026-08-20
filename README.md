# STRAITS

A single self-contained `index.html`: a 1920×1280 canvas artwork of a slowly rotating,
terraced isometric terrain — two landmasses split by a strait — floating above its own
contour-plan projection. Flat riso / screen-print aesthetic. No gradients, no images,
no libraries.

Open `index.html` in a browser. Nothing to build.

## How it works

- **Height field** — an analytic `field(x, y)` over the unit square: a curved channel
  (wide at the ends, pinched mid-span) returns 0 for water; two rotated ellipses with
  ragged, harmonic coastlines and ridged noise supply the land.
- **Geometry** — sampled once on a 92×68 grid: run-length terrace strips per level
  (cached row-wise *and* column-wise), marching-squares contour segments, and seeded
  stipple points banded to the top of each level.
- **Projection** — unit coords are rotated about (0.5, 0.5) each frame, then mapped
  through a fixed isometric basis onto two origins: the terrain plane and, 257px below,
  the plan plane. Rotation is therefore free — geometry is never rebuilt to spin.
- **Terraces** — per level the renderer picks row or column strips by which long-edge
  direction projects flatter on screen, then extrudes the lower long edge straight down
  as a cliff wall. Picking the wrong axis produces a picket-fence artifact.

## Controls

Bottom-right panel (press `h` to hide): spin on/off, seconds per revolution (12–140),
band step (5–15), stipple density (0.25–2), seed (1–40). Changing step / density / seed
rebuilds the geometry; the other two are free.
