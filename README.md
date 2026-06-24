# N-Body Gravitational Simulator

> A real-time Newtonian gravity simulation that runs in the browser from a single HTML file, with no dependencies and no build step.

![Physics](https://img.shields.io/badge/physics-Newtonian_Gravity-blue)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)
![Size](https://img.shields.io/badge/size-single_file-orange)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

## Live demo

**[Launch the simulator](https://aarav10.github.io/N-Body-Gravity-Simulator/)**

## Overview

This is a full N-body gravity simulation written from scratch in vanilla JavaScript. There's no physics engine and no libraries doing the work underneath. The force calculations, the integration, and the collision handling are all hand-written.

It solves the classical N-body problem: given N point masses, work out how each one moves as it's pulled by every other body at the same time. That's the same problem behind spacecraft trajectory work and galaxy formation models, just at a much smaller scale here.

$$F_{ij} = G \frac{m_i \, m_j}{r_{ij}^2 + \varepsilon^2}$$

The softening term ε² keeps the force from blowing up when two bodies pass very close to each other. It's a standard trick borrowed from cosmological simulations.

## Features

**Physics engine**
- O(n²) pairwise force summation, computed every frame
- 4-substep Euler integration per frame to keep orbits stable
- Softened gravitational potential so close encounters don't break the math
- Perfectly inelastic collisions: bodies merge, conserving mass and linear momentum

**Rendering**
- Multi-layer radial gradient glow with additive blending (`globalCompositeOperation: lighter`)
- Alpha-composited trail persistence that looks like a long-exposure photo
- Merge flash effects when two bodies collide
- Color mapped to mass: small bodies are hued, massive ones go white-hot

**Presets**

| Preset | Description |
|--------|-------------|
| **Solar** | Central star with 6 planets in stable Keplerian orbits |
| **Binary** | Equal-mass binary star system with a surrounding debris ring |
| **Galaxy** | 3-arm spiral galaxy seeded from a 3000-unit central black hole |
| **Chaos** | 20 randomly placed massive bodies, unpredictable and violent |
| **Figure-8** | The Chenciner-Montgomery 3-body periodic solution (1993) |
| **Lagrange** | Star-planet system with trojan asteroid swarms at the L4/L5 points |

**Controls**
- Click to spawn a body at rest
- Click and drag to spawn with velocity. The drag vector maps directly to the body's velocity.
- Sliders for simulation speed, the gravitational constant G, and spawn mass
- Pause/resume and a trail toggle

## Physics background

### The N-body problem

The N-body problem has no general closed-form solution for N ≥ 3. Henri Poincaré proved this in 1887, and it became one of the founding results of chaos theory. This simulator approximates the trajectories by integrating them numerically, which is what real astrophysical work does for small-N systems too.

### Force integration

Each timestep is split into 4 substeps. For each substep:
1. Compute the acceleration on every body from the pairwise sum of gravitational forces
2. Update velocities: $v \mathrel{+}= a \cdot \Delta t$
3. Update positions: $x \mathrel{+}= v \cdot \Delta t$

### Collision response

When two bodies overlap (distance < sum of radii), they undergo a perfectly inelastic collision:

$$m = m_1 + m_2 \qquad \vec{v} = \frac{m_1 \vec{v}_1 + m_2 \vec{v}_2}{m_1 + m_2}$$

Mass, position, and velocity are all conserved. The merged body's radius scales as the cube root of its mass, which is what you'd get for a constant-density sphere.

### The Figure-8 solution

The Figure-8 preset uses the Chenciner-Montgomery solution, a periodic orbit where 3 equal masses chase each other around a figure-8 curve. It was proven to exist in 2000 using variational methods and then checked numerically. The initial conditions here come straight from the original paper.

## Architecture

```
nbody-simulator/
└── index.html        # The whole simulator, ~300 lines, no build step
```

The entire project is one self-contained HTML file. It needs no build step and nothing to install. Open it locally or drop it on any host that serves static files.

## Roadmap

- [ ] Barnes-Hut octree to bring force computation from O(n²) down to O(n log n), so 1000+ bodies become practical
- [ ] Symplectic (leapfrog) integrator for real energy conservation over long runs
- [ ] WebGL renderer for GPU-accelerated particles at scale
- [ ] Real solar system mode using live ephemeris data from the NASA Horizons API
- [ ] GIF/video export
- [ ] Center-of-mass reference frame lock

## Deploy your own

```bash
git clone https://github.com/Aarav10/N-Body-Gravity-Simulator.git
cd N-Body-Gravity-Simulator
# Open index.html in any browser. No server needed.
```

To put it on GitHub Pages: Settings → Pages → Source: main branch.

## License

MIT licensed. Fork it and do whatever you like with it.
