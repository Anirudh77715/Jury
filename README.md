# GreenScore — Measure. Trust. Circulate.

A low-cost battery health passport for EV batteries in India. This repository holds the interactive prototype: one self-contained `index.html`, with no build step and no backend.

> **Prototype** · Demo estimator on simulated or uploaded data · Not validated on Indian battery packs · Not a safety certificate. Health estimate only — not a safety assessment.

## What the prototype does

- **Live health check.** Five simulated presets, a Custom mode and CSV upload run through *Demo estimator v0.1 (reference-curve fit)*. It returns a state-of-health estimate with a 90% band, or refuses when one of nine data-quality rules fails. The result card lists every rule as pass or fail and shows the arithmetic behind the number.
- **3D battery pack** (Three.js r128). It shows a charging animation, an exploded view and a health ring with the confidence band.
- **Battery passport.** Each record carries a QR code and a SHA-256 integrity hash. It also has a viewer mode, a verify button and a tamper test.
- **Model-results viewer.** Paste or drop JSON results from a trained model to get a parity plot, MAE, RMSE, max error and band coverage.
- **Light and dark themes**, keyboard access, and support for reduced motion.

## Deploy on Netlify

**Option 1: connect this repository.** In Netlify, choose *Add new site → Import an existing project*, then pick this GitHub repo. Leave the build command empty. `netlify.toml` already sets the publish directory to the repo root. Each push to `main` then redeploys the site.

**Option 2: drag and drop.** Open <https://app.netlify.com/drop> and drop this folder onto it.

`netlify.toml` also:

- serves `/lib/three.min.js`, `/lib/chart.umd.js` and `/lib/qrcode.min.js` from the pinned CDN builds, so the page's local-first loader never hits a 404. To self-host the libraries instead, put those three files in a `lib/` folder; real files take priority over the rewrites.
- adds basic security headers and sensible caching.

Unknown paths get the branded `404.html`.

## Run it locally

Open `index.html` in a browser, or serve the folder (phone QR scanning needs an `http(s)` address):

```bash
python -m http.server 8080
```

Then open <http://localhost:8080>.

## Handy URLs

| URL | What it does |
| --- | --- |
| `?selftest=1` | Runs all five presets without animation, logs a `console.table` and shows "Self-test: 5/5 behaved as expected" |
| `?preset=aged&autoplay=1` | Opens a preset and starts the session (`scooter`, `aged`, `rickshaw`, `short`, `noisy`, `custom`) |
| `?preset=custom&cfg=NMC_88_30_15_0.5_0.03_30` | Custom values: chemistry, true SoH %, start SoC %, minutes, C-rate, noise V, ambient °C |
| `?theme=light` / `?theme=dark` | Forces a theme for that visit |
| `#p=…` | Opens a battery passport in viewer mode (created by the page) |

The **Copy a link to this demo** button in the health check builds these links for you.

## CSV upload format

A header row with `time_s, current_A, voltage_V, temperature_C` (comma or semicolon separated). Current is positive while charging, and one row per second works best. The **Download sample CSV** button exports a valid example.

## Honesty notes

- Simulated data is labelled SIMULATED, goals are labelled TARGET and example people are labelled ILLUSTRATIVE.
- The in-browser estimator is a demo reference-curve fit, not the team's trained model. No field accuracy, partnerships or certifications are claimed.
- The integrity hash detects edits; it is not a legal signature.

## Sources

Autocar Professional (Vahan data) · IESA, FY26 · MeraEV 2026 (indicative) · RMI, 2024 · Ministry of Mines data · Industry estimate, 2026 · NITI Aayog, 2022 · Union Cabinet, Sep 2025 · MoRTH draft Battery Pack Aadhaar, Dec 2025

---

Prototype by Anirudh Kumar.
