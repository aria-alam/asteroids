# AstroForge · Prospector 🔭☄️

A short, calm asteroid-**prospecting** game in a single HTML file. You're a
mission planner choosing AstroForge's next target from a field of newly
catalogued near-Earth asteroids. You know how hard each one is to *reach*, but
not what it's *worth* — that's what your limited telescope and radar time is
for. Observe wisely, then commit to one target and live with the call.

**Play it:** open `index.html` in any browser. No build, no dependencies.

## How to play

- Each dot on the chart is a candidate asteroid.
  - **Horizontal** = mission Δv to reach it (left is cheaper — the accessibility
    that actually gates a mission).
  - **Vertical** = estimated recoverable value. The **tall error bars are what
    you don't know yet.**
- Click a candidate to open its dossier: orbit, magnitude, and a probability
  distribution over its spectral type.
- You have **8 observations per survey**. Spend them to shrink uncertainty:
  - **Spectrometer** — resolves composition, i.e. the metal *grade*. This is how
    you spot the rare metallic jackpot.
  - **Radar / IR** — resolves size, i.e. the *mass*.
- **Commit** to one target. The truth is revealed and you're graded on how much
  recoverable value you captured versus the best possible pick in the field.
- A season is **3 surveys**; your banked value accumulates into a final rank.

The whole game teaches one real lesson: metal-rich (M-type) rocks are rare, and
a fortune you can't reach cheaply isn't a fortune. Value **and** accessibility
have to line up — and you're making the call under measurement uncertainty,
which is exactly what prospecting is.

## The physics (the credible part)

Real relations drive the numbers, not hand-waving:

- **Mission Δv** comes from the Tisserand encounter speed of each orbit:
  `U = vE · √(3 − 1/a − 2·√(a(1−e²))·cos i)` (a in AU, i in radians). That's the
  speed at which the asteroid meets Earth; Δv scales with it above an ~3.8 km/s
  floor to leave LEO and rendezvous.
- **Size** from the standard absolute-magnitude / albedo relation:
  `D[km] = 1329 / √p · 10^(−H/5)`.
- **Mass** from diameter and a taxonomy-based bulk density (metallic ≈ 5300,
  silicaceous ≈ 2700, carbonaceous ≈ 1400 kg/m³).
- **Spectral abundances** follow reality: S-types dominate, and the metallic
  M-types you're hunting are only ~5% of the field.

### Honest simplifications

- Recoverable-value grade (`$/kg` by type) is illustrative — the *ordering*
  (M ≫ X > Q > S > V > C) is what's real, not the absolute dollars.
- Orbits are treated as well-determined (Δv is certain); the uncertainty lives
  in composition and size, which is where real prospecting uncertainty actually
  concentrates.
- The accessibility discount `exp(−(Δv−3.8)/2.3)` is a smooth stand-in for a
  full mission cost model.

## Development

The whole game is `index.html` (canvas chart + vanilla JS, zero assets, zero
dependencies). Physics lives in the clearly commented `physics model` block.
