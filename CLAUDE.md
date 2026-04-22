# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static single-page forecast tool for Kemp's Ridley sea turtle (*Lepidochelys kempii*) arribada nesting likelihood at South Padre Island, TX. Displays a 7-day outlook derived from NWS Brownsville (KBRO) forecast data.

## Running Locally

No build step — open `index.html` directly in a browser or serve with any static file server:

```bash
python3 -m http.server 8080
```

## Architecture

Everything lives in `index.html`. There are three logical sections:

1. **CSS** (`<style>`) — design tokens via CSS custom properties (`:root`), component styles. Dark ocean-themed palette; `--teal`, `--amber`, `--red`, `--sand` are the four rating colors used throughout.

2. **Static HTML** — header, key messages callout, best-window callout, analysis grid, and footer are all hardcoded markup. Only the forecast card list (`#forecast`) is dynamically rendered.

3. **JavaScript** (`<script>`) — a single `days` array holds the forecast data. `forEach` renders each entry into `.day-card` elements appended to `#forecast`. No external JS dependencies.

## Forecast Data

The `days` array in the `<script>` block is the only place forecast data lives. Each entry:

```js
{ name, date, wind, cloud, score, tag, tagClass, barColor, note, highlight? }
```

- `score`: 0–100 nesting likelihood. Scoring combines wind speed (15–20 mph optimal; >25 mph gusts penalized), cloud cover (overcast preferred), sea state (inferred from gusts/rip current risk), and wind direction (SE/S = onshore = favorable).
- `tagClass`: one of `tag-poor`, `tag-low`, `tag-mod`, `tag-good`, `tag-best` — drives both badge color and bar color via `barColor`.
- `highlight: true` adds the teal-tinted `.highlight` class to the card.

## External Data Sources Referenced

Links in the "Field monitoring notes" analysis card point to:
- NDBC buoys PCGT2, BZST2, 42020
- NWS BRO Area Forecast Discussion
- Windy.com model viewer
- Pivotal Weather GFS cloud cover
