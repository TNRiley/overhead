# 🛰️ Overhead

**721 real satellites, propagated live in your browser from their orbital elements.**

→ **[Open it](https://tnriley.github.io/overhead/)**

A draggable day/night globe with coastlines and ground tracks, a time scrubber running up to 3000×, and an altitude-by-inclination census that exposes the hidden structure of the sky. Rather than freezing satellite positions, it ships the orbital elements and solves Kepler's equation in the page, so it stays live indefinitely with no network. Every number drills down to the six elements behind it, and objects on wild ellipses are drawn as perigee-to-apogee lines instead of points.

## Running it

One self-contained HTML file. No build step, no server, no network access at runtime — open `index.html` in a browser, or serve the directory with any static host.

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Rebuilding it from scratch

[REBUILD.md](REBUILD.md) is written for an LLM with a shell and nothing else: the data sources and their quirks, the processing decisions, the page's structure and interactions, and a table of expected values to check the result against.

## Source

The original build pipeline was not preserved. `index.html` is complete and self-contained, and the data route is documented well enough to regenerate it — see the sources below.

## Data

- **[CelesTrak two-line element sets](https://celestrak.org)** — check redistribution terms before re-hosting the data

Every figure on the page is computed from the data shipped with it. Check the page's own methods panel for how each number is derived and where it should not be pushed.

## Built with

vanilla JS, canvas, hand-written Keplerian propagator.

## Licence

Code is MIT (see [LICENSE](LICENSE)). Data keeps the licence of its source, listed above.

---

Part of [Quick Projects](https://github.com/TNRiley/quick-projects) — one self-contained thing, built in one session. First published 2026-09-04.
