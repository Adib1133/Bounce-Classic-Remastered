# Bounce Classic — HTML Replica

A single-file, browser-based recreation of the classic Nokia "Bounce" phone
game (2001). Everything — rendering, physics, level data, and audio — lives
in one `index.html` with no build step and no external dependencies beyond
the browser itself.

## Running it

Open `index.html` directly in any modern browser. No server, no build
tools, and no installation are required.

## Controls

| Action        | Keys                                   |
|---------------|-----------------------------------------|
| Move left     | Arrow Left / A / Numpad 4               |
| Move right    | Arrow Right / D / Numpad 6              |
| Jump / swim   | Arrow Up / W / Space / Numpad 5 or 2    |
| Dive down     | Arrow Down / S / Numpad 8               |

Holding the jump key while airborne sustains a higher jump (variable jump
height), matching the feel of the original game's bounce mechanic.

## Levels

The game ships with the original 11 levels from the base Nokia release.
Ring counts, layouts, moving hazards, and level geometry are reproduced
from the original level data. Level 3 begins with the ball already
inflated (matching the original game's design).

## Core mechanics

- **Rolling and jumping** — movement uses digital (snap-to-speed)
  left/right controls rather than a gradual acceleration ramp, matching
  the original phone game's binary d-pad input. Momentum carries over
  while airborne or underwater so a jump arc can still be steered.
- **Ball size** — the ball can be small or big. Inflator/deflator pads on
  the map toggle between the two sizes; some levels start big.
- **Water** — the small ball always sinks in water; the big ball is
  always buoyant and floats to the surface. This is a fixed, size-based
  rule with no manual swim override.
- **Slopes** — eight distinct ramp tiles are supported: two steep
  ceiling overhangs, two steep 45-degree floor ramps, and four shallow
  ramp tiles that pair up across two tiles to form a gentle staircase
  slope. Each tile has its own exact collision geometry matching what is
  drawn, so the ball rolls smoothly across ramp-to-wall transitions
  without snagging.
- **Rings** — collectible rings (small single-tile and large 2x2-tile
  variants) are always passable regardless of ball size. Once collected,
  a ring stays on the map and turns from gold to grey rather than
  disappearing — it will not re-award points on a second pass.
- **Hazards** — static wall spikes are rendered as solid, riveted metal
  plates with crystalline spikes (full-tile hitbox matching the visual).
  Rotating hazards are rendered as spinning serrated buzzsaw blades,
  visually distinct from the wall spikes.
- **Camera** — the view keeps the ball anchored toward the left third of
  the screen (vertically centered) rather than dead-center, showing more
  of the level ahead. The camera is clamped to the level bounds from the
  moment a level loads, so there's no snap/jump on entry.

## Collision system notes

Solid-tile collision resolves along whichever axis (horizontal or
vertical) has the smaller overlap, rather than pushing the ball toward
the nearest point on a tile's box. This avoids the classic tile-collision
bug where a diagonal push at the seam between two adjacent tiles (for
example, where a ramp meets a full-height wall) causes the ball to
appear to hit an invisible wall mid-slope.

Rubber tiles (and rubber-tinted slopes) reflect velocity with a rebound
multiplier; every other surface simply zeroes the velocity component
along the axis of contact, so normal ground does not cause the ball to
bounce on landing.

## File structure

Everything is contained in `index.html`:

- Inline `<style>` for layout and HUD.
- Inline `<script>` containing:
  - Tile and entity constant definitions.
  - The `LEVELS_DATA` array (11 levels, each with its tile matrix and
    moving-hazard entity list).
  - The game loop, physics, collision, and rendering code.
  - A small WebAudio-based sound effect system.

There are no external assets — all graphics are drawn with Canvas 2D
primitives (gradients, paths, and shapes) at render time.
