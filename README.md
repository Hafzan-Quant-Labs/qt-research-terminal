# Hafzan QT Research Terminal

> A professional quantitative analytics environment — built entirely in a single HTML file.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Available-white?style=flat-square&labelColor=0A66C2)](YOUR_LIVE_LINK_HERE)
[![License](https://img.shields.io/badge/License-Proprietary-white?style=flat-square&labelColor=CC0000)](LICENSE)
[![Built With](https://img.shields.io/badge/Built%20With-HTML%20%7C%20CSS%20%7C%20JS-white?style=flat-square&labelColor=E34F26)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![Three.js](https://img.shields.io/badge/3D%20Engine-Three.js%20r128-white?style=flat-square&labelColor=049EF4)](https://threejs.org)
[![No Backend](https://img.shields.io/badge/Backend-None-white?style=flat-square&labelColor=2EA043)]()
[![No Framework](https://img.shields.io/badge/Framework-None-white?style=flat-square&labelColor=8B5CF6)]()

---

## Overview

**Hafzan QT Research Terminal** is a standalone, client-side quantitative analytics platform implementing four production-grade research tools used in institutional derivatives desks: a 3D implied volatility surface renderer, a Black-Scholes-Merton Greeks engine, a cross-asset correlation matrix analyser, and a stochastic Monte Carlo path simulator.

Every panel runs in the browser with zero server-side dependencies. No build system, no frameworks, no backend. A single `.html` file you can open in any modern browser and begin working immediately.

The terminal is designed to the aesthetic standard of institutional research infrastructure — pure black background, IBM Plex Mono typography, white text hierarchy, and topographic colour scales across all visualisations.

---

## Live Demo

🔗 **[Try it here → YOUR_LIVE_LINK_HERE](YOUR_LIVE_LINK_HERE)**

---

## Screenshots

| | |
|:---:|:---:|
| ![Screenshot 1](screenshots/screenshot-1.png) | ![Screenshot 2](screenshots/screenshot-2.png) |
| ![Screenshot 3](screenshots/screenshot-3.png) | ![Screenshot 4](screenshots/screenshot-4.png) |
| ![Screenshot 5](screenshots/screenshot-5.png) | ![Screenshot 6](screenshots/screenshot-6.png) |

---

## Features

### Panel 01 — Implied Volatility Surface
- Real-time 3D surface rendered via **Three.js r128**
- Interactive drag-to-rotate with inertial mouse control
- **50 × 40 grid resolution** across moneyness and expiry dimensions
- Three parametric models: **SVI/SABR approximation**, **Heston-inspired**, and **Pure SVI**
- Live parameter controls: ATM vol, put skew, smile curvature, term structure bump, vol-of-vol
- Topographic vertex colouring with 9-stop scientific colour scale
- Wireframe overlay for structural clarity
- Surface statistics panel: minimum IV, maximum IV, ATM slope, 1M/1Y term ratio

### Panel 02 — Options Greeks Engine
- Full **Black-Scholes-Merton** framework with continuous dividend yield
- Five Greeks computed: **Delta**, **Gamma**, **Theta**, **Vega**, **Rho**
- 2D heatmap across **80 × 70 strike-expiry grid**
- Proper `normCDF` and `normPDF` implementations (Abramowitz & Stegun approximation)
- Topographic isoline overlay with 12-level contour tracing
- Dual mode: **Call** and **Put**
- ATM indicator line with live Greek value at-the-money
- Colour scale with live min/max bar

### Panel 03 — Correlation Matrix Analyser
- **10-asset cross-asset universe**: SPX, NDX, TLT, GLD, CRU, BTC, EUR/USD, USD/JPY, HY CDX, VIX
- Four **market regimes**: Normal/Low Vol, Stress/Crisis, Post-Crisis Recovery, QE/Risk-On
- **Ledoit-Wolf shrinkage estimator** with adjustable shrinkage parameter λ
- Configurable estimation noise σ for simulation of estimation uncertainty
- Matrix statistics: average correlation, maximum off-diagonal, minimum off-diagonal, effective rank
- Effective rank computed as N / (1 + (N−1) × ρ̄²)
- Diverging colour scale: teal-green (positive) to red (negative)

### Panel 04 — Monte Carlo Simulation Engine
- Three stochastic processes: **GBM** (log-normal), **Merton Jump-Diffusion**, **CEV Process**
- Up to **500 simultaneous paths** with antithetic variates variance reduction
- Configurable: drift μ, volatility σ, time horizon T, jump intensity λ, jump size
- Percentile band rendering: 5th, 25th, median, 75th, 95th
- Terminal distribution statistics: E[S_T], 5th percentile, 95th percentile, P(profit), **CVaR at 5%**
- Path colouring by terminal outcome: green (gain > 15%), red (loss > 15%), neutral otherwise

---

## Mathematics

### Black-Scholes-Merton Greeks

The Greeks engine implements the continuous dividend yield extension of the BSM model. For a European call adjusted for continuous yield q:

```
d₁ = [ln(S/K) + (r − q + ½σ²)T] / (σ√T)
d₂ = d₁ − σ√T

Delta   = e^(−qT) · N(d₁)
Gamma   = e^(−qT) · N'(d₁) / (Sσ√T)
Theta   = −[Se^(−qT)N'(d₁)σ / 2√T] − rKe^(−rT)N(d₂) + qSe^(−qT)N(d₁)
Vega    = Se^(−qT) · N'(d₁) · √T / 100
Rho     = KTe^(−rT) · N(d₂) / 100
```

`normCDF` is computed using the Abramowitz & Stegun rational approximation (maximum absolute error < 1.5 × 10⁻⁷).

### Parametric Volatility Surface — SVI

The pure SVI parameterisation for the total implied variance w(k) = σ²(k)T:

```
w(k) = a + b · [ρ(k − m) + √((k − m)² + s²)]
```

where k = ln(K/F) is log-moneyness, and {a, b, ρ, m, s} are the SVI parameters mapped from the user controls.

### Monte Carlo — Merton Jump-Diffusion

The Merton (1976) jump-diffusion discretisation:

```
S(t+Δt) = S(t) · exp[(μ − ½σ²)Δt + σ√Δt · Z] + J(t)

J(t) = −ξ · S(t) · (1 + N(0, 0.3))   if U < λΔt
J(t) = 0                               otherwise
```

where Z ~ N(0,1), U ~ U(0,1), λ is jump intensity, and ξ is mean jump size. Antithetic variates are applied by negating Z for even-indexed paths.

### Ledoit-Wolf Shrinkage

The Ledoit-Wolf estimator shrinks the sample covariance matrix toward a structured target:

```
Σ̂_LW = (1 − λ) · Σ_sample + λ · Σ_target
```

λ is user-adjustable from 0 (no shrinkage) to 1 (full shrinkage toward identity).

---

## Technology Stack

| Component | Technology |
|---|---|
| 3D Rendering | Three.js r128 |
| 2D Charts | HTML5 Canvas 2D API |
| Typography | IBM Plex Mono (Google Fonts) |
| Styling | Pure CSS (custom properties, flexbox) |
| Computation | Vanilla JavaScript (ES6+) |
| Dependencies | Three.js only (CDN) |
| Build System | None |
| Backend | None |

---

## Browser Compatibility

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Safari 14+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Mobile browsers | ⚠️ Functional, not optimised |

WebGL must be enabled for the 3D volatility surface panel.

---

## Author

**Amaanullah Bhatti** (Hafzan Osmanoğlu)

Quantitative Research · Derivatives Analytics · Financial Engineering

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-white?style=flat-square&labelColor=0A66C2&logo=linkedin)](https://www.linkedin.com/in/amaanullah-bhatti/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-white?style=flat-square&labelColor=181717&logo=github)](https://github.com/amaanullah21)

---

## License

```
© 2026 Amaanullah Bhatti (Hafzan Osmanoğlu). All rights reserved.

This software and its source code are proprietary and confidential.
Unauthorised copying, distribution, modification, or use of this software,
in whole or in part, without the express written permission of the author
is strictly prohibited.
```
