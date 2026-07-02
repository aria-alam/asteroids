# Deflection Grid 🌍☄️

A planetary-defense strategy game in a single HTML file. You run Earth's
planetary defense office: asteroids on genuine Kepler-integrated collision
orbits keep coming, and you have a budget, three flavors of deflection
mission, and never quite enough lead time.

**Play it:** open `index.html` in any browser. No build, no dependencies.

## How to play

- Click a threat (on the map or in the sidebar) to open its mission panel.
- Each weapon shows an **ETA** and an honest **forecast** (STRONG / MARGINAL /
  WEAK) computed by actually simulating the mission against the asteroid's
  orbit — plus the predicted miss distance in lunar distances (LD).
- **Kinetic Impactor ($120M)** — DART-style. Cheap and weak; brilliant with a
  year of lead time, useless in a panic. 40% less effective against rubble piles.
- **Gravity Tractor ($220M)** — a continuous femto-tug. Needs the longest lead
  time of all, but it's gentle: the only option guaranteed safe on rubble piles.
- **Nuclear Standoff ($480M)** — moves mountains. Unless the mountain is a
  rubble pile, in which case there's a 45% chance you now have three mountains.
- Every mission has a failure chance. Space is hard.
- Deflected asteroids that pass Earth safely earn bounty funding. Impacts cost
  habitability; at 0% it's game over.
- `Space` pause · `1` normal speed · `2` fast-forward.

## The physics (the fun part)

- Everything moves under real 2-body heliocentric gravity, integrated with
  velocity-Verlet (`GM☉ = 4π² AU³/yr²`, so Earth orbits at 1 AU in exactly
  1 year at 2π AU/yr ≈ 29.8 km/s).
- Threats are spawned by choosing a future Earth-impact state and integrating
  **backward in time** — so every asteroid is a physically exact impactor
  until you perturb it. No scripted paths.
- Deflections are a small Δv applied along-track (the game auto-picks
  prograde vs retrograde by trial integration, which is how it's really
  chosen). Kinetic Δv scales as 1/mass, nuclear as 1/mass^⅓, and the
  tractor is a constant micro-acceleration — the same scalings as the
  real literature.
- The core real-world lesson falls straight out of the integrator: the same
  impactor that produces a 137 LD miss with 9 months of warning produces a
  1 LD miss — an impact — with 2 months of warning. **Early nudge beats
  late shove.**

### Honest simplifications

- 2D, and Earth's orbit is circular.
- Earth's collision radius is inflated (~0.012 AU) so you can see it;
  deflection Δv values are scaled up from reality (DART was mm/s) in
  proportion, preserving the lead-time trade-off that makes the real
  problem interesting.
- Interceptors fly direct intercepts rather than transfer orbits.
- Timescale: 1 game year ≈ 50 seconds at 1×.

## Development

The whole game is `index.html` (~900 lines, canvas + vanilla JS, WebAudio
synth for sound — zero assets, zero dependencies).
