# Loopline

A route generator for runners who don't know where to go — pick a distance, set a start point, and get a route that matches your terrain and lighting preferences.

**Live demo:** enable GitHub Pages on this repo (Settings → Pages) and it'll be served automatically from `index.html`.

## What it does

- Distance in km or mi, with quick-set buttons (5k, 10k, half, marathon)
- Start point via GPS, address/postcode search (with live autocomplete), or click-to-place on the map
- Terrain filter (flat / rolling / hilly), lighting filter (well-lit for night runs), loop vs out-and-back
- Real street/path routing on OpenStreetMap data via the Overpass API, biased toward genuine footways/cycleways over shared roads
- Elevation profile, lit-street percentage, and pace-based time estimate
- Falls back to an approximate, clearly-labeled synthetic street grid if live map data can't be reached, so the app stays functional either way

## Stack

Single self-contained `index.html` — no build step, no dependencies, no API keys required. Uses:
- **OpenStreetMap tile imagery** for the base map
- **Overpass API** for the street/path graph
- **Open-Elevation** for elevation data
- **Nominatim** for address search/autocomplete

All are free public services with light rate limits — fine for personal use or a demo, but for production traffic you'll want a paid tile provider (Mapbox/MapTiler) and to self-host or cache Overpass/Nominatim.

## Running locally

No build step needed — just serve the folder:

```bash
python3 -m http.server
```

Then open `http://localhost:8000`. (Opening `index.html` directly by double-clicking can break geolocation and some fetch requests in certain browsers — serving it, even locally, avoids that.)

## Known limitations

- Public Overpass/Nominatim servers can be slow or occasionally unreachable depending on network conditions — the app automatically falls back to an approximate street grid in that case (flagged clearly in the UI).
- No offline/embedded map data — everything is fetched live. A full country-wide offline dataset would require a backend service with a proper spatial database (PostGIS + pgRouting or a self-hosted Overpass instance), not a static client-side file.
