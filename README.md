# The Missing Map

A dark, single-file web tool for plotting unsolved missing-persons and no-body homicide
cases on a map and drawing a **network of threads** between related cases — the
investigator's string-board, laid over a real map.

Everything runs in your browser. **No server, no build step, no uploads.** Your data is
stored locally (in `localStorage`) and only leaves your machine if *you* export it.

![types: missing · homicide · unidentified · site of interest](https://img.shields.io/badge/cases-missing%20%C2%B7%20homicide%20%C2%B7%20unidentified%20%C2%B7%20site-8b3a3a)

---

## What it does

- **Map board.** Cases drop as colored pins on a dark basemap (CARTO dark matter).
  - 🟡 Missing · no recovery  🔴 Homicide · no body  🔵 Unidentified remains  🟢 Site of interest
- **Threads (the network).** Auto-draw red threads between cases that share traits —
  combine any of: *within a distance*, *within a time window*, *same type*, *shares a tag*.
  Rules combine with AND; distance is the spatial backbone.
- **Asserted links.** Turn on **Link mode** and click two pins to assert a connection by
  hand (brighter cyan thread), stored separately from the auto-rules.
- **Radius search.** Click **Radius**, drop a center on the map, set a mile/km radius —
  the board filters to cases inside the ring and draws it.
- **Filters.** By type, year range, tag, and full-text search across names/agencies/notes/tags.
- **Pin degree.** When threads are on, each pin shows how many connections it has, so dense
  clusters surface on their own.
- **Detail panel.** Click any pin for its full record and its asserted links.
- **Persistence + export.** Data persists between sessions; **Export** writes timestamped
  JSON + CSV copies (useful for keeping reproducible, dated snapshots of your dataset).

## Getting data in

There is no single feed for "all unsolved cases" — you assemble it from public sources.
Four ways to load:

1. **Import CSV / JSON** *(primary, always works)*. Click **CSV template** for the exact
   schema, fill it from your sources (e.g. NamUs, state clearinghouses, agency releases),
   and import. JSON exported from this tool re-imports as-is.
2. **Pull FBI cases.** Fetches missing + ViCAP/victim listings from the FBI Wanted API
   (`api.fbi.gov`, no key) and geocodes them via OpenStreetMap. The FBI API frequently
   blocks browser requests via CORS; this works most reliably when the page is served over
   https (e.g. GitHub Pages). If it's blocked, use CSV import.
3. **Add a case.** Click **Drop on map** to set coordinates, fill the form, **Add case**.
4. **Demo nodes.** Adds 8 clearly-fake `SAMPLE` nodes so you can see the thread network work
   before loading real data. Clear them anytime.

### CSV schema

```
name,type,date,lat,lng,age,sex,agency,url,tags,notes,approx
```

- `type`: `missing` | `homicide` | `unidentified` | `site` (free text is mapped on a best-effort basis)
- `date`: a year (`2018`) or ISO date (`2018-04-12`)
- `tags`: separate multiple with `;` (e.g. `downtown;last seen`)
- `approx`: `1`/`true` if the coordinate is approximate (recommended for geocoded points)

## Run it

- **Locally:** download `index.html` and open it in Chrome or Edge. (You need to be online —
  map tiles, fonts, and the map library load from a CDN.)
- **GitHub Pages:** see below. Hosting over https also makes the optional FBI pull work better.

## Deploy on GitHub Pages

1. Create a new repository and add these files (`index.html`, `.nojekyll`, `README.md`,
   `LICENSE`, `sample-data.csv`).
2. Push to the `main` branch.
3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   pick `main` / `/ (root)`, save.
4. Your site goes live at `https://<your-username>.github.io/<repo-name>/` within a minute or two.

The included `.nojekyll` file tells GitHub Pages to serve the files as-is.

## Privacy & responsible use

- **Local-first.** Nothing is uploaded by this tool. Data lives in your browser's
  `localStorage`. **Anything you commit to a public repo is public** — keep real
  case data in private CSVs you do not commit, not baked into `index.html`.
- **Leads, not verdicts.** Aggregated and crowdsourced records can be incomplete, outdated,
  or wrong. A pin or a thread is a prompt to verify, never a conclusion. Mark uncertain
  locations `approx`.
- **Respect sources.** Follow the terms of service and rate limits of any database you pull
  from. The OpenStreetMap/Nominatim geocoder used here is rate-limited to ~1 request/second.
- **Not affiliated** with the FBI, NamUs, OpenStreetMap, or any agency or database.

## Built with

[Leaflet](https://leafletjs.com/) · [CARTO basemaps](https://carto.com/basemaps/) ·
[OpenStreetMap](https://www.openstreetmap.org/) (geocoding) · vanilla HTML/CSS/JS, one file.

## License

MIT — see [LICENSE](LICENSE).
