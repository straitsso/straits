# STRAITS

A single self-contained `index.html`: a 1920×1280 canvas artwork of a slowly rotating,
terraced isometric terrain — two landmasses split by a strait — floating above its own
contour-plan projection. Flat riso / screen-print aesthetic. No gradients, no images,
no libraries.

Open `index.html` in a browser. Nothing to build. `straits-wordmark.png` sits next to it
and is the only asset the pages load (plus the Space Mono webfont).

`portfolio.html` lists the Straits companies; `index.html` links to it from the info block
above the wordmark. The roster is the `COMPANIES` array at the top of that page's script —
`name`, `what`, and an optional `url` (omit it and the row renders unlinked, marked
"in progress").

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

Hidden in production. Open with `?debug=1` or the `h` key: spin on/off, seconds per revolution (12–140),
band step (5–15), stipple density (0.25–2), seed (1–40). Changing step / density / seed
rebuilds the geometry; the other two are free.

## Display

The piece is authored on a fixed 1920×1280 stage; nothing inside it moves. The stage is
scaled by `min(innerWidth/1920, innerHeight/1280)` about its center and centered in the
viewport on load and resize (a matching negative margin collapses the layout box to the
scaled size, otherwise the oversized box start-aligns instead of centering). The canvas
backing store is sized to devicePixelRatio with `setTransform(dpr,0,0,dpr,0,0)`, so all
drawing stays in 1920×1280 coordinates while staying crisp on hi-dpi displays.
