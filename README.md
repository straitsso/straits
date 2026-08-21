# STRAITS

A single self-contained `index.html`: a 1920×1280 canvas artwork of a slowly rotating,
terraced isometric terrain — two landmasses split by a strait — floating above its own
contour-plan projection. Flat riso / screen-print aesthetic. No gradients, no images,
no libraries.

Open `index.html` in a browser. Nothing to build. `straits-wordmark.png` sits next to it
and is the only asset the pages load (plus the Space Mono webfont).

`portfolio.html` lists the Straits companies; `index.html` links to it from the info block
above the wordmark. It's a single centered screen on a desktop window; under 640px of
viewport height or 620px of width the type steps up and the page scrolls rather than
clipping rows out of the centered box. The roster is the `COMPANIES` array at the top of that page's script —
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

The piece is authored on a fixed 1920×1280 board; nothing inside it moves. The board and the
chrome (clock, info, wordmark) are two overlaid boards of that size, each placed by mapping one
anchor point of the board onto one point of the viewport: `translate(cx,cy) scale(k) translate(-ax,-ay)`
off a `0 0` origin. Every layout below is a choice of anchor, target and scale.

- **Desktop** — anchor the board center on the viewport center at `k = min(w/1920, h/1280)`.
  Chrome rides the same transform, so it scales with the art exactly as authored — except that
  the art board alone is raised `LIFT` (64) board pixels off that center line, on both layouts.
- **Small screens** (under 820px wide or 560px tall) — the chrome leaves the board and sits
  unscaled against the viewport, in a band top and bottom; `fit()` measures what those bands
  actually occupy (webfont, wrapping, `env(safe-area-inset-*)`) and gives the canvas the middle.
  The art then fits its own extent — at its widest rotation the two planes span roughly
  x 434…1487, y 300…1010, not the whole board — which is about 1.8× the naive whole-board fit.
- **Backing store** — sized to `k × devicePixelRatio` (clamped to 1…2.5), i.e. the device pixels
  the board actually covers, so it is never oversampled. Drawing stays in 1920×1280 coordinates
  via `setTransform(dpr,0,0,dpr,0,0)`. A 3× phone would otherwise allocate 5760×3840 (~88MB).
- **Viewport** — sized from `visualViewport` when present, refit on `resize`, `orientationchange`
  and `document.fonts.ready`.

Both pages hold the zoom: `maximum-scale=1,user-scalable=no` plus `touch-action` and
`overscroll-behavior:none` in CSS, and — because iOS ignores the meta — `preventDefault` on
`gesture*` and on multi-touch `touchmove`. Links keep `touch-action:manipulation` so taps stay
instant. Deliberate trade: pinch-zoom is off on both pages.
