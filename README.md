# Primordia

**A cabinet of emergence** — eight self-contained toys where a handful of tiny rules become something that looks alive.

🌐 **Live:** [primordium.cryptofolio.nl](https://primordium.cryptofolio.nl/)

Each world is a single HTML file: no libraries, no build step, no network, no assets. Open one in any modern browser and it just runs. Together they are a small museum of the ways order makes itself — out of **forces**, out of **scent-trails**, and out of **alignment**.

## The eight worlds

| World | Subtitle | Mechanism | What emerges |
|-------|----------|-----------|--------------|
| [Primordium](primordium.html) | particle life | **Forces** | Species pull and push through a secret matrix; cells, chasers and pulsing membranes appear. |
| [Primordium II](primordium2.html) | gpu particle life | **Forces** | The same law on the GPU: a hundred thousand particles surf species density fields, forming storms, membranes and living tissue. |
| [Lenia](lenia.html) | continuous life | **Growth** | No particles: a smooth field under one bell-curve rule breeds gliding Orbium organisms, coral reefs and breathing rings. |
| [Mycelia](mycelia.html) | slime intelligence | **Stigmergy** | Blind crawlers follow a glowing scent and weave living networks of veins. |
| [Formica](formica.html) | ant colony | **Stigmergy** | A leaderless colony finds the shortest road on two evaporating pheromones. |
| [Sturnus](sturnus.html) | murmuration | **Alignment** | A flock where each bird watches its seven nearest neighbours and turns as one. |
| [Vivarium](vivarium.html) | xenobiology | **Forces · game** | Primordium as a hunt: the simulation is measured, classified into twelve archetypes and collected. |
| [Navis](navis.html) | particle life voyage | **Forces · voyage** | Primordium without edges: follow a creature, tow it by hand, and rewind the last minute exactly. |

## Four kinds of emergence

- **Forces** — particles act on each other directly through an asymmetric attraction/repulsion matrix. Structure is a balance of pulls. *(Primordium, Primordium II, Navis)*
- **Growth** — no agents at all: a continuous field rises and falls under a local growth rule (a ring-shaped neighbourhood fed through a bell curve). Creatures are standing waves that keep themselves alive. *(Lenia)*
- **Stigmergy** — agents never sense each other; they only read and write an environmental field that diffuses and evaporates. The trail is the memory. *(Mycelia, Formica)*
- **Alignment** — agents copy their neighbours' heading from moment to moment, with no field and no memory. Collective motion is the whole point. *(Sturnus)*

## Vivarium: the measured world

Vivarium is the odd one out — a game rather than a toy, and the only world with a **measurement
module**. It runs Primordium's exact force law in a fixed 640×440 tank of 570 particles, lets the
swarm settle, and then measures what emerged: how many separate bodies there are, how tightly they
hold together, how elongated they are, how much angular momentum they carry, how far matter actually
travels and how straight, how strongly the species segregate, and how *n*-fold symmetric the largest
body is. Those numbers decide the archetype.

The rarity of each archetype is not invented. A headless copy of the same simulation was run over
**4200 random seeds**, and the thresholds sit on percentiles of that population — so when the game
calls a Rotor *rare*, it means roughly four seeds in a hundred.

Because particle life is chaotic, per-seed reproducibility demands bit-identical arithmetic: the tank
never stretches to fit the window (it is letterboxed instead), and the damping constant uses `sqrt`
rather than `pow`, since IEEE754 specifies the former exactly. A seed therefore yields the same
creature on every machine.

Two modes: **safari** (scan seed space, classify, catch into a bestiary held in `localStorage`) and
the **breeding lab** (six offspring per generation, pick the one closest to a hidden target organism,
scored on the same ten measured traits).

## Navis: the travelling world

Navis takes Primordium's force law, sliders and matrix out of the box. The world is a torus a little
larger than the widest view, always **centred on the camera**: whatever drifts past its far edge comes
back on the opposite side, out of sight, as fresh primordial soup. The particle count never grows, yet
there is always new soup ahead.

- **Follow** — click a form and the camera keeps it in view. The form is re-identified four times a
  second (of all the clusters its old members now belong to, the one holding most of them); swallowed
  by a larger mass, it is tracked by its own particles inside it.
- **Tow** — drag a form and every particle in it gets the same velocity change, so its shape and its own
  motion stay intact. In Push / Pull mode, dragging stirs the swarm as in Primordium.
- **Rewind** — the last 60 seconds are kept as exact snapshots (positions, velocities, species, the
  random generator, the camera and the rules). Scrub back, press play, and an untouched world replays
  bit for bit; touch anything and history takes another course. Even *Evolve* draws its mutations from
  the world's own generator, so it replays too.
- **A calm camera** — it glides instead of copying every jolt: a calm zone in the middle, the form's
  velocity averaged over 0.4 s, speed changes eased and ever firmer near the limit. Tuned on recorded
  flights of the fastest forms, it cuts camera jerk 10–50× for ordinary fast forms and 3–13× for the
  wildest ones, against tight following.

## Shared features

Every world except Vivarium carries the same kit:

- **Sliders** for live tuning, with sensible **presets**
- A 32-bit **seed** to save, share and reproduce a creature / map / flock
- Five **colour palettes**
- **Generative audio** — the simulation conducts an ambient pentatonic piece, driven by its own activity
- **Save PNG** of the artwork
- **Mouse interaction** (stir, feed, or send in a hawk) and an **about** panel
- Keyboard shortcuts: `space` pause · `r` randomize · `p` palette · `a` sound · `h` hide panel · `i` about

## Tech notes

- **Primordium** — O(n) particle simulation on a spatial hash, asymmetric force matrix.
- **Primordium II** — the same force law in its continuum limit, entirely on the GPU (WebGL2): each species deposits a density field, the fields are blurred to the interaction radius, and every particle surfs their gradients in a fragment shader. Rendered with HDR trails and a two-level bloom pyramid. Scales to hundreds of thousands of particles.
- **Lenia** — Bert Chan's continuous cellular automaton on the GPU: a polar-sampled ring kernel convolved in a fragment shader, a Gaussian growth function, bicubic upscaling and HDR bloom. The Orbium preset plants the authentic published creature; the dish reseeds itself after extinctions.
- **Mycelia / Formica** — agents reading & depositing onto diffusing scent fields (separable box-blur + evaporation), rendered by tone-mapping the field.
- **Vivarium** — Primordium's simulation plus a measurement layer: union-find clustering over the
  spatial hash (sharing one neighbour sweep with the species-mixing metric), circular-mean centroids
  for bodies straddling the toroidal seam, covariance eigenvalues for elongation, and *n*-fold
  symmetry computed by raising each particle's unit vector to the *n*-th power through an
  angle-addition recurrence — no `atan2`, `sin` or `cos` in the hot loop.
- **Navis** — Primordium's step on a camera-centred torus with renewal at the seam, a ring of 600
  snapshots for exact rewind (interpolated for smooth scrubbing), fixed 60 steps/s drawn in between
  steps for even motion, and BFS over the spatial hash to find and re-identify forms.
- **Sturnus** — boids with **topological** neighbours (the *k* nearest, not a fixed radius) on a spatial hash; that is why the flock stays whole and scale-free.

Pure vanilla JavaScript and the Canvas / WebGL2 / Web Audio APIs. Nothing else.

## Running locally

Just open any of the `.html` files directly in a browser — they need no server. To serve the whole collection:

```sh
python -m http.server      # then visit http://localhost:8000
```

---

Built with [Claude](https://claude.com/claude-code).
