# Rebuilding Overhead

> **Provenance note.** This file was reconstructed on 2026-09-05 from the build log, not from the
> original session, and the build pipeline itself was lost with that session's scratchpad. The
> page is complete and the recipe below is accurate as far as the log records it, but treat the
> parameter details as needing a check on first rebuild. The pipeline is likely recoverable with
> `catalog/tools/unsplice.py` — see step 7.

---

## 1. What you are building

A single self-contained HTML page: a live orbital census. 721 real satellites **propagated in the
browser** from actual orbital element sets — a draggable day/night globe with coastlines and
ground tracks, a time scrubber, an altitude-by-inclination census chart, and drill-down to the
six elements behind every number.

## 2. Get the data

Two-line element sets from **CelesTrak** (`https://celestrak.org`), pulled at build time.
The page ships a **sample**, not a census: 130 of 567 GEO objects, 420 of 10,721 Starlink, plus
the interesting minority in full. Say so on the page.

> **Licence check outstanding.** CelesTrak's redistribution terms have not been reviewed. The page
> ships derived output rather than raw element sets; confirm before republishing raw TLEs.

## 3. The central trick

A published Artifact cannot fetch at runtime. Rather than freezing satellite *positions* — which
would be stale within minutes — freeze the **orbital elements** and write a propagator:

- Solve Kepler's equation `M = E − e·sin E` by Newton iteration.
- Rotate perifocal → equatorial.
- GMST → latitude/longitude.

The page then stays live indefinitely with zero network. This is the whole idea; do not replace
it with baked positions.

Honest limits to state in-page: **two-body Keplerian, not SGP4** — no atmospheric drag, no J2
oblateness; spherical Earth; the globe's radial scale is log-compressed.

## 4. The page

- Draggable day/night globe with coastlines and ground tracks.
- Time scrubber at 60× / 600× / 3000×.
- Altitude × inclination census chart. **Objects on eccentric orbits must be drawn as
  perigee→apogee lines, not points** — otherwise NASA's four MMS spacecraft, Chandra and
  XMM-Newton (out past 172,000 km) read as a parse bug rather than the chart's best story.
- Filter chips, click-to-select, full drill-down to the six elements.
- **A human paragraph per object** — 58 description patterns covering what each thing is and what
  it measures, with heavy coverage of NOAA/EUMETSAT/JAXA weather and science missions.
- **Real launch IDs** derived from the international designator already in the TLE, including the
  quirk that ISS-deployed cubesats inherit the station's `1998-067` designator.
- **A Plain English toggle** that adds a plain sentence under every technical value *without*
  removing the raw number.

## 5. Verification

| Check | Expected |
|---|---|
| ISS altitude / speed / period / inclination | 416.9 km · 7.661 km/s · 93.0 min · 51.6312° |
| NOAA 20 | 827.7 km, 98.78°, correctly explained as sun-synchronous |
| Highest apogee in the set | >172,000 km (MMS constellation) |

If the ISS numbers are off, the propagator is wrong.

## 6. Notes

Methods type was originally set at 12.5px and flagged as too small; it is 14.5px now. Keep
methods text at a readable size.

## 7. Recovering the pipeline

```bash
python3 catalog/tools/unsplice.py projects/overhead/index.html --var DATA --out projects/overhead/src
```

That returns `template.html`, `payload.json` and an injector, verified by round-trip. What it
cannot return is the TLE fetcher — rewrite it against CelesTrak and verify by diffing the
regenerated payload against the recovered one.
