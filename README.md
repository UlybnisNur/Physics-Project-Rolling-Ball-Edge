# How Balls Roll Off Tables

Interactive Vite + React + TypeScript simulation based on:

M. E. Bacon, "How balls roll off tables," American Journal of Physics 73, 722-724 (2005), https://doi.org/10.1119/1.1947198

## Physics Model

The app compares three models for a ball leaving a table edge:

- Simple projectile model: the ball leaves immediately with horizontal speed `R * omega_i`.
- No-slip-only model: the ball rotates around the edge without slipping until `F_N = 0`.
- Full model: the ball starts in a no-slip phase, switches to slipping when `F_f / F_N = mu_s`, and leaves when `F_N = 0`.

The simulation uses a downward edge angle internally, but the displayed `theta/alpha` angle is the paper's angle measured from the horizontal.

## Equations Used

No-slip phase:

```text
F_N / m = -R * theta_dot^2 + g * sin(theta)
F_f / m = R * theta_double_dot + g * cos(theta)
F_f / m = -(2/5) * R * theta_double_dot
theta_double_dot = -(5/7) * (g/R) * cos(theta)
```

Slipping phase:

```text
F_N / m = -R * alpha_dot^2 + g * sin(alpha)
F_f / m = R * alpha_double_dot + g * cos(alpha)
F_f / m = mu_k * F_N / m
alpha_double_dot = -(g/R)(cos(alpha) - mu_k sin(alpha)) - mu_k alpha_dot^2
```

The final line is the dimensionally consistent form obtained from Bacon's Eqs. 5-9.

Lift-off:

```text
F_N = 0
```

The integration is fourth-order Runge-Kutta with a small fixed time step.

## Features

- Canvas animation of the ball rolling along the table, rotating around the edge, slipping, and entering projectile motion.
- Live values for phase, angle, angular velocity, normal force per mass, friction force per mass, friction ratio, final angular velocity, and landing distance.
- Controls for `omega_i`, `R`, `mu_s`, `mu_k`, table height, gravity, and animation speed.
- Model comparison panel for simple projectile, no-slip-only, and full no-slip plus slipping predictions.
- Kick detector comparing the full model with the simple projectile model.
- Threshold marker near `sqrt(g/R)`, which is about `28 rad/s` for the 2.5 cm diameter steel ball discussed in the paper.
- Graphs for angle vs time, angular velocity vs time, `F_N/m` vs time, and landing distance vs `omega_i`.
- Theory and report helper sections for a first-year physics project write-up.

## How To Run

If npm is installed:

```bash
npm install
npm run dev
```

Then open the local URL printed by Vite.

If Windows says `npm` is not recognized, open this file directly in a browser:

```text
standalone.html
```

The standalone file contains the same simulation in plain browser JavaScript and does not need a package install.

For a production build:

```bash
npm run build
npm run preview
```

## How This Supports The Report

The app shows why the simple projectile model is incomplete: for sufficiently low initial angular velocity, the edge remains in contact long enough to change the center-of-mass velocity and spin. The full model includes the static-friction limit, the required slipping phase, and the lift-off condition `F_N = 0`, so its predictions can be compared with measured landing distances or video-derived angular velocities.
