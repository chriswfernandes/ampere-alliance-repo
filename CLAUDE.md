# CLAUDE.md

Guidance for Claude / AI agents working in this repository.

## What this is

**Ampere Alliance — E&I Acquisition Pipeline** is a single-file, static web prototype
that presents and ranks a pipeline of electrical & instrumentation (E&I) acquisition
targets. It renders an interactive scoring model, a filterable/sortable target board,
and a Leaflet map, and lets users load their own target lists in the browser.

The entire app lives in one file: **`index.html`** (HTML + CSS + vanilla JS, no build step).

## Repository layout

```
.
├── index.html      # the entire application (markup, styles, and logic)
├── templates/      # downloadable upload templates (CSV / Markdown / JSON) + README
└── CLAUDE.md       # this file
```

The files in `templates/` mirror the inline `TPL` object in `index.html` (the
"Templates ▾" menu). If you change the template content or the data shape in one
place, update the other so they stay in sync.

There is intentionally **no** package manager, bundler, framework, or build pipeline.
Do not add one unless the user explicitly asks. Keep the project as a single
self-contained `index.html` so it can be dropped onto any static host.

## Running locally

Just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000/
```

External libraries load at view time from CDNs (an internet connection is required):
- Google Fonts (Space Grotesk, IBM Plex Sans, IBM Plex Mono)
- Leaflet 1.9.4 (map tiles + library) via unpkg

## Deployment

The site is deployed via **GitHub Pages** (Deploy from a branch → `main` / root) and
is live at:

```
https://chriswfernandes.github.io/ampere-alliance-repo/
```

Notes for agents:
- Do **not** add a `CNAME` file or set a custom domain unless asked. A previous
  account-wide custom domain (`chrisfernandes.ca`) caused project pages to redirect
  to a parked domain and 404; it was intentionally removed.
- Because Pages serves from the repo root, committing to `main` publishes the site.

## Architecture of `index.html`

Read the comment block at the top of `index.html` first — it documents the data
model and the three ways to add companies. Key regions, in order:

1. **Styles** (`<style>`): CSS custom properties under `:root` define the palette
   and spacing. Layout is a centered `.wrap` column with stacked `section.block`s.
2. **Markup** (`<body>`): hero + stat rail, the **scoring model** sliders, the
   **import panel** ("Load your own targets"), the **target board** + **map**, and
   a "how it works" section.
3. **Logic** (`<script>` near the bottom):
   - `seed` — the EDIT-ME array of default targets that ship with the page.
   - `FACTORS` / `DEFAULT_WEIGHTS` — the six scoring factors and their default weights.
   - State vars: `data`, `weights`, `provFilter`, `tierFilter`, `query`, `sortKey`, etc.
   - `composite(t)` — weighted score; `tierFor(s)` — buckets a score into tier 1/2/3.
   - `processed()` — applies search/filters/sort; `renderBoard()` / `renderMap()` —
     draw the list and map; `renderAll()` — recompute and redraw everything.
   - Import helpers parse **JSON, CSV, or Markdown tables** (`rowToTarget`, geocoding
     via the built-in `CITY`/province lookup and `getLatLng`).

### Data shape (per company)

```
name, city, prov, seg, rev,        // text
proven, aligned, scalable,         // scores 0–100
ambitious, succession, fit,        // scores 0–100
why,                               // one-line rationale
lat, lng                           // optional; else city is geocoded
```

Province codes: `BC AB SK MB ON QC NB NS PE NL YT NT NU` (and `US`).

### The six scoring factors

`fit` (Strategic fit), `scalable`, `proven`, `succession`, `aligned`, `ambitious`.
Weights are user-adjustable via sliders (0–40 each) and auto-normalized to 100%.

## Conventions & guardrails

- **Single file:** keep everything in `index.html`. Prefer editing inline CSS/JS over
  introducing new files.
- **Vanilla JS only:** no frameworks, no transpilation, no `npm` dependencies.
- **Style:** existing code is compact (multiple statements per line in places). Match
  the surrounding style; favor small, focused edits over reformatting whole sections.
- **Persistent target data** goes in the `seed` array (it loads for everyone). The
  import panel is session-only and not persisted.
- **Don't** add tracking, analytics, or external calls beyond the existing Fonts/Leaflet
  CDNs without being asked.

## Common tasks

- **Add/edit default targets:** modify the `seed` array; follow the data shape above.
- **Change scoring defaults:** edit `FACTORS` weights (`w`) or `DEFAULT_WEIGHTS`.
- **Add a city/province location:** extend the `CITY` lookup, or add `lat`/`lng` to a target.
- **Adjust tier thresholds:** edit `tierFor()`.
- **Restyle:** update the CSS custom properties in `:root`.

After edits, open the page in a browser to verify the board, sliders, and map still
render and re-score correctly.
