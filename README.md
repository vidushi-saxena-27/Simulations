# Simulations
# Simulations

Two small Pygame physics simulations.

## `new.py` — Particle Bowl Simulation

Balls fall under gravity inside a circular bowl, bounce off the bowl wall,
and bounce off each other.

- **Physics**:gravity updates velocity,
  velocity then updates position at every frame, scaled by `dt` for
  frame-rate independence.
- **Wall collisions**: detects when a ball's center exceeds
  `BOWL_RADIUS - PARTICLE_RADIUS` from the bowl's center, snaps it back
  onto the boundary, and reflects the outward-moving component of its
  velocity (scaled by `WALL_RESTITUTION`).
- **Ball-ball collisions**: checks every unique pair for overlap
  (`distance < 2 * PARTICLE_RADIUS`), separates overlapping balls along
  the contact normal, and applies an equal-and-opposite impulse to
  approaching pairs (scaled by `RESTITUTION`), leaving separating pairs
  untouched.
- **Config**: ball count, radius, speed, gravity, and restitution are all
  top-of-file constants.

**Run:** `uv run new.py` (or `python new.py` with `pygame` + `numpy`
installed).

## `temp.py` — Falling Sand Simulation

A cellular-automaton sandbox where SAND falls and piles, WATER falls and
spreads, and WALL is immovable.

- **Grid**: a NumPy `uint8` array (`self._types`), one `Material` enum
  value per cell; `EMPTY = 0` so a fresh grid starts empty for free.
- **Update order**: rows processed bottom-up (so a grain can't fall twice
  in one tick) and columns shuffled per row (so falling doesn't bias
  left/right).
- **Movement rules**: each occupied cell tries its moves in priority
  order and takes the first empty landing spot —
  - SAND: down, then down-left/down-right (random side first)
  - WATER: same as sand, then left/right, so it pools flat
- **Rendering**: materials are mapped to RGB via a lookup table
  (`_COLORS[grid]`) and scaled up by `cell_size` for display.
- **Controls**: `1` sand, `2` water, `3` wall, `0`/`E` erase, `[`/`]`
  brush size, `C` clear, mouse-drag to paint.

**Run:** `uv run python assignment/temp.py` (or `python temp.py` with
`pygame` + `numpy` installed).
