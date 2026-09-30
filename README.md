# Same Potential, Different Fields: The Charged Ring and Spherical Shell

A short note and an interactive demo on a classic electrostatics puzzle. Compare two cases, both with total charge $q$:

- a point charge $q$, observed at distance $R$;
- a uniformly charged ring of radius $R$, observed at its center.

The electric potential is the same in both cases:

$$V = \frac{q}{4\pi\varepsilon_0 R}$$

The electric fields are completely different. Why doesn't the equal value of $V$ tell you anything about $\mathbf{E}$? And what do the potential and field of the ring actually look like away from the center?

**[Open the interactive demo](index.html)** · **[Read the note (PDF)](https://lshlj82.github.io/circular-and-spherical-charge-electric-potential-and-field/charged_ring.pdf)**

If GitHub Pages is enabled, the demo is served at the repository's Pages URL.

## Motivation

This project was inspired by a sudden realization of the lecturer, **Sang Hoon Lee**, during his general physics lecture. He noticed that the potential at the center of a uniformly charged ring equals the potential at distance $R$ from a point charge. Shortly after the lecture came a second realization: this equality of values alone does not provide enough information to derive the ring's electric field.

That second point is worth making explicit for students. The potential is a scalar, and its value at a point depends only on how far away the charges are. The field is a vector and a gradient, so it depends on how the potential varies around the point. It's natural to think that matching potentials should mean matching physics, and the ring is a clean counterexample. Following the thought further leads to three results:

- the full potential and field profiles of the ring;
- the saddle point at its center, and what that means for the stability of a test charge (Earnshaw's theorem);
- a comparison with the spherical shell, whose interior field vanishes everywhere, not just at the center.

The note works through the mathematics, and the demo lets you see and test each result directly.

## Contents

| File | Description |
|---|---|
| `index.html` | Self-contained interactive demo. Open it in any modern browser; no build step or server is needed. |
| `charged_ring.tex` | LaTeX source of the note, with TikZ/pgfplots figures. |
| `charged_ring.pdf` | Compiled version of the note ([view online](https://lshlj82.github.io/circular-and-spherical-charge-electric-potential-and-field/charged_ring.pdf)). |

## The physics in brief

**Why the potentials match.** Potential is a scalar sum,

$$V = \frac{1}{4\pi\varepsilon_0}\int\frac{dq}{s},$$

where $s$ is the distance from each charge element to the observation point. At the ring's center every element has $s = R$, so only the total charge matters.

**Why that says nothing about the field.** The field is $\mathbf{E} = -\nabla V$, which depends on how $V$ *changes* near a point. A single value can't tell you that. At the center of the ring, $\mathbf{E} = 0$ by symmetry. At distance $R$ from a point charge, $|\mathbf{E}| = q/4\pi\varepsilon_0 R^2$.

**The ring's profiles.**
- **Along the axis,** the results are elementary: $V(z) \propto 1/\sqrt{R^2+z^2}$, and $E_z$ peaks at $z = R/\sqrt{2}$.
- **In the plane,** $V$ and $E_\rho$ require complete elliptic integrals $K(k)$ and $E(k)$ with $k^2 = 4R\rho/(R+\rho)^2$.

**A saddle point.** Near the ring's center,

$$V \approx \frac{q}{4\pi\varepsilon_0 R}\left(1 + \frac{x^2 + y^2 - 2z^2}{4R^2}\right).$$

$V$ rises in the plane of the ring and falls along its axis. A test charge at the center is therefore in unstable equilibrium whatever its sign, as Earnshaw's theorem requires. If a constraint removes the unstable direction, the charge oscillates with $\omega_z^2 = 2\omega_\rho^2$, which is a direct consequence of Laplace's equation.

**The spherical shell for comparison.** A shell of the same charge and radius gives the same central potential. For the shell, though, $V$ is constant throughout the interior and $\mathbf{E} = 0$ everywhere inside, not just at the center. Newton's cone argument explains why: patch area grows like $s^2$, which exactly offsets the $1/s^2$ fall-off of the field. A one-dimensional ring can't provide that compensation.

## Using the demo

- **Charge distribution:** switch between the point charge, the ring and the spherical shell.
- **Views:**
  - The **3D view** (default) shows the $xz$ and $xy$ potential slices together, with equipotentials and field arrows. Drag to rotate.
  - The **side ($xz$) and top ($xy$) slices** show a single plane at full size. The bold contour marks $V = q/4\pi\varepsilon_0 R$; in the ring's side slice it crosses itself at the center, which shows the saddle.
- **Probe:** place it anywhere in 3D using the sliders or number boxes. You can also drag it in the 3D view: plain dragging moves it horizontally, and Shift-dragging moves it vertically. The readout shows $V$ and $\mathbf{E}$ at the probe.
- **Test charge:** choose same or opposite sign, and free motion, confinement to the plane, or confinement to the axis. Then either nudge it from the center or release it at the probe point. A live readout tracks its motion, and the status message reports the outcome: escape, collision with the ring, bounded oscillation (with the measured period), or no motion inside the shell.
- **The long journey to the ring:** how long does an opposite-sign charge released from rest take to reach the ring? Presets cover a stable axial orbit, an unstable one, a chaotic slingshot and the straight-in route. You can set the run time (up to $t = 3000$), the wire thickness and the playback speed. The section plots the path in the plane through the axis, $\rho(t)$ and $z(t)$, and a Floquet stability chart of the axial oscillation. The chart shows that small sideways offsets stay small for release heights between about $1.32R$ and $5R$; click it to release from any height.
- **Profiles:** the plots below the map show $V$ and $E$ versus distance from the center, along the axis and in the plane.

Units throughout the demo: $R = 1$ and $q/4\pi\varepsilon_0 = 1$, and the test charge has charge-to-mass ratio $\pm 1$.

## Building the note

```bash
pdflatex charged_ring.tex
pdflatex charged_ring.tex   # second pass resolves cross-references
```

Required packages: `amsmath`, `physics`, `tikz` (with `tikz-3dplot`), `pgfplots` (compat 1.18), `subcaption`, `hyperref`. All are included in a standard TeX Live or MiKTeX installation.

pgfplots cannot evaluate elliptic integrals, so the in-plane ring curves are embedded in the source as precomputed coordinates. They were checked against direct numerical integration.

## Implementation notes

- The demo is a single HTML file with plain JavaScript and Canvas 2D; there are no dependencies. It loads Google Fonts when online and falls back to system fonts otherwise.
- The ring's potential and field use closed-form elliptic-integral expressions, with $K$ and $E$ computed by the arithmetic–geometric mean. Near the axis, a series expansion is used to avoid cancellation errors.
- Test-charge motion uses a fourth-order Runge–Kutta integrator.
- The page follows the system's light or dark color scheme.

## Credits

- Idea and motivation: Sang Hoon Lee.
- Note, figures and interactive demo: written by Claude Opus 5.5 (Anthropic), in conversation with Sang Hoon Lee.
