# N-Body Gravitational Simulator

> Real-time Newtonian gravity simulation — entirely in the browser, zero dependencies, zero frameworks.

![Physics](https://img.shields.io/badge/physics-Newtonian_Gravity-blue)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)
![Size](https://img.shields.io/badge/size-single_file-orange)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## Live Demo

👉 **[Launch Simulator](https://aarav10.github.io/N-Body-Gravity-Simulator/)**

---

## Overview

This project implements a full N-body gravitational simulation from scratch — no physics engines, no libraries, no WebGL abstractions. Every force calculation, integration step, and collision response is written by hand in vanilla JavaScript.

At its core, it solves the classical N-body problem: given N point masses, compute the trajectory of each body as it responds to the gravitational pull of every other body simultaneously. This is the same mathematical framework used by NASA's Jet Propulsion Laboratory to compute spacecraft trajectories and by astrophysicists to simulate galaxy formation.

$$F_{ij} = G \frac{m_i \, m_j}{r_{ij}^2 + \varepsilon^2}$$

The softening term ε² prevents force singularities when bodies pass close together — a technique borrowed directly from cosmological simulation literature.

---

## Features

**Physics Engine**
- O(n²) pairwise force summation computed every frame
- 4-substep Euler integration per frame for orbital stability
- Softened gravitational potential to handle close encounters
- Perfectly inelastic collisions — bodies merge conserving mass and linear momentum

**Rendering**
- Multi-layer radial gradient glow with additive blending (`globalCompositeOperation: lighter`)
- Alpha-composited trail persistence simulating photographic long exposure
- Real-time merge flash effects on collision
- Mass-dependent color mapping: small bodies are hued, massive ones go white-hot

**Presets**

| Preset | Description |
|--------|-------------|
| **Solar** | Central star with 6 planets in stable Keplerian orbits |
| **Binary** | Equal-mass binary star system with surrounding debris ring |
| **Galaxy** | 3-arm spiral galaxy seeded from a 3000-unit central black hole |
| **Chaos** | 20 randomly distributed massive bodies — unpredictable, violent |
| **Figure-8** | The Chenciner-Montgomery 3-body periodic solution (1993) |
| **Lagrange** | Star-planet system with trojan asteroid swarms at stable L4/L5 points |

**Controls**
- Click to spawn a body at rest
- Click and drag to spawn with initial velocity — drag vector maps directly to velocity
- Real-time sliders for simulation speed, gravitational constant G, and spawn mass
- Pause/resume and trail toggle

---

## Physics Background

### The N-Body Problem

The N-body problem has no general closed-form solution for N ≥ 3. Henri Poincaré proved this in 1887, and it was one of the founding results of chaos theory. This simulator uses direct numerical integration to approximate trajectories — the same approach used in real astrophysical research for small-N systems.

### Force Integration

Each timestep is split into 4 substeps. For each substep:
1. Compute acceleration on every body from the pairwise sum of gravitational forces
2. Update velocities: $v \mathrel{+}= a \cdot \Delta t$
3. Update positions: $x \mathrel{+}= v \cdot \Delta t$

### Collision Response

When two bodies overlap (distance < sum of radii), they undergo a perfectly inelastic collision:

$$m = m_1 + m_2 \qquad \vec{v} = \frac{m_1 \vec{v}_1 + m_2 \vec{v}_2}{m_1 + m_2}$$

Mass, position, and velocity are all conserved. The resulting body's radius scales as the cube root of its mass, consistent with constant-density spheres.

### The Figure-8 Solution

The Figure-8 preset uses the Chenciner-Montgomery solution — a remarkable periodic orbit where 3 equal masses chase each other around a figure-8 curve. It was proven to exist in 2000 using variational methods and validated numerically. Initial conditions are taken directly from the original paper.

---

## Architecture

```
nbody-simulator/
└── index.html        # Complete simulator — ~300 lines, zero build step
```

The entire project is a single self-contained HTML file. No npm, no bundler, no build pipeline. Open it locally or deploy it anywhere that serves static files.

---

## Roadmap

- [ ] Barnes-Hut octree — reduce force computation from O(n²) to O(n log n), enabling 1000+ body simulations
- [ ] Symplectic (leapfrog) integrator for true energy conservation over long timescales
- [ ] WebGL renderer for GPU-accelerated particle rendering at scale
- [ ] Real solar system mode using live ephemeris data from NASA Horizons API
- [ ] GIF/video export
- [ ] Center-of-mass reference frame lock

---

## Deploy Your Own

```bash
git clone https://github.com/Aarav10/nbody-simulator.git
cd nbody-simulator
# Open index.html in any browser — no server needed
```

To deploy on GitHub Pages: **Settings → Pages → Source: main branch**

---

## License

MIT — use it, fork it, learn from it.
