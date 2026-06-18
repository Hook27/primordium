# Primordia

**A cabinet of emergence** — four self-contained toys where a handful of tiny rules become something that looks alive.

🌐 **Live:** [primordium.cryptofolio.nl](https://primordium.cryptofolio.nl/)

Each world is a single HTML file: no libraries, no build step, no network, no assets. Open one in any modern browser and it just runs. Together they are a small museum of the ways order makes itself — out of **forces**, out of **scent-trails**, and out of **alignment**.

## The four worlds

| World | Subtitle | Mechanism | What emerges |
|-------|----------|-----------|--------------|
| [Primordium](primordium.html) | particle life | **Forces** | Species pull and push through a secret matrix; cells, chasers and pulsing membranes appear. |
| [Mycelia](mycelia.html) | slime intelligence | **Stigmergy** | Blind crawlers follow a glowing scent and weave living networks of veins. |
| [Formica](formica.html) | ant colony | **Stigmergy** | A leaderless colony finds the shortest road on two evaporating pheromones. |
| [Sturnus](sturnus.html) | murmuration | **Alignment** | A flock where each bird watches its seven nearest neighbours and turns as one. |

## Three kinds of emergence

- **Forces** — particles act on each other directly through an asymmetric attraction/repulsion matrix. Structure is a balance of pulls. *(Primordium)*
- **Stigmergy** — agents never sense each other; they only read and write an environmental field that diffuses and evaporates. The trail is the memory. *(Mycelia, Formica)*
- **Alignment** — agents copy their neighbours' heading from moment to moment, with no field and no memory. Collective motion is the whole point. *(Sturnus)*

## Shared features

Every world carries the same kit:

- **Sliders** for live tuning, with sensible **presets**
- A 32-bit **seed** to save, share and reproduce a creature / map / flock
- Five **colour palettes**
- **Generative audio** — the simulation conducts an ambient pentatonic piece, driven by its own activity
- **Save PNG** of the artwork
- **Mouse interaction** (stir, feed, or send in a hawk) and an **about** panel
- Keyboard shortcuts: `space` pause · `r` randomize · `p` palette · `a` sound · `h` hide panel · `i` about

## Tech notes

- **Primordium** — O(n) particle simulation on a spatial hash, asymmetric force matrix.
- **Mycelia / Formica** — agents reading & depositing onto diffusing scent fields (separable box-blur + evaporation), rendered by tone-mapping the field.
- **Sturnus** — boids with **topological** neighbours (the *k* nearest, not a fixed radius) on a spatial hash; that is why the flock stays whole and scale-free.

Pure vanilla JavaScript and the Canvas / Web Audio APIs. Nothing else.

## Running locally

Just open any of the `.html` files directly in a browser — they need no server. To serve the whole collection:

```sh
python -m http.server      # then visit http://localhost:8000
```

---

Built with [Claude](https://claude.com/claude-code).
